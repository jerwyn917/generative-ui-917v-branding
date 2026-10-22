# 917 Ventures Branding Skill

A reusable Claude Skill that documents the official 917 Ventures brand and
UI/UX design system so AI coding agents can reproduce it consistently across
websites, dashboards, admin portals, reports and data visualizations.

- **Source of truth:** https://917ventures.com, analyzed on 2026-10-22 (homepage, About Us, Portfolio, Partner With Us, Careers, News, 917V Life, Velocity; compiled Tailwind CSS; served assets).
- **Charting standard:** amCharts 5 (https://www.amcharts.com).
- **Icon standard:** Heroicons v2 (verified in use on the site, extended as the standard).

## How to install

**Claude Code (project):** copy this directory to `.claude/skills/917-ventures-branding/` in the repository. Claude loads `SKILL.md` automatically when the task matches its description, or on `/917-ventures-branding`.

**Claude Code (personal):** copy to `~/.claude/skills/917-ventures-branding/`.

**Claude.ai / API skills:** zip the directory (with `SKILL.md` at the root) and upload it as a custom skill.

No code, scripts or build steps are required; the skill is Markdown only.

## Structure

```
917-ventures-branding/
├── SKILL.md                         # entry point: instructions + file map + quick reference
├── README.md                        # this file
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
5. Record the analysis date in `SKILL.md` and this README on every update.

## License and assets

Logo and imagery are the property of 917 Ventures; this skill references them by URL and does not redistribute them. Fonts are Google Fonts (Unbounded, Montserrat; OFL). Heroicons is MIT. amCharts 5 requires a commercial license for production use without branding; see [design-system/charts-and-visualizations.md](design-system/charts-and-visualizations.md#library-version-and-licensing).
