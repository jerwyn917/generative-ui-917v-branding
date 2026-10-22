# Design Principles

Actionable rules derived from how 917ventures.com is built (see
[../brand/brand-identity.md](../brand/brand-identity.md)). Apply them to every
917 Ventures digital product. Each principle lists the verified evidence and the
product-level rule that follows from it.

## 1. Light, white, and spacious

- **Evidence (Verified):** white page background, `light-blue #FAFBFF` alternate sections, 64–96px vertical section padding (`py-16`, `pt-[90px]`/`pt-[180px]`), white cards with diffuse shadows.
- **Rule:** default every product to a light theme. Use `#FFFFFF` for content surfaces, `#FAFBFF` for app-shell backgrounds and page canvases, and reserve navy `#142741` for a single dark element (sidebar or cover). Do not build a dark mode unless the project explicitly requires one; the site has none.

## 2. One blue does the work

- **Evidence (Verified):** every interactive element (primary button, outline button, active nav link, pagination pill, carousel arrows, focus border family) is `#407DC3` or a close relative.
- **Rule:** interactive = blue. Status = semantic tokens. Decoration = gradients. Never let decoration borrow the interactive blue for non-clickable elements, and never color a clickable element with anything other than blue, white-on-blue, or a neutral ghost style.

## 3. Display type is loud, body type is quiet

- **Evidence (Verified):** Unbounded 700 uppercase section titles at 36px in blue; Montserrat 400 at 16/24 for body; nav and buttons at Montserrat 500–700.
- **Rule:** use Unbounded only for page-level titles and KPI numbers. Keep everything else in Montserrat with weight, not size, carrying hierarchy. One Unbounded element per viewport is enough.

## 4. Soft geometry

- **Evidence (Verified):** `rounded-lg` (8px) on buttons and inputs; 15px on cards and tiles; 20px on stat cards; `rounded-full` on icon buttons and avatars; shadows `md`–`2xl`; no visible 1px dark outlines except the 1px blue outline button.
- **Rule:** 8px radius on controls, 12–16px on cards, full radius on icon buttons/avatars/chips. Borders are light (`#D0D5DD`) or brand-tinted (`#50A6D8`), never black. Elevation comes from shadow, not from border weight.

## 5. Numbers are the hero

- **Evidence (Verified):** the homepage leads with three white stat cards ("46+ Ideas vetted", "5 Ventures launched & tested", "12 Portfolio companies") using gradient Unbounded numerals and uppercase labels.
- **Rule:** in dashboards and reports, lead with summary metric cards before charts or tables. Use the stat card anatomy in [components.md](components.md#summary-metric-cards-stat-cards). Use amCharts only where a chart adds analytical value ([charts-and-visualizations.md](charts-and-visualizations.md)).

## 6. Calm motion

- **Evidence (Verified):** all buttons use `transition ease-linear delay-50` with `hover:opacity-70`; cards use `hover:shadow-2xl`; carousels use keen-slider; skeletons shimmer at 1.5s and honor `prefers-reduced-motion`.
- **Rule:** transitions are 150ms linear on opacity, shadow and color. No bounces, no parallax in products, no auto-playing motion except loading skeletons. Respect `prefers-reduced-motion`.

## 7. Consistency over novelty

- **Rule (Proposed, implied by the skill's purpose):** before creating a component, look for an existing one in the project; before adding a dependency, check whether Tailwind, Heroicons or amCharts already covers the need. Do not restyle an existing screen while adding a feature. Preserve the host application's architecture, routing and state management.

## 8. Verified first, proposed second

- **Rule:** when the website gives evidence, follow it exactly. When it does not (tables, forms beyond the contact form, charts, admin patterns), follow the **Proposed** sections of this skill and say so in code comments or PR notes so the brand owner can confirm or adjust.

## 9. Accessible by default

- **Evidence (Verified):** semantic `<button>`, `<form>`, `<label>` elements; `aria-hidden` on decorative Heroicons; reduced-motion handling in skeletons. Gaps: brand blue at 4.25:1 on white, green badge text at 2.2:1, no visible focus ring styles beyond the input border.
- **Rule:** meet WCAG 2.1 AA. Use `blue-2` for small blue text, `success-700` for green text, and always provide a visible focus ring ([accessibility.md](accessibility.md), [interaction-states.md](interaction-states.md)).

## Related files

- [layout-and-spacing.md](layout-and-spacing.md), [components.md](components.md), [interaction-states.md](interaction-states.md), [accessibility.md](accessibility.md)
