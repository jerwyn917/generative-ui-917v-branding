---
name: 917-ventures-branding
description: 917 Ventures (917V) brand and generative-UI skill. Use for ANY request to visualize, chart, graph, plot, dashboard, scorecard, report, summarize visually, mock up, prototype, wireframe or design a page, screen, table, KPI view, infographic or data visualization, for interactive visuals the user wants to control from the chat (filter, toggle, drill down), and for any UI or styling work in a 917 Ventures codebase. Produces branded in-chat widgets (controllable from the conversation) or single-file HTML artifacts (amCharts 5 for charts), or applies the brand to code. Triggers include "visualize", "chart this", "make a dashboard", "turn this into a report", "show me the numbers", "let me filter", "interactive", "widget", "mockup", "landing page", "917 Ventures", "917V", "917ventures.com".
---

# 917 Ventures Branding Skill

You are producing user interface for **917 Ventures**. This skill applies in two modes:

- **Generative UI (default on Claude Desktop / claude.ai / Cowork):** the user asks to visualize, chart, report, dashboard, mock up or design something. The deliverable is a rendered visual in 917 Ventures branding built from the user's data, on one of two surfaces: an **in-chat widget** the user controls from the conversation (toggles, filters, drill-downs that send prompts back) following [examples/interactive-chat-widget.md](examples/interactive-chat-widget.md), or a **full artifact** (a single self-contained HTML page) following [examples/generative-ui-artifact.md](examples/generative-ui-artifact.md).
- **Codebase mode (Claude Code in a repository):** the user asks to build or change UI in an application. Apply the design system to code while preserving the app's architecture. Follow the "Where to look" table below.

Source of truth for the brand: https://917ventures.com (analyzed 2026-10-22). Every file labels content **Verified** (observed on the website) or **Proposed** (documented extension). Follow both; mention in your reply when a result relies on Proposed rules.

## Generative UI flow (do this when asked to visualize, chart, report, dashboard, mock up)

1. **Classify the request** with the table in [examples/generative-ui-artifact.md](examples/generative-ui-artifact.md#1-decide-what-to-render): metric cards, table, chart, report page, dashboard page, or UI mockup. Charts only when a trend, comparison, composition or distribution needs one ([charts-and-visualizations.md → When to use a chart](design-system/charts-and-visualizations.md#when-to-use-a-chart)); otherwise cards or a table.
2. **Pick the surface** ([widget or artifact?](examples/interactive-chat-widget.md#1-widget-or-artifact)): an **in-chat widget** when the user wants to interact or decide from the chat and one view fits ~680px (KPI row plus one or two charts); a **full artifact** for reports, dashboards with many sections, page mockups, or anything to share or keep. If the surface has no inline widget tool, use an artifact.
3. **Collect the data** from the conversation, uploaded files or connected tools and inline it as JSON. Never fabricate figures as real; if there is no data, use clearly labeled sample data and say so. Keep units, currency (₱) and period explicit.
4. **Build one widget or one HTML artifact.** Widget: the starter in [examples/interactive-chat-widget.md](examples/interactive-chat-widget.md#5-starter-widget-verified-2026-10-29): host theme tokens for surfaces and text, brand blue for series and wordmark, `DATA` + `STATE` objects, cheap interactions in JavaScript, `sendPrompt()` for anything that needs Claude, amCharts from cdnjs, HTML fallback. Artifact: the starter page in [examples/generative-ui-artifact.md](examples/generative-ui-artifact.md#5-starter-page): brand tokens (light + dark), Unbounded + Montserrat from Google Fonts, the logo as a data URI from [assets/logo-data-uris.md](assets/logo-data-uris.md), amCharts 5 loaded from cdnjs as global scripts (`https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/index.js`, then `xy.js`, `percent.js`, `themes/Animated.js`, `themes/Responsive.js`) with the brand theme, and a table fallback if the scripts cannot load.
5. **Respect the runtime**: widgets follow the [host rules](examples/interactive-chat-widget.md#3-widget-runtime-constraints-host-rules) (fragment only, flat, host tokens, no fixed positioning, no tables or prose inside). Artifacts: no external images, no downloads/print/export buttons, no iframes, responsive to 400px, complete on load, reduced-motion safe, 2–4 word title ([runtime constraints](examples/generative-ui-artifact.md#3-artifact-runtime-constraints)).
6. **Use the right page pattern**: report → [examples/report-guidelines.md](examples/report-guidelines.md); dashboard → [examples/dashboard-guidelines.md](examples/dashboard-guidelines.md); mockup → [design-system/components.md](design-system/components.md).
7. **Deliver**: render the widget or publish the artifact, then reply in one or two lines with what was visualized, the data source and period, and any sample or Proposed elements. Offer a PDF/XLSX/PPTX only as a separate file if the user needs one; the artifact cannot download or print.
8. **Iterate in place**: on follow-ups ("add a filter", "switch to bars"), re-render the widget with its `STATE` updated or update the same artifact, rather than creating a new one. When a widget sends a prompt (a ↗ button or a chart click), answer it and re-render the widget with the selection carried in `STATE`; "open as full dashboard" switches to the artifact flow with the same `STATE`.

## Core rules (both modes)

1. **Use only the documented brand**: colors, fonts, radii, shadows and spacing from [design-system/design-tokens.md](design-system/design-tokens.md). No new brand colors, no third typeface.
2. **Reuse before you add.** In a codebase, use existing components and libraries first and restyle them with the tokens; add a dependency only when this skill names it (Heroicons, amCharts) or the project already has it.
3. **Stay consistent**: one primary blue (`#407DC3`; `#3A6DBA` for small text), white cards on `#FAFBFF`, 8px controls / 15px cards, Unbounded for display and KPI numbers, Montserrat for everything else.
4. **Preserve the application** in codebase mode: architecture, routing, state, data fetching and behavior stay unchanged unless the user asks. Do not restyle screens you were not asked to change.
5. **Responsive and accessible by default**: mobile-first, WCAG 2.1 AA contrast ([accessibility.md](design-system/accessibility.md)), visible focus, keyboard support, semantic HTML, text alternatives for charts.
6. **Implement states**: hover, focus, active, disabled, loading, empty, error ([interaction-states.md](design-system/interaction-states.md)). Never ship a data view without loading, empty and error states.
7. **Charts = amCharts 5**, with the brand theme and categorical order from [charts-and-visualizations.md](design-system/charts-and-visualizations.md). Never introduce another chart library without a documented technical limitation.
8. **Icons = Heroicons v2** for functional glyphs; brand assets for identity; never mix icon sets ([icons.md](brand/icons.md)).
9. **Read the specific file before implementing** (table below). Read only what the task needs.
10. **Say what is Verified vs Proposed** and flag anything you could not match to the documentation as a recommendation.

## Where to look

| Task | Read |
|---|---|
| Interactive visual the user controls from the chat (toggle, filter, drill down) | [examples/interactive-chat-widget.md](examples/interactive-chat-widget.md) (start here for widgets) |
| Visualize / chart / report / dashboard / mockup as an artifact | [examples/generative-ui-artifact.md](examples/generative-ui-artifact.md) (start here for artifacts), then the pattern file below |
| Logo for an artifact or single-file page | [assets/logo-data-uris.md](assets/logo-data-uris.md) |
| Any codebase UI work (start here) | [design-system/design-tokens.md](design-system/design-tokens.md), [design-system/design-principles.md](design-system/design-principles.md) |
| Colors, gradients, semantic colors | [brand/color-palette.md](brand/color-palette.md) |
| Fonts, sizes, headings, KPI numerals | [brand/typography.md](brand/typography.md) |
| Logo files, favicon, imagery rules | [brand/logo-and-assets.md](brand/logo-and-assets.md) |
| Icons (library, sizes, colors) | [brand/icons.md](brand/icons.md) |
| Brand voice, personality, positioning | [brand/brand-identity.md](brand/brand-identity.md) |
| Page structure, app shell, grids, spacing | [design-system/layout-and-spacing.md](design-system/layout-and-spacing.md) |
| Buttons, nav, sidebar, cards, badges, tabs, dialogs, alerts, tooltips, empty/loading/error | [design-system/components.md](design-system/components.md) |
| Forms, inputs, search, selects, validation | [design-system/forms-and-inputs.md](design-system/forms-and-inputs.md) |
| Tables, sorting, filtering, pagination, key–value lists | [design-system/tables-and-data-display.md](design-system/tables-and-data-display.md) |
| Chart selection, amCharts theme, formats, states, export, performance | [design-system/charts-and-visualizations.md](design-system/charts-and-visualizations.md) |
| Breakpoints and mobile behavior | [design-system/responsive-design.md](design-system/responsive-design.md) |
| Contrast, keyboard, screen readers | [design-system/accessibility.md](design-system/accessibility.md) |
| Hover/focus/disabled/loading states | [design-system/interaction-states.md](design-system/interaction-states.md) |
| Dashboard or admin portal layout | [examples/dashboard-guidelines.md](examples/dashboard-guidelines.md) |
| Report layout (screen, PDF, print) | [examples/report-guidelines.md](examples/report-guidelines.md) |

## Quick reference (verified unless noted)

- **Colors:** primary blue `#407DC3` · small blue text `#3A6DBA` · text `#040815` · navy `#142741` · canvas `#FAFBFF` · borders `#D0D5DD` · focus `#50A6D8` · error `#D92D20` · success text `#1E7A3C` (proposed) · selected/info surface `#EEF4FF` (proposed).
- **Gradients:** brand `#65DDF5 → #3A6DBA`; stat numerals `#DB6E4F → #DA4E8F` (display sizes only).
- **Fonts:** Unbounded (display, KPI numbers) + Montserrat (body, UI). Google Fonts: `https://fonts.googleapis.com/css2?family=Unbounded:wght@500;600;700&family=Montserrat:wght@400;500;600;700&display=swap`.
- **Buttons:** `bg-blue text-white px-8 py-3 font-bold rounded-lg hover:opacity-70 transition ease-linear delay-50 disabled:opacity-80`; outline `bg-white text-blue border border-blue`.
- **Inputs:** 44px, `border-[#D0D5DD] rounded-lg p-3 text-sm`, focus `border-[#50A6D8]` 2.5px; label 12px above; required `*` in `#D92D20`.
- **Cards:** white, 15px radius (20px stat cards), `shadow-xl` marketing / `shadow-sm` + light border in products, 20–32px padding.
- **Header:** sticky white, `shadow-md`, 96px marketing / 64px app, logo 135–240px.
- **Icons:** Heroicons v2, 24 outline default, 20 solid inline, `currentColor`.
- **Charts:** amCharts 5.21.5; series order blue, coral `#DB6E4F`, blue-2, cyan `#52BCD5`, pink `#DA4E8F`, steel `#8EA1BD`, navy, amber `#B45309`; tooltip navy/white; `₱#,###`, `dd MMM yyyy`; donut ≤ 6 slices; value axis from zero.
- **Breakpoints:** sm 640 · md 768 · lg 1024 · xl 1280 (desktop nav) · 2xl 1440/1536.
- **Artifact loading:** scripts from cdnjs (preferred), jsDelivr or unpkg only; amCharts from cdnjs script tags, never jsDelivr `+esm`; stylesheets from Google Fonts only; images as data URIs.
- **In-chat widgets (proposed):** fragment only; host tokens (`--surface-1`, `--text-primary`, `--text-secondary`, `--bg-accent`, `--text-success`, `--text-danger`, `--radius`) for surfaces and text; brand blue `#407DC3` for series and the text wordmark; Unbounded for numerals only; Tabler outline icons (host-provided); `sendPrompt('<one specific sentence>')` on ↗ buttons and chart clicks; ids prefixed `v917-`.

## Codebase workflow

1. Identify the surface (marketing page, app screen, dashboard, report) and read the matching files.
2. Inventory the project: tokens, component library, icon package, chart library, Tailwind config. Map them to this skill's tokens; extend the Tailwind config from [design-system/design-tokens.md](design-system/design-tokens.md) if tokens are missing.
3. Implement with the documented components and states, reusing project components.
4. Check contrast of new pairings, keyboard path, 360px and 1440px layouts, loading/empty/error states, chart checklist.
5. Report what changed, which rules were Verified vs Proposed, and any deviation with its reason.

## Do not

- Introduce new brand colors, a third typeface, another icon set, or another chart library.
- Use gradient text below 24px, `#34C759` or `#8D919F` as small text, or `#407DC3` for 12–14px regular text.
- Remove focus styles, add autoplaying motion, or use carousels in products.
- Put Export, Print or Download buttons inside an artifact, load external images, or hotlink 917ventures.com assets in production.
- Present sample data as real, or restyle unrelated screens while applying branding.
- Put tables, headings or explanatory prose inside a chat widget, hard-code brand text colors in one (use host tokens), or leave a widget button that does nothing: every action either works in JavaScript or sends a prompt.
