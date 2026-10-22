---
name: 917-ventures-branding
description: Official 917 Ventures brand and UI/UX design guidelines. Use whenever building or modifying any website, dashboard, admin portal, data visualization, report, email template or other digital product for 917 Ventures (917V) — colors, typography, logo, Heroicons icon system, layout, components, forms, tables, amCharts charts, responsive rules, accessibility and interaction states. Trigger on "917 Ventures", "917V", "917ventures.com", or any UI work inside a 917 Ventures repository.
---

# 917 Ventures Branding Skill

You are building or changing user interface for **917 Ventures**. Apply this
skill before writing any UI code, styles, charts or design documentation.

Source of truth: https://917ventures.com (analyzed 2026-10-22). Every file in
this skill labels content as **Verified** (observed on the website) or
**Proposed** (a documented extension for products the website does not cover).
Follow both; when you rely on a Proposed rule, say so in your summary so the
brand owner can confirm.

## Core instructions

1. **Follow the documented brand** for every new or modified screen. Do not invent colors, fonts, icons, radii or shadows; take them from [design-system/design-tokens.md](design-system/design-tokens.md).
2. **Reuse before you add.** Use the host project's existing components, utilities and libraries first. Restyle them with the brand tokens rather than introducing a new component library. Add a dependency only when the skill names it (Heroicons, amCharts) or the project already has it.
3. **Stay consistent across pages.** One primary blue, two typefaces, 8px/15px radii, white cards on `#FAFBFF`. Match existing screens in the same app; do not redesign screens you were not asked to change.
4. **Preserve the application.** Keep its architecture, routing, state management, data fetching and functionality unless explicitly instructed otherwise. Branding work is additive and visual.
5. **Build responsive and accessible UI by default**: mobile-first, WCAG 2.1 AA contrast, visible focus, keyboard support, semantic HTML.
6. **Implement interaction states** (hover, focus, active, disabled, loading, error, empty) using [design-system/interaction-states.md](design-system/interaction-states.md); never ship a data view without loading, empty and error states.
7. **Use amCharts 5** for charts, dashboards and analytical reports; use tables, metric cards or text when they communicate better. Never add a competing chart library without a documented technical limitation.
8. **Use Heroicons** for functional icons and the brand assets for identity; never mix icon libraries.
9. **Consult the specific file before implementing a feature** (table below). Read only what the task needs.
10. **Distinguish verified from proposed** in your output, and flag anything you could not match to the documentation as a recommendation, not a brand standard.

## Where to look

| Task | Read |
|---|---|
| Any UI work (start here) | [design-system/design-tokens.md](design-system/design-tokens.md), [design-system/design-principles.md](design-system/design-principles.md) |
| Colors, gradients, semantic colors | [brand/color-palette.md](brand/color-palette.md) |
| Fonts, sizes, headings, KPI numerals | [brand/typography.md](brand/typography.md) |
| Logo, favicon, imagery, brand assets | [brand/logo-and-assets.md](brand/logo-and-assets.md) |
| Icons (library, sizes, colors) | [brand/icons.md](brand/icons.md) |
| Brand voice, personality, positioning | [brand/brand-identity.md](brand/brand-identity.md) |
| Page structure, app shell, grids, spacing | [design-system/layout-and-spacing.md](design-system/layout-and-spacing.md) |
| Buttons, nav, sidebar, cards, badges, tabs, dialogs, alerts, tooltips, empty/loading/error | [design-system/components.md](design-system/components.md) |
| Forms, inputs, search, selects, validation | [design-system/forms-and-inputs.md](design-system/forms-and-inputs.md) |
| Tables, sorting, filtering, pagination, key–value lists | [design-system/tables-and-data-display.md](design-system/tables-and-data-display.md) |
| Charts, dashboards' visuals, amCharts setup, formatting, export | [design-system/charts-and-visualizations.md](design-system/charts-and-visualizations.md) |
| Breakpoints and mobile behavior | [design-system/responsive-design.md](design-system/responsive-design.md) |
| Contrast, keyboard, screen readers | [design-system/accessibility.md](design-system/accessibility.md) |
| Hover/focus/disabled/loading states | [design-system/interaction-states.md](design-system/interaction-states.md) |
| Assembling a dashboard or admin portal | [examples/dashboard-guidelines.md](examples/dashboard-guidelines.md) |
| Assembling a report (screen, PDF, print) | [examples/report-guidelines.md](examples/report-guidelines.md) |

## Quick reference (verified unless noted)

- **Primary blue** `#407DC3` (`blue`) · small blue text `#3A6DBA` (`blue-2`) · text `#040815` · navy `#142741` · canvas `#FAFBFF` · borders `#D0D5DD` · focus `#50A6D8` · error `#D92D20` · success badge `#34C759` (text: proposed `#1E7A3C`).
- **Gradients:** brand `#65DDF5 → #3A6DBA`; stat numerals `#DB6E4F → #DA4E8F`.
- **Fonts:** Unbounded (display, KPI numbers) + Montserrat (everything else). Load via `next/font` or Google Fonts; `font-display: swap`.
- **Buttons:** `bg-blue text-white px-8 py-3 font-bold rounded-lg hover:opacity-70 transition ease-linear delay-50 disabled:opacity-80`; outline variant `bg-white text-blue border border-blue`.
- **Inputs:** 44px, `border-[#D0D5DD] rounded-lg p-3 text-sm`, focus `border-[#50A6D8]` 2.5px; label 12px above; required `*` in `#D92D20`.
- **Cards:** white, 15px radius (20px for stat cards), `shadow-xl` marketing / `shadow-sm` + light border in products, 24–32px padding.
- **Header:** sticky white, `shadow-md`, 96px marketing / 64px app, logo 135–240px.
- **Icons:** Heroicons v2, 24 outline default, 20 solid inline, `currentColor`.
- **Charts:** amCharts 5 + brand theme; categorical order blue, coral, blue-2, cyan, pink, steel, navy, amber; tooltips navy with white text; `₱#,###`, `dd MMM yyyy`.
- **Breakpoints:** sm 640 · md 768 · lg 1024 · xl 1280 (desktop nav) · 2xl 1440/1536.

## Workflow for a UI task

1. Identify the surface (marketing page, app screen, dashboard, report) and read the matching files above.
2. Inventory the host project: existing design tokens, component library, icon package, chart library, Tailwind config. Map them to this skill's tokens; extend the Tailwind config with [design-system/design-tokens.md](design-system/design-tokens.md) if tokens are missing.
3. Implement with the documented components and states; reuse the project's components.
4. Check: contrast of every new text/background pair, keyboard path, responsive behavior at 360px and 1440px, loading/empty/error states, chart checklist.
5. Report what you built, which rules were Verified vs Proposed, and any deviation with its reason.

## Do not

- Introduce new brand colors, a third typeface, a different icon set, or another chart library.
- Use gradient text below 24px, `success-green` or `light-grey` as small text, or `blue` for 12–14px regular text.
- Remove focus styles, build a dark theme unprompted, add autoplaying motion, or use carousels in products.
- Hotlink assets from 917ventures.com in production; copy them into the project.
- Restyle unrelated screens or change application behavior while applying branding.
