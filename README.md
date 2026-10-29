# 917 Ventures Branding Skill

A reusable Claude Skill that documents the official 917 Ventures brand and
UI/UX design system and turns "visualize this", "chart these numbers",
"build a dashboard" or "mock up a page" requests into branded visuals: interactive
widgets inside the chat that the user can control (toggle, filter, drill down,
send actions back to Claude) or self-contained HTML artifacts for full pages
(charts with amCharts 5 in both: columns, lines, donuts, funnels, gauges, treemaps,
sankeys, heatmaps, Philippine maps and more). In a codebase it applies the same
system to application UI.

- **Source of truth:** https://917ventures.com, analyzed on 2026-10-22 (homepage, About Us, Portfolio, Partner With Us, Careers, News, 917V Life, Velocity; compiled Tailwind CSS; served assets). Artifact and in-chat widget patterns verified 2026-10-29.
- **Charting standard:** amCharts 5 (https://www.amcharts.com), loaded from cdnjs inside widgets and artifacts.
- **Icon standard:** Heroicons v2 (verified in use on the site, extended as the standard).

## How it works on Claude Desktop

1. Install the skill (below).
2. Ask Claude anything generative-UI shaped: "visualize this quarter's venture revenue", "turn this PDF report into a dashboard", "chart the attached CSV by region", "mock up the partner portal home". The skill's description matches these requests and loads automatically.
3. Claude classifies the request (cards, table, chart, report, dashboard, mockup), inlines your data, and picks the surface: an **interactive widget in the chat** built from [examples/interactive-chat-widget.md](examples/interactive-chat-widget.md) when you want to explore or control the view (period toggles, filters, click a bar to drill down, ↗ buttons that send a request back to Claude), or a **full HTML artifact** built from [examples/generative-ui-artifact.md](examples/generative-ui-artifact.md) for reports, dashboards and mockups, with the brand tokens, logo, fonts and amCharts theme.
4. You control the result from the conversation: follow-ups ("switch to quarterly", "only Velocity", "make it a line chart") re-render the widget with its state updated or update the same artifact; a widget's "Open as full dashboard ↗" hands the current state to the artifact flow.

## How to install

**Claude Desktop / claude.ai (custom skill):** zip the folder so that `SKILL.md` is at the root of the archive, then upload it under Settings → Capabilities → Skills (the exact menu name may differ by release). The skill is then available in chats and projects.

```bash
cd /Users/917v-jnrabor/Documents/claude/generative-ui-skill && zip -r 917-ventures-branding.zip 917-ventures-branding -x "*.DS_Store"
```

**Claude Code (project):** copy this directory to `.claude/skills/917-ventures-branding/` in the repository; it loads when the task matches or on `/917-ventures-branding`.

**Claude Code (personal):** copy to `~/.claude/skills/917-ventures-branding/`.

**Cowork:** add the folder as a skill or plugin; artifacts are written as `.html` files to the outputs folder.

No code, scripts or build steps are required; the skill is Markdown plus one Markdown asset file with the logo as data URIs.

## Structure

```
917-ventures-branding/
├── SKILL.md                         # entry point: generative-UI flow, core rules, file map, quick reference
├── README.md                        # this file
├── assets/
│   └── logo-data-uris.md            # horizontal logo + square mark as base64 data URIs for artifacts
├── brand/
│   ├── brand-identity.md            # positioning, personality, voice, evidence labels
│   ├── color-palette.md             # every verified HEX, semantic extensions, gradients, usage rules
│   ├── typography.md                # Unbounded + Montserrat, verified scale, product scale
│   ├── logo-and-assets.md           # logo files, favicon, imagery, usage rules
│   └── icons.md                     # Heroicons system, sizes, colors, rules
├── design-system/
│   ├── design-tokens.md             # CSS variables + Tailwind config (machine-readable)
│   ├── design-principles.md         # nine actionable principles with evidence
│   ├── layout-and-spacing.md        # container, spacing, grids, app shell, report layout
│   ├── components.md                # buttons … tooltips, empty/loading/error
│   ├── forms-and-inputs.md          # verified contact form spec + product form rules
│   ├── tables-and-data-display.md   # tables, sorting, filtering, pagination, data displays
│   ├── charts-and-visualizations.md # amCharts 5 standard: theme, chart types, formats, states, export
│   ├── responsive-design.md         # breakpoints and per-breakpoint layout rules
│   ├── accessibility.md             # computed contrast matrix, focus, semantics, checklist
│   └── interaction-states.md        # hover/focus/active/disabled/loading/error matrices
└── examples/
    ├── interactive-chat-widget.md   # in-chat widget flow: chat ↔ widget control loop, host rules, tested starter with sendPrompt
    ├── amcharts-recipes.md          # every amCharts family on cdnjs: modules, type picker, per-type recipes, verified 10-type switcher widget
    ├── generative-ui-artifact.md    # request → artifact flow, runtime constraints, HTML starter page with amCharts
    ├── media-cards.md               # discovery / recommendation answers with photos: gather via web search, widget starter, artifact variant
    ├── dashboard-guidelines.md      # dashboard / admin portal blueprint + React skeleton
    └── report-guidelines.md         # report structure, print/PDF rules, export
```

## Verified vs. proposed

Every section is labeled:

- **Verified** — taken directly from 917ventures.com markup, compiled CSS, served assets or rendered computed styles. HEX codes, font families, button/input/card classes, header/footer behavior, breakpoints and the Heroicons usage are verified.
- **Proposed** — extensions required for products the website does not have (tables, dashboards, charts, semantic status colors, dark sidebar, focus rings). These are recommendations chosen to fit the verified identity and to pass WCAG AA; a brand owner should review them.

Known gaps in the website that the skill corrects for products: brand blue at 4.25:1 on white (use `#3A6DBA` for small text), green badge text at 2.2:1 (use `#1E7A3C` text on `#E8F8EE`), no visible focus ring on buttons, empty `alt` on the header logo, no reverse (white) logo file published.

## Maintaining the skill

1. Re-run the analysis when the website changes: fetch the compiled stylesheet (`/_next/static/css/*.css`), diff the Tailwind color/font utilities, and update [brand/color-palette.md](brand/color-palette.md), [brand/typography.md](brand/typography.md) and [design-system/design-tokens.md](design-system/design-tokens.md) together.
2. Keep rules in one place: tokens in `design-tokens.md`, usage in the brand files, component behavior in `components.md`. Other files link rather than repeat.
3. When a Proposed rule is confirmed by the brand team, change its label to Verified and note the source (brand book, Figma, decision date).
4. Add new surfaces as files under `examples/` (e.g. `email-guidelines.md`) and register them in the "Where to look" table in `SKILL.md`.
5. When amCharts publishes a new 5.x release, re-test the cdnjs script build (`index.js`, `xy.js`, `percent.js`, themes) in an artifact, then bump the pinned version in `examples/generative-ui-artifact.md` and `SKILL.md`.
6. Record the analysis date in `SKILL.md` and this README on every update.

## License and assets

Logo and imagery are the property of 917 Ventures; the skill embeds the two logo files as data URIs (artifacts cannot load remote images) for use by 917 Ventures and its agents only. Photos shown in media cards belong to the credited sites; the skill links them with a visible source credit and does not copy them. Fonts are Google Fonts (Unbounded, Montserrat; OFL). Heroicons is MIT. amCharts 5 requires a commercial license for production use without branding; see [design-system/charts-and-visualizations.md](design-system/charts-and-visualizations.md#library-version-and-licensing).
