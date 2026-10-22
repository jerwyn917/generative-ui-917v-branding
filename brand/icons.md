# Icons

## What the website uses (Verified)

917ventures.com has no icon font and no large icon library. Observed icons:

| Icon | Source | Spec |
|---|---|---|
| Hamburger (`bars-3`) and close (`x-mark`) in the mobile header | **Heroicons v2, 20px "solid" set** — identifiable by `viewBox="0 0 20 20"`, `fill="currentColor"`, `data-slot="icon"`, and path data matching Heroicons | Rendered at `w-6` (24px), inherits text color |
| Carousel previous / next arrows | Inline SVG chevrons inside 24px circular buttons | `w-6 h-6 rounded-full bg-blue text-white`, disabled `bg-[#D4E0ED] text-blue` |
| Facebook, Instagram, LinkedIn | Custom SVG files (`/facebook-icon.svg` etc.) | 32 × 32, glyph `#272E38` on a white rounded square (`rx="9.14"`) |
| Quotation mark | Custom SVG (`/quote-icon.svg`) | 58 × 43 decorative glyph |
| Email / LinkedIn contact icons on the Partner page | Small white glyphs in rounded squares on the gradient banner | ~32px |

So the site uses **Heroicons** for functional UI and **bespoke SVGs** for brand/social marks. Only two Heroicons appear, so the following system is a **Proposed standard** that extends what is verifiably in use.

## Standard icon library (Proposed, based on verified Heroicons usage)

**Library:** [Heroicons v2](https://heroicons.com) — MIT licensed, by the Tailwind team, matching the site's Tailwind stack.

Install:

```bash
npm install @heroicons/react      # React / Next.js
# or: npm install @heroicons/vue   # Vue
# or copy SVGs from heroicons.com for plain HTML
```

Import by set:

```tsx
import { MagnifyingGlassIcon } from '@heroicons/react/24/outline'; // default UI icon
import { CheckCircleIcon } from '@heroicons/react/20/solid';        // dense/inline icon
import { ChartBarIcon } from '@heroicons/react/24/solid';           // filled emphasis
```

### Sets and sizes

| Set | Size | Stroke | Use for |
|---|---|---|---|
| `24/outline` | 24 × 24, rendered at 20–24px | 1.5px | Default: navigation, sidebar items, toolbar actions, empty-state illustrations (at 48px), card headers |
| `20/solid` | 20 × 20, rendered at 16–20px | filled | Inline with 14px text, table row actions, badges, status dots, form field adornments, dropdown chevrons (the site's own header icons are this set) |
| `24/solid` | 24 × 24 | filled | Selected/active state of a nav item, filled emphasis on a primary action |
| `16/solid` | 16 × 16 | filled | Only inside 12px text or tight chips |

Rules:

- Pick **one set per context**: outline for a sidebar, solid for a table. Do not mix outline and solid icons in the same row, toolbar, or menu.
- Size icons with Tailwind `h-5 w-5` (20px) next to 14–16px text and `h-6 w-6` (24px) in navigation and buttons; never scale an icon with CSS `transform`.
- Keep the default 1.5px stroke; do not override `stroke-width`.

### Color

- Icons inherit `currentColor`. Set color on the parent text, not on the SVG.
- Neutral icons: `dark-grey #323643` on white; `grey #606471` for secondary; `light-grey #8D919F` only when disabled.
- Interactive icons: `blue #407DC3` when active/selected/hovered; `white` on `bg-blue` buttons.
- Status icons use the semantic tokens in [color-palette.md](color-palette.md#semantic-colors-verified): `success-700`, `warning-700`, `error-red`, `blue-2` for info.
- Icon containers (Verified pattern from the social icons and carousel buttons): 32px or 40px square or circle, white or `blue` background, 8px radius or `rounded-full`. Use `bg-light-blue #FAFBFF` or `#EEF4FF` with a `blue` icon for soft "feature" icons.

### Usage by context

| Context | Guideline |
|---|---|
| Navigation / sidebar | `24/outline`, 24px, left of the label with 12px gap; active item switches to `24/solid` in `blue`. Icon-only collapsed sidebars need a tooltip and `aria-label`. |
| Buttons | Icon left of the label, 20px, 8px gap. Icon-only buttons are at least 40 × 40px with `aria-label`. |
| Actions in tables | `20/solid`, 16–20px, inside a 32px ghost button; show on row hover but keep keyboard-focusable. |
| Status indicators | Pair the icon with a colored text label or badge; never rely on color or icon alone (`CheckCircleIcon` + "Active"). |
| Data displays / KPI cards | One optional icon at 24px in a 40px tinted container, top-right or left of the title; delta arrows use `ArrowTrendingUpIcon` / `ArrowTrendingDownIcon` at 16px in `success-700` / `error-red`. |
| Empty states | `24/outline` at 48px in `light-grey`, or a brand illustration (see [logo-and-assets.md](logo-and-assets.md)). |
| Charts | No icons inside amCharts plots; icons may appear in the chart card header or legend toggles only. |

### Brand assets vs. library icons

- Use the **brand assets** (logo mark, social SVGs, quote glyph) for identity and social links.
- Use **Heroicons** for every functional glyph (search, filter, settings, close, chevrons, status).
- Do not hand-draw replacements for icons Heroicons already provides; do not pull in Font Awesome, Material Icons, Lucide, Tabler or emoji alongside Heroicons. If an icon is genuinely missing (e.g. a product-specific glyph), draw it on the 24px grid with a 1.5px stroke and store it under `components/icons/`.

### Accessibility

- Decorative icons: `aria-hidden="true"` (Heroicons React components set this by default).
- Meaningful icon-only controls: `aria-label` on the button, plus a visible tooltip on hover/focus.
- Maintain 3:1 contrast between an icon and its background when the icon conveys state.

## Related files

- [../design-system/components.md](../design-system/components.md) — where icons appear in buttons, nav, badges and alerts.
- [../design-system/interaction-states.md](../design-system/interaction-states.md) — icon color per state.
