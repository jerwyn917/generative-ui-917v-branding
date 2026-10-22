# Typography

Fonts are self-hosted through `next/font` on 917ventures.com and exposed as CSS
variables and Tailwind utilities. All families, weights and sizes below are
verified from the compiled stylesheet and rendered computed styles unless marked
**Proposed**.

## Font families (Verified)

| Role | Tailwind utility | CSS variable | Family | Loaded weights | Fallback (site) |
|---|---|---|---|---|---|
| Display / headings | `font-primary` | `--font-unbounded` | **Unbounded** | 200–900 (variable) | Arial (size-adjusted) |
| Body / UI (default) | `font-secondary-1` | `--font-montserrat` | **Montserrat** | 100–900 (variable) | Arial (size-adjusted) |
| Accent serif | `font-secondary-4` | `--font-dm-serif-display` | **DM Serif Display** | 400, used italic | Times New Roman |
| Loaded, no observed use | — | `--font-quicksand` | Quicksand | 300–700 | Arial |
| Referenced, not loaded in the main stylesheet | `font-secondary-3` | `--font-poppins` | Poppins | — | — |

Notes:

- The root wrapper carries `font-secondary-1 text-black`, so **Montserrat is the default for all text**, including buttons, navigation, forms, cards and the footer.
- **Unbounded** is used only for: the hero headline, uppercase section titles, and the large statistic numbers.
- **DM Serif Display** (italic) appears only on the Velocity event page. Do not use it in core products.
- Quicksand and Poppins are present in the build but have no verified role. **Do not use them** in new work; treat the brand as a two-family system.

All four are open-source Google Fonts. Load Unbounded and Montserrat as variable
fonts (via `next/font/google`, `@fontsource-variable/*`, or Google Fonts CSS).
Use `font-display: swap`, which the site uses.

**Proposed fallback stack** for non-Next projects:

```css
--font-display: "Unbounded", "Arial Black", Arial, sans-serif;
--font-body: "Montserrat", "Segoe UI", Roboto, Arial, sans-serif;
```

## Type scale (Verified sizes, with roles)

Sizes come from the Tailwind utilities and arbitrary values actually used on the site. Line heights are the rendered values.

| Role | Family / weight | Mobile | Tablet (md ≥ 768) | Desktop (xl ≥ 1280) | Line height | Case / color |
|---|---|---|---|---|---|---|
| Hero headline | Unbounded 500 | 24px | 32px (sm ≥ 640) | 44px | 1.5 (66px at 44px) | Sentence case, white on navy |
| Section title | Unbounded 700 | 24px (`text-2xl`) | 30px (`text-3xl`) | 36px (`text-4xl`) | 1.1–1.33 | UPPERCASE, `blue #407DC3`, centered |
| Statistic number | Unbounded 700 | 22px | 36px | 36px | 1.5 | Pink gradient text |
| Page title (inner pages) | Montserrat 700 | 26px | 40px | 48px | 1.2 | UPPERCASE on hero image, white or `#080C47` |
| Card heading | Montserrat 700 | 20px | 20px | 20px | 1.4 | e.g. "Contact Us" |
| News card title | Montserrat 700 | 16px | 16px | 18px (`text-lg`) | 1.5–1.78 | Sentence case |
| Body | Montserrat 400 | 16px | 16px | 16px | 24px | `#040815` |
| Body emphasis | Montserrat 500/600 | 16px | 16px | 16px | 24px | Used for intro paragraphs and nav |
| Statistic label | Montserrat 700 | 16px (sm: 14px) | 16px | 16px | 24px | UPPERCASE, `#232323` |
| Button label | Montserrat 700 | 16px | 16px | 16px | 24px | Sentence case |
| Navigation item | Montserrat 500 (active: 700) | 16px | 16px | 16px | 24px | `dark-grey`; active `blue` + underline |
| Small / meta | Montserrat 300–500 | 12px (`text-xs`) | 14px (`text-sm`) | 14px | 18–20px | `grey #606471` |
| Form label | Montserrat 400 | 12px | 12px | 12px | 16px | `#040815` or `grey` |
| Input text | Montserrat 400 | 14px | 14px | 14px | 20px | `#040815`; placeholder `#667085` |
| Badge / ribbon | Montserrat 700 | 8px | 11px (sm) | 11px | 1 | UPPERCASE white on green |

Tracking: the site uses default letter spacing everywhere except `tracking-tight`
(-0.025em) on one Velocity display heading. Do not add letter spacing to
Unbounded headlines; the face is already wide.

## Proposed product type scale

Marketing sizes are too large for dense dashboards. For admin portals, tables and reports use this scale, which keeps the same families and weights:

| Token | Size / line height | Family / weight | Use |
|---|---|---|---|
| `display` | 32px / 40px | Unbounded 700 | Report cover titles, landing hero inside an app |
| `h1` | 24px / 32px | Unbounded 600 | Page title in app shell |
| `h2` | 20px / 28px | Montserrat 700 | Section/card titles |
| `h3` | 16px / 24px | Montserrat 600 | Sub-sections, table captions, widget titles |
| `body` | 14px / 20px | Montserrat 400 | Default product body, table cells, form inputs |
| `body-lg` | 16px / 24px | Montserrat 400 | Long-form text, dialogs, marketing-style blocks |
| `label` | 12px / 16px | Montserrat 500 | Form labels, column headers (sentence case; uppercase only with +0.04em tracking) |
| `caption` | 12px / 16px | Montserrat 400 | Helper text, timestamps, chart axis labels |
| `kpi` | 28–36px / 1.2 | Unbounded 700 | Numbers in summary metric cards (flat `black` or `blue`; gradient text only on cover pages) |
| `mono` | 13px / 20px | ui-monospace, "SF Mono", Menlo, monospace | IDs, code, hashes (Proposed; the site has no monospace) |

Rules:

- Headlines at `h1` and above use Unbounded; everything from `h2` down uses Montserrat. Unbounded is heavy and wide, so never set it below 20px or in long sentences.
- Use **tabular numerals** (`font-variant-numeric: tabular-nums`) for numbers in tables, KPI cards and chart labels so columns align. Montserrat supports this.
- Body text in products is 14px; never go below 12px for readable text.
- Minimum line height is 1.4 for body, 1.2 for headlines.
- Keep paragraph measure between 45 and 75 characters.

## Implementation

Tailwind (matching the site's naming, plus proposed aliases):

```js
// tailwind.config.js (excerpt) — see design-system/design-tokens.md for the full file
fontFamily: {
  primary: ['var(--font-unbounded)', 'Arial', 'sans-serif'],      // Verified name
  'secondary-1': ['var(--font-montserrat)', 'Arial', 'sans-serif'], // Verified name
  display: ['var(--font-unbounded)', 'Arial', 'sans-serif'],      // Proposed alias
  body: ['var(--font-montserrat)', 'Arial', 'sans-serif'],        // Proposed alias
},
```

Next.js:

```ts
import { Unbounded, Montserrat } from 'next/font/google';
export const unbounded = Unbounded({ subsets: ['latin'], variable: '--font-unbounded', display: 'swap' });
export const montserrat = Montserrat({ subsets: ['latin'], variable: '--font-montserrat', display: 'swap' });
// apply `${unbounded.variable} ${montserrat.variable} font-secondary-1 text-black` on the root element
```

## Related files

- [color-palette.md](color-palette.md) — text color tokens.
- [../design-system/design-tokens.md](../design-system/design-tokens.md) — full token file.
- [../design-system/accessibility.md](../design-system/accessibility.md) — minimum sizes and contrast for text.
