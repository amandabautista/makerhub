# ADR 001: No security-scoped bookmarks

**Date:** 2026-08-27
**Status:** Accepted. Still in force (checked October 2026).
**Affects:** `watch_folders`, the `files` module

## Context

The initial data model planned a `watch_folders.bookmark BLOB` column to store a macOS *security-scoped bookmark*, so that access to watched folders would survive app restarts.

Before building that machinery, we needed to check whether our distribution model requires it.

## Research

From Electron 44's official type definitions (`node_modules/electron/electron.d.ts`):

- `app.startAccessingSecurityScopedResource(bookmarkData)` is annotated **`@platform mas`**. It only exists in Mac App Store builds.
- The `securityScopedBookmarks` option of `dialog.showOpenDialog` is documented as *"Create security scoped bookmarks **when packaged for the Mac App Store**"*, annotated `@platform darwin,mas`.
- The `bookmarks` / `bookmark` return fields are annotated `_macOS_ _mas_` and are only filled in when that option is on.

MakerHub is distributed as a **DMG outside the Mac App Store** (`electron-builder.yml` → `mac.target: dmg`). It does not use the App Sandbox.

## Decision

**MakerHub does not use security-scoped bookmarks, and the `watch_folders.bookmark` column is dropped.**

Bookmarks solve an **App Sandbox** problem: a sandboxed app only gets access to the files the user picked during the current session, and needs the bookmark to get it back after a restart.

Outside the App Sandbox, access to protected folders (Desktop, Documents, Downloads, external and network volumes) is governed by **TCC**. TCC permissions:

- are granted per app, identified by its bundle ID and signature;
- **persist across restarts**, stored in the user's TCC database;
- can only be revoked in System Settings → Privacy & Security.

In our distribution model, a bookmark would be a field that never gets filled in, consumed by an API that doesn't exist.

## Consequences

What does need doing, because it is the real problem:

1. **Add watched folders through `dialog.showOpenDialog`.** Besides choosing the path, this is what triggers the TCC permission request.
2. **Treat revoked permissions as an expected state, not a failure.** If the user revokes access in System Settings, reads start returning `EPERM` or `EACCES`. The scan must notice, mark the folder as inaccessible and tell the user exactly what happened and where to fix it, instead of failing silently or giving the files up for lost.
3. **Don't confuse "folder inaccessible" with "files deleted".** Files in a folder without permission are marked `is_missing` reversibly, never removed from the index.

## If MakerHub is ever distributed through the Mac App Store

Bookmarks would have to come back: turn on `securityScopedBookmarks` in the dialog, store the returned blob, and wrap each access between `startAccessingSecurityScopedResource()` and the stop function it returns. That would be a new migration adding the column, not a change to the existing model.

---

*Translated from Spanish in October 2026. The decision is unchanged.*
