# How MakerHub evolved

From the first commit on 27 August 2026 to version 0.2.2, in about six weeks.

## Late August 2026: foundations

The first days covered the core of the product:

- A library of designs, each grouping the files that belong together.
- Tags, search and a first visual identity, including the mascot, Nilo.
- Reading print data from slicer project files, and opening them in the slicer.
- The Inbox: finding new downloads in chosen folders and sorting them into designs.
- A print history with filament costs and first statistics.

An internal test build for macOS was ready within the first two days, and several rounds of quality review followed straight away.

## Early September: real use

Version 0.1.1 went to a second user. From here on, most changes came from using the app with real libraries rather than from a plan.

## 19 September: version 0.2

- **Accessibility.** After an outside review flagged readability problems, every text and control was brought to WCAG 2.2 AA contrast, with automated tests to keep it there.
- **A larger preview in the Inbox**, to decide what to do with a file without opening it elsewhere.
- **Spools that retire themselves** when a print leaves less than 3 g.
- **Moving to another computer**, with the whole library in one file.
- **Removed:** the materials screen and the printer-compatibility section, which weeks of use showed nobody needed.

## 20–21 September: version 0.2.1

- **A guided tour** that moves through the app and points at each part, starting with the one step every user needs: choosing a folder to watch.
- **Colours that mean different things now look different.** Warning, error and success were nearly the same colour for colour-blind users. They are now separated by lightness, not only by hue.
- **Three palettes instead of five**, with saved choices carried over to the closest survivor.
- **Four tags per design, one per group.** New tags are created inside a group, and renaming a suggested tag updates it in place.
- **Statistics redesigned, twice.** The first pass removed seven identical boxes; the second gave the page a real hierarchy and put the library's health on top. The full print log moved inside it.
- **Removed:** a free-text category field nobody used.
- **Two review passes before release:** one for leftovers from removed features, one for design consistency.
- **Sharper previews.** 3D model previews are now drawn at twice the resolution with smoothed edges, and existing ones are redrawn automatically.

## 8 October: version 0.2.2

- **Turning models in the Inbox preview**, to look at a 3D model from other angles before deciding what to do with it.
- **Previews the right way up.** Some 3D model previews were drawn upside down. They are now drawn correctly, and the existing ones are redrawn automatically, including files still waiting in the Inbox.
