# MakerHub brand identity

The name, type, colour, icons and mascot that make MakerHub look like itself. Colour tokens and the theme architecture are described in more depth in [ADR 002](adr/002-visual-system.md).

## 1. Name

The visible name is **MakerHub**, with the tagline *3D design library*. Both live in one place: `src/shared/branding.ts`.

Two things share the same text and must not be confused:

| | Where | Can it change? |
| --- | --- | --- |
| **Visible name** | `src/shared/branding.ts` | Yes, that is what it is for |
| **Application identifier** | `app.setName()`, `appId`, `productName`, package name | **No** |

The identifier decides where the user's data lives on disk. Changing it would make the app lose its own library.

## 2. Typography: Inter

Inter is bundled with the app and never loaded from the web: MakerHub works offline.

- **Variable font.** One file covers weights 100 to 900.
- **Two subsets**, in `src/renderer/src/assets/fonts/`: `latin` (47 KB) for English and Spanish, and `latin-ext` (83 KB), loaded only when a file name needs it. Model files come from all over the world.
- **OpenType features** `cv05`, `cv08` and `ss03` are switched on for legibility, and numbers use tabular figures so they line up.
- Licensed under the SIL Open Font License 1.1, included next to the font files.

**Type scale.** Every text picks a *role*, not a number, and nothing goes below 12 px. `tests/typography.test.ts` blocks loose sizes from coming back.

| Role | Size | Used for |
| --- | --- | --- |
| caption | 12 px | Badges, counters, footnotes |
| small | 13 px | Field labels, help text, metadata |
| body | 14 px | Names, paragraphs, buttons, fields |
| lead | 15 px | Introductions |
| title | 17 px | Section titles |
| heading | 19 px | Dialog titles |
| display | 24 px | Page titles |
| hero | 36 px | The one figure that opens a screen |

Hierarchy comes from the type, not from giant headlines. No all-caps, no decorative fonts.

## 3. Colour

**Three themes:** Light, Dark, and System, which follows macOS.

**Three palettes**, down from five. Each one pairs a main colour with a quieter second accent:

| Palette | Light: main / accent | Dark: main / accent |
| --- | --- | --- |
| **Forest**: deep green, more contrast | `#294536` / `#90523A` | `#8FB49C` / `#D7A08A` |
| **Terracotta**: earth tones up front | `#8F533B` / `#4A6956` | `#D49B80` / `#9FBFA9` |
| **Rose**: dusty pink, warm and soft | `#964A63` / `#7A5E2F` | `#D9A3B6` / `#D4B07A` |

In dark mode every palette is lightened, or it would not have enough contrast on charcoal.

**Colours that mean something must look different.** Success, warning and error are separated by lightness as well as by hue, so they stay distinct for colour-blind users. All text meets WCAG 2.2 AA contrast. Both rules are checked automatically by `tests/contrast.test.ts` and `tests/color-meaning.test.ts`, and `tests/theme-tokens.test.ts` keeps the light and dark token sets in sync.

## 4. Icons: Lucide

Every icon goes through `src/renderer/src/components/Icon.tsx`, which sets size and stroke once. No component renders a `lucide-react` icon directly: mixed sizes and stroke weights appear exactly when each screen decides on its own.

- Sizes: `sm` 14 px, `md` 16 px, `lg` 20 px.
- Stroke width 1.75. Lucide's default of 2 looks too heavy next to Inter at these sizes.
- Icons inherit `currentColor`, so they follow the theme and palette.
- An icon shown on its own, without a visible label, needs an accessible `label`.
- Icons support labels; they don't replace them when the meaning could be ambiguous.

## 5. Nilo, the mascot

**Nilo** is a snail whose shell is a spool of filament. The spiral of the shell and the filament wound on its spool are the same shape: that is the whole joke, and it works because it doesn't need explaining.

### Why a snail

3D printing is slow, and MakerHub's weakest moments are waits: importing, hashing files, drawing previews. A snail is in no hurry. It turns slowness into calm instead of frustration. **The mascot has a job**: keeping people company while they wait.

### Where the illustrations come from

The concept, personality and creative direction are Amanda Bautista's. The illustrations were **generated with AI (ChatGPT)** from that direction; they are not hand-drawn. [CREDITS.md](../CREDITS.md) describes their origin and the terms that apply to them.

### How Nilo appears

- **Always with words.** Nilo never carries meaning on his own. The sidebar reads "Nilo, MakerHub's snail", which replaced "Powered by Nilo": read alone, that sounded like a technical credit ("Powered by Electron") rather than the name of a character.
- **Where there is room for him.** Empty states, waits, errors, and quietly in a corner of some screens. He never covers or competes with content.
- **One pose per situation.** The official set has 20 illustrations; Settings › *Who is Nilo* shows his 17 poses. Some examples:

| Pose | Where |
| --- | --- |
| Logo, facing forward | The brand in the sidebar |
| Next to an empty bookcase | An empty library |
| In the workshop, with goggles and calipers | Workshop |
| Next to the printer | Print history |
| Hugging a spool | No spools added yet |
| Painting a picture | Milestones you write yourself |
| Celebrating | A milestone reached, the only pose that expresses joy rather than work |
| Holding a finished piece | Favourites |
| Sad, next to a broken print | Errors |

### Technical rules

- **Mixed system, decided inside the component** (`src/renderer/src/components/Nilo.tsx`), so no screen has to know:
  - From 96 px up, the official illustration is used.
  - At 28 px, a drawn vector version is used instead. The illustrations have goggles, leaves and shadows that become a smudge at that size, and the vector drawing follows the theme and palette colours.
- **A plate in dark mode.** On charcoal, the illustrations' outline drops from 19.3:1 to 1.4:1 contrast and disappears. Each illustration therefore sits on a light, warm plate, which also makes Nilo an object inside the interface rather than a sticker on top of it. In light mode the plate is barely visible.
- Like the rest of the interface, any motion respects `prefers-reduced-motion`.

## 6. What we don't do

- Literal wood, paper or clay textures. The reference is the colour and the feeling, not decoration.
- Saturated badges, pill-shaped buttons, heavy shadows.
- Celebrating routine operations. Reaching a milestone is the only celebration.
