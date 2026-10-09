# Product decisions

The choices that shaped MakerHub, what each one costs, and why I made it anyway. Most of them are about what the product refuses to do.

## 1. Local-first, with no account and no cloud

**Decision.** MakerHub runs entirely on the user's computer. There is no sign-up, no server and no sync.

**Why.** A personal library of files is exactly the kind of thing that should keep working when a service shuts down, changes its pricing or loses its connection. It also removes a whole class of privacy questions: there is nothing to leak because nothing leaves the machine.

**Trade-off.** No access from a second device. Moving to a new computer needed its own feature (see 7).

## 2. Never touch the originals

**Decision.** Adding a file to MakerHub never moves, renames, modifies or deletes it. Each file is either *linked* where it already lives or *copied* into a library folder. The choice is the user's, per file.

**Why.** People keep their downloads in folders that make sense to them. A tool that reorganises those folders without asking is a tool they stop trusting after the first surprise.

**Trade-off.** If a linked file is moved outside MakerHub, the app has to notice and help reconnect it, instead of simply owning the file.

## 3. What the file says is not what you say

**Decision.** Information read from a slicer project file (printer, print time, filament) is shown separately from anything the user typed, and never overwrites it.

**Why.** Automatic data is useful until it silently replaces a note someone wrote on purpose. Keeping the two apart means the app can read as much as it wants without ever destroying the user's own work.

**Trade-off.** Two places to look for similar information. The interface labels the automatic one clearly as coming from the file.

## 4. Look for files only when asked

**Decision.** MakerHub checks the watched folders when the user presses **Scan folders**, not continuously in the background.

**Why.** Predictability. Files appear when the user asks for them, and the app never goes through their folders on its own.

**Trade-off.** New downloads don't appear by magic. In practice a single click is a small price for knowing exactly when the app is working.

## 5. Hide complexity until it is needed

**Decision.** A design can hold several versions of a model, but until a design has more than one, the interface shows no version list and no version number.

**Why.** Most designs have exactly one version. Showing version controls everywhere would make the common case harder in order to serve the rare one.

## 6. Fewer options that actually differ

**Decision.** Remove choices that look different on paper but don't change anything in practice.

**Examples.**
- Five colour palettes became three, because several of them were hard to tell apart.
- A free-text "category" field was removed in favour of grouped tags. Nobody filled it in, and free text produced near-duplicates.
- The materials screen and the printer-compatibility section were removed after weeks of use showed they never changed a decision.
- Tags were limited to four per design, one for each question they answer: where it lives, what kind of thing it is, what it is about and what it is for. Unlimited tags make filtering useless.

**Why.** Every option is something to understand, maintain and get wrong. Removing one is a product decision, not a loss.

## 7. Moving to another computer is a feature

**Decision.** The whole library can be exported into one file and imported on a new machine, with help reconnecting folders whose paths have changed.

**Why.** A local-first app has to solve, deliberately, what a cloud app gets for free. Without this, changing computers would mean starting from zero.

## 8. Accessibility enforced by tests, not by good intentions

**Decision.** Text contrast (WCAG 2.2 AA), the size of the type scale, and how well status colours separate for colour-blind users are all checked automatically.

**Why.** An outside accessibility review found contrast problems early on. They were fixed, and then made impossible to reintroduce: a later change that breaks contrast fails a test instead of shipping.

**Trade-off.** Some colour choices are constrained. A warning colour that reads well on a light background, a dark one and to a colour-blind user is a narrower target than one that only has to look nice.
