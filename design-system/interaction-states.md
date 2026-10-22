# Interaction States

Verified states come from the site's Tailwind variants and custom classes;
the site exposes few states (hover, disabled, focus on inputs), so most of
this file is a **Proposed** standard that keeps the verified timing and colors.

## Global motion rules (Verified)

- `transition` (150ms, Tailwind default property set) with `ease-linear` and `delay-50` (50ms) on buttons and tiles.
- Hover feedback is **opacity**, not color change: `hover:opacity-70` on buttons and links, `hover:opacity-40` on portfolio tiles, `hover:brightness-125` on icon buttons.
- Cards lift with `hover:shadow-2xl`.
- Flip animation 0.8s (`.enterprise-card`); skeleton shimmer 1.5s; both respect `prefers-reduced-motion` (skeleton verified; apply the same to all animation).

**Proposed:** keep 150ms for hover/focus, 200ms for panels (dropdowns, dialogs) and 300ms for drawers; `ease-out` for entering, `ease-in` for leaving; no transition longer than 300ms in products.

## Buttons

| State | Primary (Verified unless noted) | Secondary / outline | Ghost (Proposed) | Destructive (Proposed) |
|---|---|---|---|---|
| Default | `bg-blue text-white` | `bg-white text-blue border-blue` | `text-blue-2` | `bg-error-red text-white` |
| Hover | `opacity-70` | `opacity-70` | `bg-light-blue` | `opacity-70` |
| Focus-visible (Proposed) | `ring-2 ring-blue ring-offset-2` | same | same | `ring-error-red` |
| Active / pressed (Proposed) | `opacity-60`, no transform | `bg-light-blue` | `bg-[#EEF4FF]` | `opacity-60` |
| Disabled | `disabled:opacity-80` (verified) — **Proposed:** add `cursor-not-allowed` and prefer `opacity-50` in products for a clearer cue | same | `text-light-grey` | same |
| Loading (Proposed) | spinner replaces icon, label becomes progressive ("Saving…"), `aria-busy`, pointer-events none | same | same | same |
| Icon button disabled | `bg-[#D4E0ED] text-blue` (verified) | — | — | — |

## Links and navigation

| State | Verified | Proposed addition |
|---|---|---|
| Default nav item | Montserrat 500 `dark-grey` | — |
| Hover | not styled on site | `text-blue` |
| Active page | 700, `blue`, underline, offset 6px | sidebar: `bg-[#EEF4FF] text-blue-2 font-semibold` |
| Focus-visible | — | `ring-2 ring-blue rounded` |
| Visited | unstyled | unstyled (products) |

## Cards and tiles

| State | Verified |
|---|---|
| Hover (clickable card) | `shadow-2xl` (news), `opacity-40` (portfolio tile) |
| Hover (static card) | none |
| Focus-visible (Proposed) | `ring-2 ring-blue ring-offset-2` on the card link |
| Selected (Proposed) | `border-2 border-blue bg-light-blue` |

## Form controls

| State | Input / textarea | Checkbox / radio / toggle (Proposed) |
|---|---|---|
| Default | 1px `#D0D5DD` (verified) | border `#D0D5DD`, white |
| Hover (Proposed) | border `#B8BEC9` | border `grey` |
| Focus | 2.5px `#50A6D8`, `outline: none` (verified) | `ring-2 ring-blue ring-offset-2` |
| Filled | text `#040815` (verified) | — |
| Error (Proposed) | border `error-red`; message in `error-red`; icon | border `error-red` |
| Success (Proposed, optional) | border `success-700` | — |
| Disabled (Proposed) | `bg-gray-100 text-light-grey cursor-not-allowed` | `opacity-50 cursor-not-allowed` |
| Read-only (Proposed) | `bg-light-blue`, no focus border change | — |
| Checked | — | `bg-blue` + white check (site config has `checked:bg-blue`) |

## Tables, rows and list items (Proposed)

| State | Style |
|---|---|
| Row hover | `bg-light-blue` |
| Row selected | `bg-[#EEF4FF]`; checkbox checked |
| Row focus (keyboard) | `ring-2 ring-inset ring-blue` |
| Sortable header hover | `text-blue` + sort icon visible |
| Sorted column | header `text-blue font-semibold`, arrow icon |
| Disabled row | `text-light-grey`, actions hidden |

## Overlays (Proposed)

- Dropdown / popover: fade + 4px slide, 150ms; close on Esc, outside click, and selection.
- Dialog: overlay fades 200ms; panel fades and scales from 0.98; focus trapped; close returns focus.
- Toast: slide from top-right 200ms; auto-dismiss 5s; pause on hover and focus.

## Loading, empty and error (Proposed state matrix for data views)

| State | What to show |
|---|---|
| Initial load | Skeletons matching the final layout (KPI numbers, 5 table rows, chart area) |
| Refreshing with data | Keep data visible; add a subtle top progress bar or a spinner in the card header; disable filters |
| Empty (no data at all) | Empty-state block with primary action |
| Empty (filtered) | "No results" block with "Clear filters" |
| Partial error | Inline error alert in the affected card, retry button; other cards stay interactive |
| Full error | Full-page error layout |
| Stale / offline | Info banner: "Showing data as of 10:42. Reconnecting…" |

## Reduced motion

Under `@media (prefers-reduced-motion: reduce)`: remove shimmer and slides, keep opacity fades at ≤ 100ms, disable card flips and parallax.

## Related files

- [components.md](components.md), [forms-and-inputs.md](forms-and-inputs.md), [tables-and-data-display.md](tables-and-data-display.md), [accessibility.md](accessibility.md#focus-and-keyboard)
