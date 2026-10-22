# Color Palette

All colors below were extracted from the compiled Tailwind stylesheet and page
markup of https://917ventures.com. Names in the "Token" column are the site's own
Tailwind color names where one exists; names in *italics* are proposed token
names for colors the site only uses as arbitrary values. Machine-readable
tokens live in [../design-system/design-tokens.md](../design-system/design-tokens.md).

Contrast ratios are WCAG 2.x relative-luminance calculations against the listed
background. "AA" = ≥ 4.5:1 (normal text), "AA-large" = ≥ 3:1 (text ≥ 24px, or
≥ 19px bold, and UI component boundaries), "Fail" = < 3:1.

## Core palette (Verified)

| Role | Token | HEX | Where the site uses it | Contrast on white |
|---|---|---|---|---|
| Primary brand blue | `blue` | `#407DC3` | Primary buttons, active nav item, section titles, links, outline-button text/border, pagination pills | 4.25:1 (AA-large only) |
| Primary dark (gradient end) | `blue-2` | `#3A6DBA` | End stop of the brand gradient, footer CTA band, `border-blue-2` | 5.15:1 (AA) |
| Primary light (gradient start) | `blue-1` | `#65DDF5` | Start stop of the brand gradient | 1.59:1 (decorative only) |
| Text / "black" | `black` | `#040815` | Default body text color (`text-black` on the root wrapper), careers search input text | 19.99:1 (AA) |
| Surface | `white` | `#FFFFFF` | Page background, header, cards, buttons | — |
| Hero navy | *`navy`* | `#142741` | Homepage hero background and its video feather (`bg-[#142741]`) | White text on it: 15.04:1 (AA) |
| Light surface | `light-blue` | `#FAFBFF` | Footer background, light section backgrounds | — |
| Light purple surface | `light-purple` | `#F7F5FC` | Alternate light section background | — |

## Neutrals (Verified)

| Role | Token | HEX | Where the site uses it | Contrast on white |
|---|---|---|---|---|
| Dark grey text | `dark-grey` | `#323643` | Inactive navigation items | 12.04:1 (AA) |
| Grey text | `grey` | `#606471` | Form labels, secondary text | 5.90:1 (AA) |
| Light grey text | `light-grey` | `#8D919F` | Tertiary text, captions | 3.14:1 (AA-large only) |
| Placeholder | *`placeholder`* | `#667085` | Input placeholder text (`placeholder-[#667085]`) | 4.97:1 (AA) |
| Stat label | — | `#232323` | Uppercase labels under hero statistics | 15.72:1 (AA) |
| Input border | *`border-input`* | `#D0D5DD` | Default border of text inputs and textareas | 1.47:1 (non-text) |
| Focus border | *`border-focus`* | `#50A6D8` | Focused input border (2.5px); portfolio tile border (1px) | 2.70:1 (non-text) |
| Disabled control | *`disabled-surface`* | `#D4E0ED` | Disabled carousel arrow button background | — |
| Steel blue border | `soft-steel-blue` | `#8EA1BD` | Decorative borders | 2.63:1 |
| Secondary blue border | `secondary-blue` | `#52BCD5` | Card borders on the 917V Life page, testimonial card border | 2.21:1 |
| Skeleton base / highlight | — | `#EBEBEB` / `#F5F5F5` | react-loading-skeleton shimmer | — |
| Tailwind greys also present | `gray-100/300/400/500/800` | `#F3F4F6` `#D1D5DB` `#9CA3AF` `#6B7280` `#1F2937` | Occasional utility use | — |

## Semantic colors (Verified)

| Role | Token | HEX | Where the site uses it | Contrast |
|---|---|---|---|---|
| Success | `success-green` | `#34C759` | "EXITED" / "SPUN IN" ribbon badges on portfolio tiles (white bold text) | White on it: 2.22:1 (Fail) |
| Error | `error-red` | `#D92D20` | Required-field asterisk, validation text | 4.83:1 on white (AA) |
| Warning / amber | — | `#EA9430`, `#FFC767` | Found in the stylesheet (Velocity page accents); no semantic use observed | 2.39:1 / 1.54:1 |

**Proposed semantic extensions** (not on the site; needed for dashboards and admin tools). Chosen to pass AA as text on white and to sit comfortably next to the verified palette:

| Role | Token | HEX | Use |
|---|---|---|---|
| Success text / icon | *`success-700`* | `#1E7A3C` | Status text, success icons on white (5.4:1) |
| Success surface | *`success-50`* | `#E8F8EE` | Success alert/badge background with `success-700` text |
| Warning text / icon | *`warning-700`* | `#B45309` | Warning text on white (5.0:1) |
| Warning surface | *`warning-50`* | `#FFF4E0` | Warning alert/badge background |
| Error surface | *`error-50`* | `#FFF5F4` | Error alert/badge background with `error-red` text (4.5:1) |
| Info text / icon | *`info-700`* | `#3A6DBA` | Reuse `blue-2`; do not add a new blue |
| Info surface | *`info-50`* | `#EEF4FF` | Info alert, selected-row and active-nav background with `blue-2` text (4.7:1). The site stylesheet contains a similar `#E3EDFF`, which falls short of AA with small blue text, so the lighter value is proposed. |

Keep `success-green #34C759` for filled badges and ribbons where the text is
large/bold and decorative, as the site does; use `success-700` whenever green
must carry meaning as text on a light background.

## Brand gradients (Verified)

| Name | Stops | Direction on site | Used for |
|---|---|---|---|
| Brand gradient (`bg-brand-gradient`) | `#65DDF5` → `#3A6DBA` | top → bottom | Full-width brand backgrounds, decorative bands |
| CTA band | `#55C0D5` → `#3A6DBA` | top → bottom | Pre-footer "Join us in building futures" band |
| Stat gradient | `#DB6E4F` (`pink-1`) → `#DA4E8F` (`pink-2`) | left → right, clipped to text | Hero statistics ("46+", "5", "12") |
| Submit gradient (`.submit-gradient`) | `#8B81FF` → `#0B90FC` at 163.84° | diagonal | A gradient submit button (Velocity forms) |
| Planet section | `#81C5D3` → `#FFFFFF` 91% | top → bottom | About page background |
| Dark overlay | `rgba(4,8,21,.9)` → transparent | varies | Image overlays for white text |

Gradient rules:

- Gradient **backgrounds** always carry white text; never place body-size dark text on a brand gradient.
- Gradient **text** (`bg-clip-text`) is reserved for hero-scale numbers or display headlines (≥ 36px). Never use it for body copy, labels, or data values in tables.
- The coral/pink stat gradient is a highlight device, not a semantic color. It must not be used for errors, alerts, or negative deltas.

## Velocity sub-brand palette (Verified)

The `/velocity` event page layers purples over the core palette. Use these only
for Velocity-branded surfaces, never for core products.

`#8754FF`, `#6E12C5`, `#AD76FF`, `#B92DFF`, `#FF5EFF`, `#C07DFD`, `#552CA0`,
`#4A227C`, `#714888`, `#249FF7`, `#A0F9FF`, `#080C47` (headline navy),
`#A4A6BA` (muted text), `#110C20` (dark section). Velocity also uses italic DM Serif Display and gradient text such as `#FFFFFF → #C07DFD → #8754FF`.

## Usage rules

1. **One primary.** Interactive elements (buttons, links, active states, focus rings, selected tabs) use `blue #407DC3`. Do not introduce a second interactive color.
2. **Small blue text uses `blue-2`.** Because `#407DC3` is 4.25:1 on white, use it for text only when the text is ≥ 19px bold or ≥ 24px regular (headlines, buttons at 16px bold are borderline and verified on the site). For 12–16px regular links and labels in product UI, use `blue-2 #3A6DBA` (5.15:1). *(Proposed rule based on measured contrast; the site itself uses `#407DC3` for 16px bold nav text.)*
3. **Text hierarchy:** `black #040815` for primary text → `dark-grey #323643` for secondary → `grey #606471` for labels/meta. Use `light-grey #8D919F` only for non-essential text ≥ 19px bold, or for disabled text.
4. **Surfaces:** page = `#FFFFFF`; alternate sections and app shells = `light-blue #FAFBFF`; cards are always white on either.
5. **Borders:** `#D0D5DD` for form controls and table rules; `#50A6D8` for focus and for outlined feature tiles; `gray-300 #D1D5DB` is an acceptable Tailwind fallback for dividers.
6. **Navy `#142741`** is a hero/marketing color. In products, use it only for a dark app-shell sidebar or a dark report cover, with white text (15:1).
7. Never add Tailwind default `blue-500/600` (`#3B82F6`, `#2563EB`) as a brand substitute; `#2563EB` appears in the stylesheet only as a Tailwind form-reset default for focus rings.

## Related files

- [../design-system/design-tokens.md](../design-system/design-tokens.md) — CSS variables and Tailwind config for every token above.
- [../design-system/accessibility.md](../design-system/accessibility.md) — full contrast matrix and pairing rules.
- [../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md) — how the palette maps to amCharts series.
