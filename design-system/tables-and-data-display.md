# Tables and Data Display

917ventures.com has no data tables, so this file is a **Proposed** standard
built from the verified tokens (Montserrat, `#D0D5DD` rules, `light-blue`
surfaces, blue pagination pills). Use ordinary tables for record lists and
exact values; use amCharts only when a trend or comparison needs a picture
([charts-and-visualizations.md](charts-and-visualizations.md)).

## Table anatomy

```
Card (white, 15px radius, border #D0D5DD/60)
├── Toolbar: title (16px 700) · count badge · search (sm) · filters · [Export] [Primary action]
├── <table>
│   ├── thead: 12px 600 grey, sentence case, bg light-blue, border-b #D0D5DD, sticky
│   ├── tbody rows: 14px 400 black, 12px × 16px cell padding, border-b #D0D5DD, hover light-blue
│   └── numeric cells right-aligned, tabular-nums
└── Footer: "Showing 1–25 of 1,284" (12px grey) · page size select · pagination
```

### Specification

| Element | Spec |
|---|---|
| Header cell | Montserrat 600 12px `grey`, `bg-light-blue`, `py-3 px-4`, sentence case, sortable headers show `ChevronUpDownIcon` 16px on hover and `ChevronUpIcon`/`ChevronDownIcon` when sorted; sorted header `text-blue` |
| Body cell | Montserrat 400 14px `black`, `py-3 px-4`, `border-b border-[#D0D5DD]`, vertical-align middle, single line with ellipsis + tooltip for long text |
| Dense variant | `py-2 text-xs` for admin lists > 25 rows |
| Row height | 48px default, 36px dense |
| First column | Primary identifier, 600 weight, may be a `blue-2` link |
| Numbers | Right-aligned, `tabular-nums`, same decimals per column, thousands separators |
| Dates | Left-aligned, `DD MMM YYYY` (e.g. 22 Oct 2026); relative time only with a title tooltip giving the absolute date |
| Status | Badge pill from [components.md](components.md#badges-and-status-indicators) |
| Actions | Right-most column, ghost icon buttons 32px, revealed on hover but always focusable; overflow in a `EllipsisHorizontalIcon` menu after 2 actions |
| Selection | Checkbox column 40px wide; selected rows `bg-[#EEF4FF]`; bulk action bar replaces the toolbar |
| Zebra striping | Not used (borders + hover suffice); allowed for print |
| Borders | Horizontal only; no vertical rules; outer border comes from the card |
| Empty / loading / error | See [interaction-states.md](interaction-states.md#loading-empty-and-error-proposed-state-matrix-for-data-views) |

### Sorting

- One sort column at a time by default; multi-sort via Shift-click only if the product needs it.
- Default sort: most useful for the task (newest first for logs, name A–Z for directories). State it in the header.
- Sorting is server-side above 1,000 rows.

### Filtering and search

- Search field (sm) at the toolbar left filters the primary text columns; debounce 300ms; show "n results for 'query'".
- Column filters as dropdown chips: `[Status: Active ×]`, chip = `rounded-full border border-[#D0D5DD] px-3 py-1 text-xs`, active chip `bg-[#EEF4FF] text-blue-2 border-blue`.
- "Clear all" link appears when any filter is active.
- Persist filters in the URL query string so views are shareable.

### Pagination

- Verified pattern (News page): 40px round pills, active `bg-blue text-white font-semibold`, others `text-dark-grey`, previous/next chevrons.
- Products: page size 25 default (10 / 25 / 50 / 100), pills for ≤ 7 pages, compact "‹ 3 of 52 ›" beyond that. Show total count. Cursor pagination allowed for infinite logs; never infinite scroll on tables with actions.

### Responsive tables

- ≥ `md`: full table with horizontal scroll inside the card when columns exceed width (sticky first column).
- < `md`: collapse each row into a card list (label–value pairs, primary identifier as the card title, actions in a menu). Hide tertiary columns first; expose them in a row detail drawer.

## Other data displays

| Pattern | Spec |
|---|---|
| Key–value list (detail pages) | Two-column `dl`: `dt` 12px 500 `grey`, `dd` 14px `black`, 12px row gap, 24px column gap; single column on mobile |
| Summary metric cards | [components.md](components.md#summary-metric-cards-stat-cards) |
| Progress bar | 8px tall, `rounded-full`, track `bg-gray-100`, fill `bg-blue` (semantic colors when conveying status); label with percentage right |
| Sparkline in a cell | amCharts micro line, 80 × 24px, no axes, `blue` stroke; see charts file |
| Timeline / activity feed | 32px avatars or 24px icon dots on a 1px `#D0D5DD` line; 14px text, 12px `grey` timestamps |
| Comparison delta | `▲ 12.4%` `success-700` / `▼ 3.1%` `error-red` / `— 0.0%` `grey`; include the comparison basis ("vs last month") |
| Code / IDs | 13px mono, `bg-gray-100 rounded px-1.5` |
| Avatars | `rounded-full`, 24 / 32 / 40px; fallback initials on `bg-blue text-white` |

## Export

Tables offer **Export CSV** (and XLSX when the host project already has a library) from the toolbar's secondary button; export respects current filters and sort, and includes a header row with the same column names as the UI. Chart/report export is covered in [charts-and-visualizations.md](charts-and-visualizations.md#exporting).

## Accessibility

- Use real `<table>` markup with `<th scope="col">`; `<caption>` or `aria-labelledby` naming the table.
- Sortable headers are buttons with `aria-sort`.
- Row selection checkboxes have labels ("Select row for ACME Corp").
- Keyboard: Tab into the table, arrow keys move between focusable cells if implemented as a grid; otherwise rely on native tab order.
- Do not convey status by color alone; badges include text.

## Related files

- [components.md](components.md), [charts-and-visualizations.md](charts-and-visualizations.md), [../examples/dashboard-guidelines.md](../examples/dashboard-guidelines.md)
