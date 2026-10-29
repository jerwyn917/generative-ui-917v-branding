# amCharts Recipes for Widgets and Artifacts

Every amCharts 5 chart family is available to in-chat widgets and artifacts
through the cdnjs script builds, not only columns and lines. This file lists
the modules, when to use each chart type, the shared setup, a recipe per type,
and a verified widget that switches between ten types. Everything is
**Proposed** (verified to render on 2026-10-29 in a chat-widget harness at
720px and 375px, no console errors); chart choice rules come from
[../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md).

## 1. Modules on cdnjs (amCharts 5.21.5)

Base path: `https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/`. Load `index.js` first, then only the modules the view uses, then your inline script.

| File | Global | Chart types |
|---|---|---|
| `index.js` | `am5` | Root, `am5.color`, `am5.percent`, `am5.Label`, `am5.Tooltip`, `am5.Container` (always) |
| `xy.js` | `am5xy` | column, bar, grouped, stacked, line, area, step, scatter, bubble, heatmap, range, candlestick |
| `percent.js` | `am5percent` | pie, donut, funnel, pyramid, pictorial |
| `radar.js` | `am5radar` | radar, polar, gauge (also needs `xy.js`) |
| `hierarchy.js` | `am5hierarchy` | treemap, sunburst, partition, pack, force-directed, tree |
| `flow.js` | `am5flow` | sankey, chord, arc diagram |
| `map.js` + `geodata/philippinesLow.js` or `geodata/worldLow.js` | `am5map`, `am5geodata_philippinesLow`, `am5geodata_worldLow` | choropleth, bubble map |
| `timeline.js`, `venn.js`, `wc.js` | `am5timeline`, `am5venn`, `am5wc` | timeline, venn, word cloud (rare) |
| `stock.js` | `am5stock` | financial panels (artifacts only; heavy) |
| `themes/Responsive.js` | `am5themes_Responsive` | always |
| `themes/Animated.js` | `am5themes_Animated` | artifacts only, skipped under reduced motion; never in widgets |
| `locales/en.js` | `am5locales_en` | optional; number and date formats are set per axis instead |

Not used: `themes/Dark.js` (dark mode comes from the host or brand tokens), `plugins/exporting.js` (downloads do nothing in artifacts or widgets). `plugins/regression.js` is not on cdnjs.

Rules: pin the version; check the globals before drawing; keep `paddingBottom: 20` on charts for the free-build watermark; dispose the previous `am5.Root` before drawing again into the same element; never load jsDelivr `+esm` bundles.

## 2. Pick the type

| Question in the data | Chart | Module |
|---|---|---|
| How do categories compare? | Column (vertical) or bar (horizontal, long labels) | xy |
| How do two series compare per category? | Grouped columns | xy |
| How do parts add up per category? | Stacked columns; 100% stacked for shares | xy |
| How does a value move over time? | Line; area for one series; step for rates | xy |
| Share of a whole (≤ 6 slices)? | Donut with the total in the center | percent |
| Stage-to-stage drop-off? | Funnel | percent |
| Progress against a target? | Gauge (half ring) or a metric card | radar |
| Proportions with hierarchy? | Treemap or sunburst | hierarchy |
| Flow from sources to targets? | Sankey; chord for many-to-many | flow |
| Intensity across two dimensions? | Heatmap | xy |
| Where, by province? | Choropleth on `philippinesLow` | map |
| Relationship between two measures? | Scatter; bubble for a third | xy |
| Several measures per item? | Radar | radar |

Prefer the simplest form; a metric card beats a one-bar chart and a table beats a chart with more than eight series.

## 3. Shared setup

```js
const SERIES = ["#407DC3", "#DB6E4F", "#3A6DBA", "#52BCD5", "#DA4E8F", "#8EA1BD", "#142741", "#B45309"];
const html = document.documentElement;
const isDark = html.dataset.mode ? html.dataset.mode === "dark" : matchMedia("(prefers-color-scheme: dark)").matches;
const ink = { text: isDark ? "#c3c2b7" : "#52514e", grid: isDark ? "#2c2c2a" : "#e1e0d9", surface: isDark ? "#1a1a19" : "#fcfcfb" };
const col = h => am5.color(h);
const root = am5.Root.new("v917-chart");
root.setThemes([am5themes_Responsive.new(root)]);
```

In artifacts read `ink` from the brand tokens instead (`getComputedStyle(document.documentElement).getPropertyValue("--muted")`), add `am5themes_Animated` unless `prefers-reduced-motion`, and keep everything else the same. Axis labels and grid use `ink`; series use `SERIES` in order; slices, stacked segments and heatmap cells get a 2px `ink.surface` stroke as the gap between marks. Legends are HTML next to the chart (small colored squares, 12px), not amCharts legends, so they follow the host theme.

## 4. Recipes

Each recipe is the `draw(root)` function of the matching type in the widget in section 5. What to change and what to watch:

- **Columns / grouped** (`am5xy.ColumnSeries` on a `CategoryAxis` + `ValueAxis`): one series per measure, `width: am5.percent(70)`, `maxWidth: 24`, rounded top corners, value axis `min: 0` and `numberFormat: "₱#M"`. Rotate category labels −45° when the chart is narrower than 520px. Horizontal bars: swap the axes (`CategoryAxis` on Y with `inversed: true`, `ValueAxis` on X, `valueXField` / `categoryYField`).
- **Stacked** (`stacked: true` on each `ColumnSeries`): segments separated by a 2px surface-colored stroke; for 100% stacking set `calculateTotals: true` on the value axis and `valueYShow: "valueYTotalPercent"` on each series.
- **Line / area** (`am5xy.LineSeries`): 2px stroke, area fill at 10% opacity, 4px bullets with a surface ring, `XYCursor` with `behavior: "none"` for the crosshair. For real dates use `am5xy.DateAxis.new(root, { baseInterval: { timeUnit: "month", count: 1 }, renderer })` with `valueXField` holding timestamps and `dateFormats`/`tooltipDateFormat` set to `"dd MMM yyyy"`.
- **Donut** (`am5percent.PieChart` + `PieSeries`, `innerRadius: am5.percent(60)`): ≤ 6 slices, colors from `SERIES`, labels inside (`alignLabels: false`, ticks hidden), total in the center as an `am5.Label` pushed to `chart.seriesContainer`. Legend with percentages in HTML.
- **Funnel** (`am5percent.SlicedChart` + `FunnelSeries`, `orientation: "vertical"`, `bottomRatio: 1`): labels "{category}: {value}", links at 30% opacity. Pyramid: `PyramidSeries` with the same options.
- **Gauge** (`am5radar.RadarChart`, `startAngle: 180`, `endAngle: 360`, `innerRadius: am5.percent(70)`): a `ValueAxis` 0–100 with `strictMinMax`, two axis ranges (track in `ink.grid`, progress in brand blue), the value as a label at the bottom center. Needs `xy.js` for the axis classes.
- **Treemap** (`am5hierarchy.Treemap`, `initialDepth: 1`): data as one root node with `children`; `nodePaddingInner: 2`, rounded rectangles, white labels. Sunburst: `am5hierarchy.Sunburst` with the same data.
- **Sankey** (`am5flow.Sankey`, `sourceIdField` / `targetIdField` / `valueField`): `nodeWidth: 10`, `nodePadding: 16`, links at 35% opacity, `paddingRight` so target labels fit. Chord: `am5flow.Chord` with the same data.
- **Heatmap** (`ColumnSeries` on two `CategoryAxis`, `calculateAggregates: true`): cells `width`/`height: am5.percent(100)`, `heatRules` from `#D4E0ED` to brand blue-2 on `fill`, grid hidden, X axis `opposite: true`, Y axis `inversed: true`.
- **Map** (`am5map.MapChart` with `geoMercator`, `MapPolygonSeries` with `geoJSON: am5geodata_philippinesLow`): data rows `{ id, value }` keyed by ISO 3166-2 province codes (82 features: `PH-00` Metropolitan Manila, `PH-CEB` Cebu, `PH-DAS` Davao del Sur, `PH-LAG` Laguna, `PH-CAV` Cavite, `PH-PAM` Pampanga, `PH-ILI` Iloilo, `PH-BUL` Bulacan, …; read `am5geodata_philippinesLow.features[i].id` for the rest). Heat rule as the heatmap; polygons without data stay `ink.grid`. The country is tall, so give the container at least 440px; the chart fits the geodata on its own, no `goHome` needed. Disable pan and wheel in widgets.
- **Scatter / bubble** (`am5xy.LineSeries` with `strokeOpacity: 0` and circle bullets; `valueField` + `heatRules` on `radius` for bubbles) on two `ValueAxis`. **Radar** (`am5radar.RadarChart` + `am5radar.RadarLineSeries` on a `CategoryAxis` with `AxisRendererCircular`). **Word cloud** (`am5wc.WordCloud`, `wc.js`). Same setup, same colors.

## 5. Verified multi-type widget

One container, a chart-type control, ten renderers keyed by name. Switching disposes the previous root and draws the next type; each type sets its own note and legend. Use it as a test page or copy single `draw` functions into the starter in [interactive-chat-widget.md](interactive-chat-widget.md#5-starter-widget-verified-2026-10-29). In a real widget keep only the types the user needs, and load only their modules.

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Unbounded:wght@600&display=swap">
<style>
.v917 { font-size: 14px; color: var(--text-primary); }
.v917 .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); }
.v917 .display { font-family: Unbounded, "Arial Black", Arial, sans-serif; font-weight: 600; }
.v917 .seg { display: flex; flex-wrap: wrap; gap: 6px; }
.v917 .seg button { padding: 6px 12px; font-size: 13px; }
.v917 .seg button[aria-pressed="true"] { background: var(--bg-accent); color: var(--text-accent); border-color: var(--border-accent); }
.v917 .legend { display: flex; flex-wrap: wrap; gap: 12px; font-size: 12px; color: var(--text-secondary); margin: 0 0 8px; }
.v917 .legend i { display: inline-block; width: 10px; height: 10px; border-radius: 2px; margin-right: 4px; vertical-align: -1px; }
</style>
<div class="v917">
<h2 class="sr-only">Sample 917 Ventures portfolio data shown as a selectable chart type: column, stacked, line, donut, funnel, gauge, treemap, sankey, heatmap or map.</h2>
<div style="display:flex; flex-wrap:wrap; align-items:center; justify-content:space-between; gap:12px; margin:0 0 1rem;">
  <div style="display:flex; align-items:center; gap:8px;">
    <span class="display" style="font-size:13px; color:#407DC3; letter-spacing:0.02em;">917 Ventures</span>
    <span style="font-size:13px; color:var(--text-secondary);">Portfolio 2026 · ₱ millions · sample data</span>
  </div>
</div>
<div class="seg" role="group" aria-label="Chart type" id="v917-types" style="margin:0 0 1rem;"></div>
<div class="legend" id="v917-legend"></div>
<div id="v917-chart" style="width:100%; height:300px;" role="img" aria-label="Chart of sample portfolio data"></div>
<p id="v917-note" style="font-size:12px; color:var(--text-muted); margin:8px 0 0;"></p>
<div style="display:flex; flex-wrap:wrap; gap:8px; margin:1rem 0 0;">
  <button type="button" onclick="sendPrompt('Explain what the current 917 Ventures chart shows and what stands out')">Explain this view ↗</button>
  <button type="button" onclick="sendPrompt('Turn the current 917 Ventures chart into a full dashboard artifact')">Open as full dashboard ↗</button>
</div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/index.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/xy.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/percent.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/radar.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/hierarchy.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/flow.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/map.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/geodata/philippinesLow.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/themes/Responsive.js"></script>
<script>
(function () {
  const STATE = { type: "column" };
  const SERIES = ["#407DC3", "#DB6E4F", "#3A6DBA", "#52BCD5", "#DA4E8F", "#8EA1BD", "#142741", "#B45309"];
  const SECTORS = [
    { sector: "Fintech", h1: 120.4, h2: 138.9, share: 38 },
    { sector: "Health", h1: 64.2, h2: 71.5, share: 20 },
    { sector: "Logistics", h1: 52.8, h2: 58.1, share: 16 },
    { sector: "Media", h1: 41.0, h2: 39.6, share: 12 },
    { sector: "Retail", h1: 30.3, h2: 36.4, share: 9 },
    { sector: "Other", h1: 14.9, h2: 17.2, share: 5 }
  ];
  const MONTHS = ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct"];
  const TREND = [42.1, 45.8, 51.2, 48.9, 55.4, 58.0, 61.3, 57.7, 64.9, 68.2];
  const FUNNEL = [{ stage: "Leads", value: 1200 }, { stage: "Qualified", value: 640 }, { stage: "Pilots", value: 210 }, { stage: "Contracts", value: 85 }];
  const FLOWS = [{ from: "Fintech", to: "Luzon", value: 80 }, { from: "Fintech", to: "Visayas", value: 35 }, { from: "Fintech", to: "Mindanao", value: 24 }, { from: "Health", to: "Luzon", value: 40 }, { from: "Health", to: "Visayas", value: 18 }, { from: "Logistics", to: "Luzon", value: 30 }, { from: "Logistics", to: "Mindanao", value: 22 }];
  const REGIONS = { "PH-00": 182, "PH-CEB": 96, "PH-DAS": 74, "PH-LAG": 61, "PH-CAV": 48, "PH-PAM": 37, "PH-ILI": 28, "PH-BUL": 22 };
  const HEAT = [];
  SECTORS.slice(0, 5).forEach((s, i) => ["Q1", "Q2", "Q3", "Q4"].forEach((q, j) => HEAT.push({ sector: s.sector, quarter: q, value: Math.round((s.h1 / 2) * (0.8 + 0.1 * j) + i * 3) })));

  const html = document.documentElement;
  const isDark = html.dataset.mode ? html.dataset.mode === "dark" : matchMedia("(prefers-color-scheme: dark)").matches;
  const ink = { text: isDark ? "#c3c2b7" : "#52514e", grid: isDark ? "#2c2c2a" : "#e1e0d9", surface: isDark ? "#1a1a19" : "#fcfcfb" };
  const $ = id => document.getElementById(id);
  const col = h => am5.color(h);

  function legend(items) {
    $("v917-legend").innerHTML = items.map(x => '<span><i style="background:' + x.color + '"></i>' + x.label + '</span>').join("");
  }
  const narrow = () => $("v917-chart").clientWidth < 520;
  function styleAxis(renderer, gridVisible, categoriesX) {
    renderer.labels.template.setAll({ fill: col(ink.text), fontSize: 12 });
    if (categoriesX && narrow()) renderer.labels.template.setAll({ rotation: -45, centerY: am5.p50, centerX: am5.p100 });
    renderer.grid.template.setAll({ stroke: col(ink.grid), strokeOpacity: 1, visible: gridVisible });
  }
  function xyChart(root, opts) {
    return root.container.children.push(am5xy.XYChart.new(root, Object.assign({ panX: false, panY: false, wheelX: "none", wheelY: "none", paddingLeft: 0, paddingRight: 0, paddingBottom: 20 }, opts || {})));
  }

  const TYPES = {
    column: { label: "Columns", note: "Grouped columns: H1 vs H2 by sector", draw(root) {
      const chart = xyChart(root);
      const xr = am5xy.AxisRendererX.new(root, { minGridDistance: 30 }); styleAxis(xr, false, true);
      const x = chart.xAxes.push(am5xy.CategoryAxis.new(root, { categoryField: "sector", renderer: xr }));
      const yr = am5xy.AxisRendererY.new(root, {}); styleAxis(yr, true);
      const y = chart.yAxes.push(am5xy.ValueAxis.new(root, { min: 0, numberFormat: "₱#M", renderer: yr }));
      x.data.setAll(SECTORS);
      [["h1", "H1 2026", SERIES[0]], ["h2", "H2 2026", SERIES[1]]].forEach(([f, name, c]) => {
        const s = chart.series.push(am5xy.ColumnSeries.new(root, { name, xAxis: x, yAxis: y, valueYField: f, categoryXField: "sector", fill: col(c), stroke: col(c) }));
        s.columns.template.setAll({ width: am5.percent(70), maxWidth: 24, cornerRadiusTL: 4, cornerRadiusTR: 4, tooltipText: "{name} · {categoryX}: ₱{valueY}M" });
        s.data.setAll(SECTORS);
      });
      legend([{ label: "H1 2026", color: SERIES[0] }, { label: "H2 2026", color: SERIES[1] }]);
    } },
    stacked: { label: "Stacked", note: "Stacked columns: H1 + H2 by sector", draw(root) {
      const chart = xyChart(root);
      const xr = am5xy.AxisRendererX.new(root, { minGridDistance: 30 }); styleAxis(xr, false, true);
      const x = chart.xAxes.push(am5xy.CategoryAxis.new(root, { categoryField: "sector", renderer: xr }));
      const yr = am5xy.AxisRendererY.new(root, {}); styleAxis(yr, true);
      const y = chart.yAxes.push(am5xy.ValueAxis.new(root, { min: 0, numberFormat: "₱#M", renderer: yr }));
      x.data.setAll(SECTORS);
      [["h1", "H1 2026", SERIES[0]], ["h2", "H2 2026", SERIES[3]]].forEach(([f, name, c]) => {
        const s = chart.series.push(am5xy.ColumnSeries.new(root, { name, stacked: true, xAxis: x, yAxis: y, valueYField: f, categoryXField: "sector", fill: col(c), stroke: col(ink.surface), tooltip: am5.Tooltip.new(root, { labelText: "{name} · {categoryX}: ₱{valueY}M" }) }));
        s.columns.template.setAll({ width: am5.percent(60), maxWidth: 24, strokeWidth: 2 });
        s.data.setAll(SECTORS);
      });
      legend([{ label: "H1 2026", color: SERIES[0] }, { label: "H2 2026", color: SERIES[3] }]);
    } },
    line: { label: "Line", note: "Monthly revenue trend with area fill", draw(root) {
      const chart = xyChart(root);
      const xr = am5xy.AxisRendererX.new(root, { minGridDistance: 28 }); styleAxis(xr, false, true);
      const x = chart.xAxes.push(am5xy.CategoryAxis.new(root, { categoryField: "m", renderer: xr }));
      const yr = am5xy.AxisRendererY.new(root, {}); styleAxis(yr, true);
      const y = chart.yAxes.push(am5xy.ValueAxis.new(root, { min: 0, numberFormat: "₱#M", renderer: yr }));
      const data = MONTHS.map((m, i) => ({ m, v: TREND[i] }));
      x.data.setAll(data);
      const s = chart.series.push(am5xy.LineSeries.new(root, { name: "Revenue", xAxis: x, yAxis: y, valueYField: "v", categoryXField: "m", stroke: col(SERIES[0]), fill: col(SERIES[0]), tooltip: am5.Tooltip.new(root, { labelText: "{categoryX}: ₱{valueY}M" }) }));
      s.strokes.template.setAll({ strokeWidth: 2 });
      s.fills.template.setAll({ fillOpacity: 0.1, visible: true });
      s.bullets.push(() => am5.Bullet.new(root, { sprite: am5.Circle.new(root, { radius: 4, fill: col(SERIES[0]), stroke: col(ink.surface), strokeWidth: 2 }) }));
      s.data.setAll(data);
      chart.set("cursor", am5xy.XYCursor.new(root, { behavior: "none" }));
      legend([]);
    } },
    donut: { label: "Donut", note: "Share of portfolio revenue by sector", draw(root) {
      const chart = root.container.children.push(am5percent.PieChart.new(root, { innerRadius: am5.percent(60), paddingBottom: 20 }));
      const s = chart.series.push(am5percent.PieSeries.new(root, { valueField: "share", categoryField: "sector", alignLabels: false }));
      s.get("colors").set("colors", SERIES.map(col));
      s.slices.template.setAll({ stroke: col(ink.surface), strokeWidth: 2, tooltipText: "{category}: {value}%" });
      s.labels.template.setAll({ text: "{category}", fill: col(ink.text), fontSize: 12, radius: 8 });
      s.ticks.template.set("visible", false);
      s.data.setAll(SECTORS);
      const total = SECTORS.reduce((a, b) => a + b.h2, 0);
      chart.seriesContainer.children.push(am5.Label.new(root, { text: "₱" + Math.round(total) + "M", centerX: am5.percent(50), centerY: am5.percent(50), fontSize: 20, fontWeight: "600", fill: col(ink.text) }));
      legend(SECTORS.map((d, i) => ({ label: d.sector + " " + d.share + "%", color: SERIES[i] })));
    } },
    funnel: { label: "Funnel", note: "Deal pipeline: leads to contracts", draw(root) {
      const chart = root.container.children.push(am5percent.SlicedChart.new(root, { layout: root.verticalLayout, paddingBottom: 20 }));
      const s = chart.series.push(am5percent.FunnelSeries.new(root, { alignLabels: true, orientation: "vertical", valueField: "value", categoryField: "stage", bottomRatio: 1 }));
      s.get("colors").set("colors", [SERIES[0], SERIES[2], SERIES[3], SERIES[5]].map(col));
      s.slices.template.setAll({ stroke: col(ink.surface), strokeWidth: 2, tooltipText: "{category}: {value}" });
      s.labels.template.setAll({ text: "{category}: {value}", fill: col(ink.text), fontSize: 12 });
      s.links.template.setAll({ fillOpacity: 0.3 });
      s.data.setAll(FUNNEL);
      legend([]);
    } },
    gauge: { label: "Gauge", note: "Progress to annual target: 72%", draw(root) {
      const chart = root.container.children.push(am5radar.RadarChart.new(root, { startAngle: 180, endAngle: 360, innerRadius: am5.percent(70), paddingBottom: 0 }));
      const ar = am5radar.AxisRendererCircular.new(root, { strokeOpacity: 0 });
      ar.labels.template.setAll({ fill: col(ink.text), fontSize: 12 });
      ar.grid.template.set("visible", false);
      const axis = chart.xAxes.push(am5xy.ValueAxis.new(root, { min: 0, max: 100, strictMinMax: true, renderer: ar }));
      const track = axis.createAxisRange(axis.makeDataItem({ value: 0, endValue: 100 }));
      track.get("axisFill").setAll({ visible: true, fill: col(ink.grid), fillOpacity: 1 });
      const prog = axis.createAxisRange(axis.makeDataItem({ value: 0, endValue: 72 }));
      prog.get("axisFill").setAll({ visible: true, fill: col(SERIES[0]), fillOpacity: 1 });
      chart.radarContainer.children.push(am5.Label.new(root, { text: "72%", centerX: am5.percent(50), centerY: am5.percent(100), fontSize: 28, fontWeight: "600", fill: col(ink.text) }));
      legend([{ label: "Achieved ₱415M of ₱576M", color: SERIES[0] }]);
    } },
    treemap: { label: "Treemap", note: "Revenue by sector, area = ₱M", draw(root) {
      const container = root.container.children.push(am5.Container.new(root, { width: am5.percent(100), height: am5.percent(100), layout: root.verticalLayout }));
      const s = container.children.push(am5hierarchy.Treemap.new(root, { singleBranchOnly: false, downDepth: 1, upDepth: 0, initialDepth: 1, valueField: "value", categoryField: "name", childDataField: "children", nodePaddingOuter: 0, nodePaddingInner: 2 }));
      s.get("colors").set("colors", SERIES.map(col));
      s.rectangles.template.setAll({ strokeWidth: 2, stroke: col(ink.surface), cornerRadiusTL: 4, cornerRadiusTR: 4, cornerRadiusBL: 4, cornerRadiusBR: 4, tooltipText: "{category}: ₱{sum}M" });
      s.labels.template.setAll({ fontSize: 12, fill: am5.color("#ffffff"), text: "{category}" });
      s.data.setAll([{ name: "Portfolio", children: SECTORS.map(d => ({ name: d.sector, value: d.h2 })) }]);
      s.set("selectedDataItem", s.dataItems[0]);
      legend([]);
    } },
    sankey: { label: "Sankey", note: "Revenue flow from sector to region", draw(root) {
      const s = root.container.children.push(am5flow.Sankey.new(root, { sourceIdField: "from", targetIdField: "to", valueField: "value", nodeWidth: 10, nodePadding: 16, paddingRight: 70 }));
      s.nodes.get("colors").set("colors", SERIES.map(col));
      s.nodes.labels.template.setAll({ fill: col(ink.text), fontSize: 12 });
      s.links.template.setAll({ fillOpacity: 0.35, tooltipText: "{sourceId} → {targetId}: ₱{value}M" });
      s.data.setAll(FLOWS);
      legend([]);
    } },
    heatmap: { label: "Heatmap", note: "Revenue intensity by sector and quarter", draw(root) {
      const chart = xyChart(root, { paddingBottom: 20 });
      const yr = am5xy.AxisRendererY.new(root, { minGridDistance: 20, inversed: true }); styleAxis(yr, false);
      const y = chart.yAxes.push(am5xy.CategoryAxis.new(root, { categoryField: "sector", renderer: yr }));
      const xr = am5xy.AxisRendererX.new(root, { minGridDistance: 30, opposite: true }); styleAxis(xr, false);
      const x = chart.xAxes.push(am5xy.CategoryAxis.new(root, { categoryField: "quarter", renderer: xr }));
      const s = chart.series.push(am5xy.ColumnSeries.new(root, { xAxis: x, yAxis: y, categoryXField: "quarter", categoryYField: "sector", valueField: "value", calculateAggregates: true, stroke: col(ink.surface) }));
      s.columns.template.setAll({ tooltipText: "{categoryY} · {categoryX}: ₱{value}M", strokeWidth: 2, width: am5.percent(100), height: am5.percent(100), cornerRadiusTL: 4, cornerRadiusTR: 4, cornerRadiusBL: 4, cornerRadiusBR: 4 });
      s.set("heatRules", [{ target: s.columns.template, min: col("#D4E0ED"), max: col(SERIES[2]), dataField: "value", key: "fill" }]);
      y.data.setAll(SECTORS.slice(0, 5)); x.data.setAll(["Q1", "Q2", "Q3", "Q4"].map(q => ({ quarter: q })));
      s.data.setAll(HEAT);
      legend([{ label: "Low", color: "#D4E0ED" }, { label: "High", color: SERIES[2] }]);
    } },
    map: { label: "Map", note: "Revenue by province, Philippines", height: 440, draw(root) {
      const chart = root.container.children.push(am5map.MapChart.new(root, { panX: "none", panY: "none", wheelX: "none", wheelY: "none", projection: am5map.geoMercator(), paddingBottom: 20 }));
      const poly = chart.series.push(am5map.MapPolygonSeries.new(root, { geoJSON: am5geodata_philippinesLow, valueField: "value", calculateAggregates: true }));
      poly.mapPolygons.template.setAll({ tooltipText: "{name}: ₱{value}M", stroke: col(ink.surface), strokeWidth: 1, fill: col(ink.grid), interactive: true });
      poly.set("heatRules", [{ target: poly.mapPolygons.template, min: col("#D4E0ED"), max: col(SERIES[0]), dataField: "value", key: "fill" }]);
      poly.data.setAll(Object.keys(REGIONS).map(id => ({ id, value: REGIONS[id] })));
      legend([{ label: "Low", color: "#D4E0ED" }, { label: "High", color: SERIES[0] }, { label: "No data", color: ink.grid }]);
    } }
  };

  let root = null;
  function show(type) {
    STATE.type = type;
    if (root) { root.dispose(); root = null; }
    $("v917-chart").innerHTML = "";
    $("v917-chart").style.height = (TYPES[type].height || 300) + "px";
    $("v917-note").textContent = TYPES[type].note;
    $("v917-chart").setAttribute("aria-label", TYPES[type].note);
    document.querySelectorAll("#v917-types button").forEach(b => b.setAttribute("aria-pressed", String(b.dataset.type === type)));
    if (typeof am5 === "undefined") { $("v917-note").textContent = "Chart library did not load."; return; }
    try {
      root = am5.Root.new("v917-chart");
      root.setThemes([am5themes_Responsive.new(root)]);
      TYPES[type].draw(root);
    } catch (e) { $("v917-note").textContent = "Could not draw " + TYPES[type].label + ": " + e.message; }
  }
  const bar = $("v917-types");
  Object.keys(TYPES).forEach(k => {
    const b = document.createElement("button"); b.type = "button"; b.dataset.type = k; b.textContent = TYPES[k].label;
    b.addEventListener("click", () => show(k)); bar.appendChild(b);
  });
  show(STATE.type);
})();
</script>
```

## 6. The same recipes in artifacts

Identical module list and `draw` functions. Differences: colors come from the brand tokens (`--blue`, `--muted`, `--border`, dark-mode variants from [generative-ui-artifact.md](generative-ui-artifact.md#4-brand-dark-mode-tokens-artifacts-only)), the Animated theme is allowed, labels use Montserrat, and the chart sits in a brand card with a title and an insight sentence. Multiple charts on one page: one `am5.Root` per container, never shared.

## Checklist

- [ ] `index.js` first, then only the modules the chart types need, pinned to 5.21.5 on cdnjs
- [ ] Type chosen from the table in section 2; ≤ 8 series, ≤ 6 slices, value axes from zero
- [ ] `SERIES` order kept; axis ink and surface gaps from `ink` (host tokens in widgets, brand tokens in artifacts)
- [ ] Legend as HTML with values; chart container has `role="img"` and an `aria-label` that names the current type
- [ ] `paddingBottom: 20` for the watermark; previous root disposed before redrawing
- [ ] Map keyed by `PH-…` province ids, container ≥ 440px; narrow category labels rotated
- [ ] Fallback text or bars when a global is missing

## Related files

- [interactive-chat-widget.md](interactive-chat-widget.md), [generative-ui-artifact.md](generative-ui-artifact.md), [../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md), [../brand/color-palette.md](../brand/color-palette.md)
