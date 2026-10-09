# MakerHub

A local-first desktop library for 3D printing. It keeps the models you download, the prints you make and the filament you use in one place, on your own computer.

> Personal project · macOS · Source code not public · Built with AI-assisted development

![The MakerHub library](screenshots/01-library.png)

## Overview

People who 3D-print at home collect files fast. Models arrive from different sites as STL, OBJ or 3MF files, often several per design, and pile up in the Downloads folder. The slicer knows how long a print took and how much filament it used, but nothing keeps that history, and nothing tells you how much is left on each spool.

MakerHub turns that pile into a library. It finds new files in the folders you choose, helps you sort them into designs, reads the print data that slicer project files already contain, and keeps a log of what you printed, what it cost and what is left on each spool.

Everything stays on your machine. There is no account and no cloud, and your original files are never moved, renamed or modified.

## Why I built it

My downloads folder held hundreds of 3D models, and I had lost track of them: I didn't know what I had, which file was the right one, or what I had already printed. I couldn't find a tool that did this without an account or a cloud, so I decided to build one.

I also wanted to find out how far I could take a real product, from a personal need to an installable app, by pairing my product skills with an AI coding assistant.

## Key features

- **An inbox for new downloads.** Point MakerHub at the folders where models land. It scans when you ask, flags duplicates, suggests which design a file belongs to, and lets you preview each file at full size, turning STL and OBJ models to see them from other angles, before deciding what to do with it.
- **A library organised by design.** A design groups the files that belong together. Tags are grouped by the question they answer: where it lives, what kind of thing it is, what it is about and what it is for. Collections and favourites sit alongside.
- **Your files stay where they are.** Each file is either linked where it already is or copied into a library folder. MakerHub never moves or deletes the original.
- **Print data from project files.** For sliced 3MF projects, MakerHub shows the printer, nozzle, plate type, print time and filament, kept clearly separate from anything you typed.
- **A print log that does the maths.** Logging a print fills itself in from the project file and takes the grams off the spool you used. Spools move to "Used up" on their own.
- **Statistics.** Prints, filament, printer time and cost; spending and savings on spools; monthly history; most-printed designs; and the health of the library.
- **Milestones.** Goals for the library, some of which MakerHub ticks off by itself.
- **A guided tour.** A first-run walkthrough that moves through the app and points at each part.
- **Moving to another computer.** Export everything into one file and bring it back on a new machine, with help reconnecting your folders.
- **Built for daily use.** Light and dark themes, three colour palettes, automatic backups and integrity checks. It works fully offline.

## Tech stack

Electron · React · TypeScript · Tailwind CSS · SQLite · Vite · TanStack Query · Zod · Node.js built-in test runner

A native macOS desktop app that runs entirely offline.

## AI-assisted development

I'm a project manager, not a software engineer, so I want to be precise about how MakerHub was built.

The code was written with an AI coding assistant, Claude through Claude Code, working from my direction. The split looked like this.

**What I owned:** the problem and the product. What MakerHub should do, for whom and in what order; every scope decision, including what to remove; the experience and the visual direction; using the app every week, finding what was wrong, and deciding when something was finished.

**What AI accelerated:** prototyping and implementing features, refactoring, debugging, exploring alternatives before committing to one, writing automated tests, and running structured reviews for accessibility, leftover code, design consistency and security.

**How I kept it honest:**

- Changes were checked in the running app, not only in theory.
- Where a decision could be measured, it was: colour contrast, colour-blind separation, the cost of rendering previews.
- Fixes came with a test that fails without the fix.
- The AI's mistakes were part of the process: a background job that stopped halfway, a statistic divided by the wrong total, an animation that jumped across the screen. I caught them by using the app every week and by asking for measurements instead of assurances. See [docs/ai-workflow.md](docs/ai-workflow.md).

## Product and development process

1. **Start from a real need.** The first version covered the core loop: find new files, sort them into designs, see them in a library.
2. **Use it, then iterate.** I used MakerHub every week with my own library and shared builds with a second user. Every round of real use produced the next list of changes.
3. **Ship small, versioned releases.** 0.1, then 0.2, 0.2.1 and 0.2.2. Each delivered build is tagged, so it can always be rebuilt exactly.
4. **Review before releasing.** Accessibility (WCAG 2.2 AA), leftover code from removed features, design consistency and security became regular steps, not one-off clean-ups.
5. **Cut what does not earn its place.** Several features were removed once real use showed they didn't change any decision.

MakerHub was built over about four weeks in August and September 2026, and reached nearly 800 automated tests by version 0.2.1.

## Design decisions

A few of the decisions that shaped MakerHub:

- **Local-first and offline.** A personal library should not depend on an account or a server. Nothing leaves the computer.
- **Never touch the originals.** People need to trust that adding a file to MakerHub cannot damage it.
- **What the file says is not what you say.** Data read from a project file is shown separately and never overwrites the user's own notes.
- **Look for files when asked.** MakerHub scans only when you press Scan. It is predictable, and nothing runs behind your back.
- **Hide complexity until it is needed.** Designs can have versions, but until a design has more than one, the interface shows no version list and no version number.
- **Fewer options that actually differ.** Five colour palettes became three. A free-text category field gave way to grouped tags. The materials screen and the printer-compatibility section were removed after weeks of use showed nobody needed them.
- **Accessibility enforced by tests.** Contrast ratios, and how well status colours separate for colour-blind users, are checked automatically, so a later change cannot quietly break them.

More context on each decision in [docs/product-decisions.md](docs/product-decisions.md), and how the product changed release by release in [docs/evolution.md](docs/evolution.md).

## Visual identity

Nilo, the snail whose shell is a spool of filament, was conceived as part of the product's visual identity. The concept, personality and creative direction are mine; the illustrations were generated with AI (ChatGPT) from that direction. They are not hand-drawn. See [CREDITS.md](CREDITS.md) for details.

The full brand guide, from the type scale to the three palettes and the rules for Nilo, is in [docs/branding.md](docs/branding.md).

## Screenshots

| | |
|---|---|
| ![Library](screenshots/01-library.png) | ![The same library in dark mode](screenshots/12-library-dark.png) |
| Library | The same library in dark mode |
| ![A design: tags, files and project data](screenshots/02-design.png) | ![A design: versions and print history](screenshots/02b-design-history.png) |
| A design: tags, files and project data | Versions and print history |
| ![Inbox](screenshots/13-inbox.png) | ![Inbox preview](screenshots/03-inbox-preview.png) |
| Inbox: new files waiting to be sorted | Inbox preview, turned to face you |
| ![Statistics](screenshots/04-statistics.png) | ![Prints](screenshots/09-prints.png) |
| Statistics | Every print, newest first |
| ![Spools](screenshots/05-spools.png) | ![Collections](screenshots/07-collections.png) |
| Spools, with what is left on each | Collections |
| ![Milestones](screenshots/10-milestones.png) | ![Guided tour](screenshots/06-guided-tour.png) |
| Milestones | Guided tour, first step |
| ![Settings](screenshots/11-settings.png) | ![Favourites](screenshots/08-favourites.png) |
| Settings, with Nilo's poses | Favourites |

*Screenshots show my own library. The 3D models in it belong to their creators: see [CREDITS](CREDITS.md).*

## Demo

https://github.com/user-attachments/assets/8f2fd719-6198-466b-b164-9f4c4132e6ca

A 66-second walkthrough of MakerHub, recorded with my own library. No sound: the captions carry the story. Some frames from it:

| | | |
|---|---|---|
| ![Search](demo/stills/01-search.png) | ![Tags](demo/stills/02-tags.png) | ![Versions and prints](demo/stills/03-versions-and-prints.png) |
| ![Inbox preview](demo/stills/04-inbox-turn.png) | ![Statistics](demo/stills/05-statistics.png) | ![Dark mode](demo/stills/06-dark-mode.png) |

## What I learned

- **Removing things is a product decision.** Some of the most useful changes took features away. Fewer options that genuinely differ beat many that don't.
- **Measure before deciding.** Contrast, colour-blind separation and performance were measured, not guessed. Several times the measurement contradicted what looked fine.
- **"Done" needs evidence.** A fix is not finished until there is a test that would have caught the bug.
- **AI is fast at writing code, and just as fast at being confidently wrong.** The speed is real. So is the need to check every result in the running app.
- **Real use beats specifications.** The most valuable feedback came from using the app every week, not from planning it.

## Disclaimer

MakerHub is a personal project, and this repository is a case study: the full source code is not published. Screenshots and the demo show my own library.

MakerHub is not affiliated with or endorsed by Bambu Lab or any 3D-model platform.

Text and screenshots © 2026 Amanda Bautista. All rights reserved. For the illustrations and third-party assets, see [CREDITS.md](CREDITS.md).
