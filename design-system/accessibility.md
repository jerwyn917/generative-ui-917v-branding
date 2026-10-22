# Accessibility

Target: **WCAG 2.1 AA** for every 917 Ventures product. The website gives a
partial baseline (semantic markup, `aria-hidden` on decorative icons,
reduced-motion support in skeletons) but also has gaps this file corrects.
Contrast values below were computed from the verified HEX codes.

## Contrast matrix (computed)

| Foreground | Background | Ratio | Result | Allowed use |
|---|---|---|---|---|
| `black #040815` | white | 19.99:1 | AA | Any text |
| `dark-grey #323643` | white | 12.04:1 | AA | Any text |
| `grey #606471` | white | 5.90:1 | AA | Any text, labels |
| `placeholder #667085` | white | 4.97:1 | AA | Placeholder, captions |
| `light-grey #8D919F` | white | 3.14:1 | AA-large only | Text ≥ 24px / ≥ 19px bold, disabled text, decorative icons |
| `blue #407DC3` | white | 4.25:1 | AA-large only | Headlines, 16px **bold** button/nav text (site practice), icons ≥ 3:1 |
| white | `blue #407DC3` | 4.25:1 | AA-large only | Primary button label (16px bold passes as large-ish; keep ≥ 14px bold) |
| `blue-2 #3A6DBA` | white | 5.15:1 | AA | Small links, 12–14px blue text, info text |
| white | `blue-2 #3A6DBA` | 5.15:1 | AA | Buttons needing small text |
| white | `navy #142741` | 15.04:1 | AA | Hero, dark sidebar, tooltips |
| `blue-1 #65DDF5` | navy | 9.46:1 | AA | Accent text on navy |
| `blue-1 #65DDF5` | white | 1.59:1 | Fail | Decorative only |
| `error-red #D92D20` | white | 4.83:1 | AA | Error text, required marks |
| `success-green #34C759` | white | 2.22:1 | Fail | Filled badge background only, never as text |
| white | `success-green #34C759` | 2.22:1 | Fail | The site's ribbon text fails; in products use `success-700` text on `success-50` |
| `success-700 #1E7A3C` | white | 5.38:1 | AA | Success text |
| `warning-700 #B45309` | white | 5.02:1 | AA | Warning text |
| `error-red #D92D20` | `error-50 #FFF5F4` | 4.51:1 | AA | Error alerts and badges |
| `success-700` | `success-50 #E8F8EE` | 4.89:1 | AA | Success alerts and badges |
| `warning-700` | `warning-50 #FFF4E0` | 4.61:1 | AA | Warning alerts and badges |
| `blue-2 #3A6DBA` | `info-50 #EEF4FF` | 4.67:1 | AA | Info alerts, selected rows, active nav items |
| `pink-2 #DA4E8F` / `pink-1 #DB6E4F` | white | 3.84 / 3.30:1 | AA-large | Gradient stat numerals ≥ 24px only |
| white | `#55C0D5` (CTA band top) | 2.12:1 | Fail | The band's white text passes only toward the `#3A6DBA` end; put text on the darker half or add a `navy` 30% overlay |
| `#50A6D8` (focus border) | white | 2.70:1 | Below 3:1 | Keep for the input border but **add** a 2px `blue` focus ring for AA component contrast |
| `#D0D5DD` (input border) | white | 1.47:1 | Below 3:1 | Acceptable as a non-essential border; the field is identified by its label and padding. For AA-strict projects use `#7A8699` (3.7:1) |

Rules derived from the matrix:

1. Body and UI text ≤ 18px regular uses `black`, `dark-grey` or `grey`.
2. Blue text at small sizes uses `blue-2`; `blue` is for headlines, bold ≥ 16px, and fills.
3. Green and amber never appear as text in their verified `#34C759` / `#EA9430` forms; use the `-700` tokens.
4. Gradient text is limited to display sizes (≥ 24px).
5. Icons that convey state need 3:1 against their background.

## Focus and keyboard

- Every interactive element is reachable with Tab in visual order and operable with Enter/Space; menus and tabs support arrow keys; Esc closes overlays.
- Visible focus: the verified input focus (2.5px `#50A6D8` border) plus, for everything else, `focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-blue focus-visible:ring-offset-2` (white offset; on navy use `ring-white`). Never remove focus styles without a replacement (the site's `outline-none` on inputs is acceptable only because the border change exists).
- Skip link ("Skip to content") as the first focusable element in app shells.
- Focus management: dialogs trap focus and return it; after route changes, move focus to the page `h1`; after deleting a row, move focus to the next row or the table.

## Semantics and structure

- One `h1` per page; headings descend without skipping levels (the site's inner pages use `h1`/`h2`; the homepage relies on styled `div`s — do not copy that; use real headings).
- Landmarks: `header`, `nav` (with `aria-label`), `main`, `aside`, `footer`.
- Buttons for actions, links for navigation; no `div` click handlers.
- Forms: see [forms-and-inputs.md](forms-and-inputs.md#accessibility). Tables: see [tables-and-data-display.md](tables-and-data-display.md#accessibility). Charts: see [charts-and-visualizations.md](charts-and-visualizations.md#accessibility-and-readability).
- Images: logo `alt="917Ventures"`; informative images describe content; decorative `alt=""`. The site's header logo currently has an empty `alt` — products must not repeat this.
- Icons: decorative `aria-hidden`; icon-only controls `aria-label`.
- Live regions: toasts `role="status"`; errors `role="alert"`; loading `aria-busy` on the region.

## Text and sizing

- Minimum readable text 12px (captions) and 14px (body) in products; marketing may go larger, never smaller.
- Line height ≥ 1.4 for body; paragraph width ≤ 75 characters.
- Support 200% zoom and 320px-wide reflow without horizontal scrolling or loss of content.
- Do not use uppercase for long strings (the site uses it only for short titles and labels); uppercase labels need +0.04em tracking.
- Touch targets ≥ 44px on touch devices; ≥ 24px with spacing on desktop.

## Motion and media

- Honor `prefers-reduced-motion` (verified in skeletons): disable shimmer, flips, slides and chart entry animations.
- No autoplaying video with sound; the hero video is muted and decorative — in products avoid background video.
- No flashing content.

## Color independence

- Status badges include text; charts use labels/markers in addition to color; required fields use `*` plus the word "required" in `aria-label` or helper text; links in body text are underlined on hover and distinguishable by weight.

## Testing checklist

- [ ] Axe / Lighthouse accessibility score ≥ 95 with no serious violations
- [ ] Keyboard-only walkthrough of every flow, including dialogs and menus
- [ ] Screen reader pass (VoiceOver + Safari, NVDA + Chrome) on the main dashboard and one form
- [ ] Contrast verified for any new color pairing with the formula in [../brand/color-palette.md](../brand/color-palette.md)
- [ ] 200% zoom and 360px width check
- [ ] Reduced-motion mode check

## Related files

- [../brand/color-palette.md](../brand/color-palette.md), [interaction-states.md](interaction-states.md), [forms-and-inputs.md](forms-and-inputs.md), [charts-and-visualizations.md](charts-and-visualizations.md)
