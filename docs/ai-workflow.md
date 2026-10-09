# How I worked with AI

MakerHub was built with an AI coding assistant: Claude, through Claude Code. This page explains how that worked in practice. It is neither "AI built this for me" nor "I wrote every line": the code was written by the AI, and the product was mine.

## The split

| I decided | AI did, under my direction |
|---|---|
| What problem to solve and for whom | Writing and refactoring the code |
| What to build next, and what to remove | Debugging |
| How it should look and feel | Exploring options before committing to one |
| When something was good enough | Writing automated tests |
| What to ship, and when | Running structured reviews: accessibility, leftover code, design, security |

## The loop

Most work followed the same cycle:

1. **I described a need**, usually from using the app: *"when I open a design, the sidebar stops showing I'm in Library"*, *"the enlarged images look pixelated"*.
2. **The AI proposed and built a change**, explaining the trade-offs when there was more than one way to do it.
3. **I checked it in the running app**, not just in a description of it.
4. **I accepted it, pushed back, or changed my mind.** Sometimes the answer was to undo it: I asked to remove the Tags tab from Workshop and, shortly after, asked for it back.

## Rules I set

- **Measure, don't assure.** "It should work" wasn't an answer. Colour contrast, colour-blind separation and rendering cost were measured before deciding.
- **A fix needs a test that fails without it.** Otherwise there is no proof the bug is gone, and nothing stops it from coming back.
- **Check the real thing.** Screenshots from the running app, counts from the real data, not just passing tests.
- **Big or risky changes go on a separate branch**, so they can be thrown away if they don't convince me.
- **Explain decisions where they live.** Every non-obvious choice is documented next to it, so the reason survives the conversation that produced it.

## Mistakes, and how they were caught

The speed is real, and so is the confidence when something is wrong. A few examples:

- **A preview that looked pixelated.** I noticed it when enlarging files in the Inbox. The cause was previews drawn far smaller than the size they were shown at.
- **An animation that jumped across the screen** in the guided tour. I spotted it while trying the tour. Investigating it revealed a second, worse problem: highlight boxes piling up on screen with every step.
- **A background job that stopped halfway.** After fixing the previews, a check of the actual files showed that most of them had not been redrawn. The loop mistook "this batch did nothing" for "nothing is left".
- **A statistic divided by the wrong total.** A savings bar showed 59% where the real discount was 78%. It was caught by comparing the new figure with the old one before accepting it.

The pattern: problems I found by using the app, and problems that surfaced because I asked for evidence instead of reassurance.

## What I would tell someone starting the same way

- Use the product every week. The most valuable changes came from use, not from planning.
- Ask for numbers, screenshots and tests. Confidence isn't evidence.
- Treat removing features as part of the job. An AI will happily keep adding things; deciding what doesn't belong is the product work.
