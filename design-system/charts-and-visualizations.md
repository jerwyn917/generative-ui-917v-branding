# Charts and Visualizations (amCharts)

917ventures.com contains no charts, so everything here is a **Proposed**
standard that applies the verified brand tokens to **amCharts 5**, the
preferred charting library for all 917 Ventures analytical reports,
dashboards and data visualizations.

Reference: https://www.amcharts.com · Docs: https://www.amcharts.com/docs/v5/

## When to use a chart

| Need | Use |
|---|---|
| Exact values, many attributes, row actions | Table ([tables-and-data-display.md](tables-and-data-display.md)) |
| One headline number with a delta | Summary metric card ([components.md](components.md#summary-metric-cards-stat-cards)) |
| Trend over time | Line or area chart |
| Compare categories | Column (vertical) for ≤ 12 categories; bar (horizontal) for long labels or > 12 |
| Composition of a whole (≤ 6 parts) | Donut (preferred) or pie; otherwise a stacked 100% column or a table |
| Composition over time | Stacked column / stacked area |
| Relationship between two measures | Scatter (bubble for a third measure) |
| Distribution | Histogram (column), box plot |
| Funnel / stages | Funnel (amCharts `percent` module) or a horizontal bar |
| Geography (PH regions etc.) | amCharts Maps (`@amcharts/amcharts5-geodata`), separate license tier |
| Single value vs. target | Gauge or a progress bar; prefer the progress bar in tables |

Do not chart a single number, a two-value comparison, or a list of names — use text, a metric card, or a table. Prefer one clear chart per card over combo charts.

## Library, version and licensing

- **Package:** `@amcharts/amcharts5` (amCharts 5, the current major version). Use the latest 5.x; minor upgrades are free.
- **Modules:** `@amcharts/amcharts5` (core), `@amcharts/amcharts5/xy`, `@amcharts/amcharts5/percent`, `@amcharts/amcharts5/radar`, `@amcharts/amcharts5/hierarchy`, `@amcharts/amcharts5/themes/Animated`, `@amcharts/amcharts5/themes/Responsive`, `@amcharts/amcharts5/plugins/exporting`; maps via `@amcharts/amcharts5/map` + `@amcharts/amcharts5-geodata`.
- **Licensing (per amcharts.com, checked 2026-10-22):** amCharts 5 can be used without a paid license; the free use shows a small amCharts branding link inside the chart. Paid licenses remove the branding and add support. Tiers at the time of checking: **Basic** (public websites without login), **Single App / SaaS** (sites with user logins — the tier most 917 Ventures dashboards and portals fall under), **OEM** (redistributable apps), **Extended OEM**. Annual and perpetual options exist; Maps, Stock and Gantt are licensed separately.
- **Rule:** internal tools and prototypes may run on the free version with the branding visible. Anything customer-facing or behind a login in production needs the appropriate paid license, applied with `am5.addLicense("…")` before creating a Root. Store the key in an environment variable, not in source. Never hide the branding without a license.
- Do not add Chart.js, Recharts, ECharts, Highcharts, D3 charts or similar alongside amCharts. If a project already uses another library, keep it only for existing screens and document the limitation; new analytical screens use amCharts.

## Setup pattern

```ts
import * as am5 from "@amcharts/amcharts5";
import * as am5xy from "@amcharts/amcharts5/xy";
import am5themes_Animated from "@amcharts/amcharts5/themes/Animated";
import am5themes_Responsive from "@amcharts/amcharts5/themes/Responsive";
import { brandTheme } from "./amcharts-brand-theme"; // see below

// React: run inside useLayoutEffect and dispose in the cleanup; keep one Root per container.
const root = am5.Root.new("chartdiv");                 // the div needs an explicit height
root.setThemes([am5themes_Animated.new(root), am5themes_Responsive.new(root), brandTheme(root)]);
root.numberFormatter.set("numberFormat", "#,###.##");
root.dateFormatter.set("dateFormat", "dd MMM yyyy");
root.locale.firstDayOfWeek = 1;

const chart = root.container.children.push(am5xy.XYChart.new(root, {
  panX: false, panY: false, wheelX: "none", wheelY: "none", layout: root.verticalLayout,
}));
// ... axes, series, series.data.setAll(data) last
// cleanup: root.dispose()
```

Rules: create objects with `.new(root, {...})`; set data with `data.setAll()` after configuring the series; call `root.dispose()` on unmount; never create a second Root in the same div.

**No-build pages and artifacts** (no bundler): load the script builds from cdnjs, which define globals, and call them exactly as above with `am5`, `am5xy`, `am5percent`, `am5themes_Animated`, `am5themes_Responsive`:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/index.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/xy.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/percent.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/themes/Animated.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/themes/Responsive.js"></script>
```

`index.js` must come first. Do not load amCharts through jsDelivr's `+esm` bundles: each entry bundles its own copy of the core, so `am5.color()` and `am5.percent()` values are not recognized by the chart modules. Full artifact rules: [../examples/generative-ui-artifact.md](../examples/generative-ui-artifact.md).

## Brand theme (apply to every chart)

```ts
import * as am5 from "@amcharts/amcharts5";

export function brandTheme(root: am5.Root) {
  const theme = am5.Theme.new(root);

  // Series colors: categorical order (see palette table below)
  theme.rule("ColorSet").set("colors", [
    am5.color(0x407DC3), am5.color(0xDB6E4F), am5.color(0x3A6DBA), am5.color(0x52BCD5),
    am5.color(0xDA4E8F), am5.color(0x8EA1BD), am5.color(0x142741), am5.color(0xB45309),
  ]);
  theme.rule("ColorSet").set("reuse", false);

  // Typography
  theme.rule("Label").setAll({ fontFamily: "Montserrat, Arial, sans-serif", fontSize: 12, fill: am5.color(0x606471) });
  theme.rule("Label", ["axis"]).setAll({ fontSize: 12, fill: am5.color(0x606471) });
  theme.rule("Label", ["legend"]).setAll({ fontSize: 12, fill: am5.color(0x323643) });

  // Axes and grid
  theme.rule("Grid").setAll({ stroke: am5.color(0xD0D5DD), strokeOpacity: 0.6, strokeWidth: 1 });
  theme.rule("AxisRenderer").setAll({ stroke: am5.color(0xD0D5DD), strokeOpacity: 1 });
  theme.rule("AxisTick").setAll({ visible: false });

  // Tooltips
  theme.rule("Tooltip").setAll({ getFillFromSprite: false });
  theme.rule("Tooltip").get("background")?.setAll({ fill: am5.color(0x142741), fillOpacity: 1, stroke: am5.color(0x142741), cornerRadius: 8 });
  theme.rule("Label", ["tooltip"]).setAll({ fill: am5.color(0xFFFFFF), fontSize: 12 });

  // Marks
  theme.rule("RoundedRectangle", ["series", "column"]).setAll({ cornerRadiusTL: 4, cornerRadiusTR: 4, strokeOpacity: 0 });
  theme.rule("LineSeries").setAll({ strokeWidth: 2 });
  theme.rule("Legend").setAll({ useDefaultMarker: true });
  theme.rule("RoundedRectangle", ["legend", "marker"]).setAll({ cornerRadiusTL: 4, cornerRadiusTR: 4, cornerRadiusBL: 4, cornerRadiusBR: 4 });

  return theme;
}
```

(The tooltip background is set through `tooltip.get("background")` at chart level if the theme rule is not sufficient in your version; both are supported patterns.)

## Applying the palette to series

### Categorical (distinct series)

| Order | Token | HEX | Notes |
|---|---|---|---|
| 1 | `blue` | `#407DC3` | Primary series / "this period" |
| 2 | `pink-1` | `#DB6E4F` | Second series, warm contrast |
| 3 | `blue-2` | `#3A6DBA` | Deeper blue |
| 4 | `secondary-blue` | `#52BCD5` | Cyan |
| 5 | `pink-2` | `#DA4E8F` | Pink |
| 6 | `soft-steel-blue` | `#8EA1BD` | Muted |
| 7 | `navy` | `#142741` | Dark |
| 8 | `warning-700` | `#B45309` | Amber |

Stop at 8 series; group the rest as "Other" (`#8D919F`). Keep the same color for the same entity across all charts on a page.

### Sequential (one measure, light → dark)

`#E3EDFF` → `#A7C4E8` → `#6FA0D9` → `#407DC3` → `#3A6DBA` → `#142741`. Use for heatmaps, choropleths, and ordered buckets.

### Diverging (negative ↔ positive)

`#DB6E4F` (negative) → `#F3F4F6` (neutral) → `#407DC3` (positive). Do not use red/green for diverging data; the brand coral/blue pair reads correctly for color-blind users.

### Semantic

Positive deltas, targets met: `success-700 #1E7A3C`. Negative, breaches: `error-red #D92D20`. Thresholds / targets lines: `#B45309` dashed. Comparison period (last year): same hue at 40% opacity or a dashed `#8EA1BD` line.

### Gradients

The brand gradient (`#65DDF5 → #3A6DBA`) may fill a single-series area chart (top opacity 0.35 → 0) or a single-series column chart on a report cover. Never use gradients on multi-series charts or anything that must be read precisely.

## Chart types: specifications

| Chart | amCharts class | Rules |
|---|---|---|
| Line | `am5xy.LineSeries` | 2px stroke; bullets (4px circle) only when ≤ 30 points; `DateAxis` with `baseInterval`; smoothing `tensionX` 0.8 allowed only for ≥ 50 points; ≤ 5 lines |
| Area | `LineSeries` with `fills` visible | Single series: brand gradient fill 0.35 → 0; stacked: solid fills at 0.8 opacity with 1px white separators |
| Column | `am5xy.ColumnSeries` | Width 60–70% of the cell (`width: am5.percent(65)`), 4px top radius, value labels optional (12px, above); clustered ≤ 4 series |
| Bar (horizontal) | `ColumnSeries` with swapped axes | Sort descending unless the order is meaningful; labels left-aligned, truncate at 24 chars with tooltip |
| Stacked / 100% stacked | `ColumnSeries` + `stacked: true` (+ `calculateTotals` and a `valueAxis` with `calculateTotals`) | Order segments consistently; total label on top for stacked; use 100% stacked for share questions |
| Pie / donut | `am5percent.PieChart` + `PieSeries` | Donut preferred (`innerRadius: am5.percent(60)`), center label with the total in Unbounded 700 20px; ≤ 6 slices; labels outside with ticks, or legend only below 480px; no 3D, no exploded slices |
| Scatter / bubble | `LineSeries` with `strokeOpacity: 0` and bullets | 6–10px circles at 0.7 opacity; bubble radius scaled by area, max 40px; trend line optional, dashed `#8EA1BD` |
| Heatmap | `ColumnSeries` on two `CategoryAxis` with heat rules | Sequential palette; cell labels only when cells ≥ 32px |
| Funnel | `am5percent.FunnelSeries` | Sequential blues top → bottom; show count and % of top |
| Gauge | `am5radar.RadarChart` + `ClockHand` | Only for a single KPI vs target; bands in `#F3F4F6` / `#E3EDFF` / `#407DC3` |
| Sparkline | `LineSeries`, no axes/grid/legend, `chart.plotContainer` only | 80 × 24px, `blue` stroke, last point bullet |
| Map | `am5map.MapChart` with Philippines geodata | Sequential palette; Maps license required for production |

## Typography, labels, legends, axes, tooltips, grid

- **Font:** Montserrat for everything inside charts (Unbounded only for a donut center total or a big-number annotation).
- **Chart title:** outside the chart, in the card header (16px 700). Subtitle / unit (12px `grey`) beside it, e.g. "Revenue, ₱ millions".
- **Axis labels:** 12px `grey`; value axis on the left with `minGridDistance: 30`; category axis labels rotate to −45° only when overlapping, otherwise show every nth; date axis uses "MMM yy" or "dd MMM" depending on span.
- **Axis lines:** `#D0D5DD`; no ticks; value axis starts at zero for columns/bars (never truncate); lines may auto-scale, say so in the subtitle.
- **Grid:** horizontal lines only, `#D0D5DD` at 60% opacity; no vertical grid on time series.
- **Legend:** below the chart (`centerX: 50%`, `x: 50%`), 12px, rounded 4px markers, clickable to toggle series; hide the legend for single-series charts.
- **Tooltip:** navy `#142741` background, white 12px text, 8px radius; shared tooltip (`XYCursor` with `behavior: "none"`) on time series listing every series; format: `{name}: {valueY.formatNumber('#,###')}`; include the unit.
- **Data labels:** off by default; on for ≤ 8 columns or when the chart is for print.
- **Annotations:** target/threshold with `AxisRange` (dashed `#B45309` 1px) and a 12px label; events as vertical ranges at 10% opacity.

## Number, date and currency formatting

| Type | Format | amCharts |
|---|---|---|
| Integer counts | `1,284,900` | `#,###` |
| Decimals | 1–2 places, consistent per axis | `#,###.##` |
| Large numbers on axes | `1.2M`, `850K` | `#.#a` with `bigNumberPrefixes` `[{number:1e3,suffix:"K"},{number:1e6,suffix:"M"},{number:1e9,suffix:"B"}]` |
| Currency (PHP) | `₱1,284,900.00`; axes `₱1.2M` | `root.numberFormatter.set("numberFormat", "'₱'#,###.00")`; use `PHP` code in tables for exports |
| Currency (other) | ISO code before the number in tables (`USD 12,000`), symbol in charts | — |
| Percent | `12.4%`, one decimal; 0 decimals when all values are integers | `#.0'%'` (values already ×100) or `#.0%` |
| Dates | `22 Oct 2026`; axes `Oct 26` / `22 Oct`; months `Oct 2026` | `dd MMM yyyy`, `MMM yy` |
| Time | `14:30` 24-hour, Asia/Manila unless the product is multi-region | `HH:mm` |
| Null / missing | Gap in lines (`connect: false`), "—" in tables and tooltips | — |

Use the host app's locale utility if one exists; otherwise `Intl.NumberFormat("en-PH")`.

## Responsive layouts and mobile behavior

- Chart container: `width: 100%`, explicit height (desktop 320–400px, mobile 240–280px, sparklines fixed). Never leave the div without a height.
- Use `am5themes_Responsive` and add rules: below 480px hide the value-axis title, reduce bullets, move the legend below and truncate labels; below 360px switch donut labels to legend-only.
- Columns: ≤ 6 categories on mobile, else switch to horizontal bars.
- Disable wheel zoom in cards (`wheelX/wheelY: "none"`); enable `panX` + a `XYChartScrollbar` only for long time series on desktop.
- Touch: tooltips on tap; `cursor.lineX` visible; ensure legend items are ≥ 32px tall.
- Print / export: 2× pixel ratio, white background, no animations.

## Loading, empty, error and no-data states

| State | Treatment |
|---|---|
| Loading | Skeleton block the size of the chart (`#EBEBEB` shimmer) with the card title visible; do not render an empty amCharts Root |
| Empty (no data) | Replace the chart area with the empty-state block: 48px `ChartBarIcon` outline in `light-grey`, "No data for this period", helper text, "Change date range" secondary button |
| Partial data | Render the chart; add a 12px `grey` note "Data available from 03 Mar 2026"; gaps are shown, not interpolated |
| Error | Inline error alert in the card with "Retry"; keep the previous chart visible and dimmed (opacity 0.5) if it existed |
| Stale | Info text in the card header: "Updated 10:42" |

## Interactivity

- **Tooltips** always; **legend toggle** for multi-series; **hover highlight** dims other series to 0.3 opacity.
- **Zoom / pan:** only on time series with > 90 points, via scrollbar; provide a "Reset zoom" ghost button.
- **Drill-down:** click a column/slice to load the next level; show breadcrumbs above the chart ("All regions › NCR"); animate 300ms; keep colors stable.
- **Cross-filtering** between charts on a dashboard is allowed when it is obvious and reversible (chip in the filter bar shows the applied selection).
- Animations: `am5themes_Animated` with 500ms on first render only; disable under `prefers-reduced-motion` (`root.setThemes` without Animated, or `root.interfaceColors`/`am5.ready` check).

## Accessibility and readability

- Enable amCharts accessibility: `series.set("tooltipText")` plus `focusable: true` on series/bullets and columns, `ariaLabel` templates (e.g. `columns.template.set("ariaLabel", "{categoryX}: {valueY}")`), `chart.set("ariaLabel", ...)`. Themes do not set these; add them per chart.
- Provide a text alternative: a "View as table" toggle or a visually hidden `<table>` with the same data for every chart in reports.
- Contrast: series must reach 3:1 against white and against adjacent series; the categorical order above is checked for this. Do not rely on color alone — use direct labels, markers (dashed vs solid), or a legend.
- Minimum text 12px; minimum stroke 2px; minimum bullet 6px.
- Color-blind check: coral/blue and the sequential blues are distinguishable under deuteranopia and protanopia; avoid red/green pairs.

## Exporting

- Use `@amcharts/amcharts5/plugins/exporting`: `am5plugins_exporting.Exporting.new(root, { menu: am5plugins_exporting.ExportingMenu.new(root, {}), filePrefix: "917v-<report>-<yyyy-mm-dd>", pngOptions: { quality: 1, pageSize: null, minWidth: 1200 }, csvOptions: { addColumnNames: true }, dataSource: data })`.
- Offer **PNG** (2× scale, white background) and **CSV/XLSX** of the underlying data; PDF is produced at the report level (see [../examples/report-guidelines.md](../examples/report-guidelines.md)), embedding chart PNGs.
- Place the export control in the card header as a ghost icon button (`ArrowDownTrayIcon`) with a menu, or route it through the page-level "Export report" action; do not show amCharts' default floating menu inside the plot.
- Exported images include the chart title, unit, source and date stamp as `exporting.set("extraImages")` or as part of the surrounding card capture.

## Performance

- Keep ≤ 5,000 rendered points per chart; aggregate server-side (daily → weekly → monthly) based on the selected range, and tell the user the granularity.
- For large series: `series.set("minBulletDistance", 10)`, disable bullets, `connect: false` for gaps, and use `DateAxis` grouping (`groupData: true`, `groupCount: 500`).
- Lazy-load charts below the fold (`IntersectionObserver`) and dispose Roots on unmount or tab change; cap concurrent Roots to ~6 per page.
- Reuse one Root per chart; update with `data.setAll()` instead of recreating.
- Disable animations when more than 4 charts render at once.
- Tree-shake by importing only the modules you use; load `@amcharts/amcharts5-geodata` per region file.

## Checklist before shipping a chart

- [ ] Right chart type for the question; a table or metric card was considered first
- [ ] Brand theme applied; series colors in the categorical order; same entity = same color across the page
- [ ] Title, unit, period and source present; axis starts at zero for columns
- [ ] Formats: thousands separators, ₱/%, `dd MMM yyyy`
- [ ] Loading skeleton, empty state, error state implemented
- [ ] Responsive height and legend behavior checked at 360px and 1440px
- [ ] Tooltip and keyboard focus work; table alternative available
- [ ] Export (PNG + CSV) available where the spec requires it
- [ ] License key set for production; no competing chart library added

## Related files

- [../brand/color-palette.md](../brand/color-palette.md), [design-tokens.md](design-tokens.md), [tables-and-data-display.md](tables-and-data-display.md), [../examples/dashboard-guidelines.md](../examples/dashboard-guidelines.md), [../examples/report-guidelines.md](../examples/report-guidelines.md)
