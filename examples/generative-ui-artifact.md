# Generative UI Artifacts

How to turn a request such as "visualize this report", "chart these numbers",
"build me a dashboard for this data" or "mock up a page" into a single,
self-contained HTML artifact in 917 Ventures branding. This is the file to
follow when the skill runs inside Claude Desktop / claude.ai, Cowork, or any
context where the output is a rendered page rather than code in a repository.

Everything here is **Proposed** (the website has no artifacts); the tokens it
uses are the verified ones from [../design-system/design-tokens.md](../design-system/design-tokens.md).

## 1. Decide what to render

First decide the surface: if the user wants to interact with or control the visual from the chat and one view fits ~680px, render an in-chat widget instead ([interactive-chat-widget.md](interactive-chat-widget.md#1-widget-or-artifact)). Otherwise build an artifact from the table below.

| Request shape | Render |
|---|---|
| A few headline numbers, status, totals | Metric cards (no chart) |
| A list of records, exact values, many columns | Table card with sorting |
| Trend, comparison, composition, distribution | amCharts chart card, picked with the table in [../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md#when-to-use-a-chart) |
| "Report" / "summary" of a document or dataset | Report page: cover band → summary paragraph → metric row → 2–4 chart/table sections with insight sentences → source footer ([report-guidelines.md](report-guidelines.md)) |
| "Dashboard" / "overview" / "scorecard" | Dashboard page: page header → metric row → charts in a 2-column grid → table ([dashboard-guidelines.md](dashboard-guidelines.md)) |
| "Mockup", "prototype", "landing page", "screen" | Static UI built from [../design-system/components.md](../design-system/components.md) with realistic content; interactive only where the request implies it |
| Slides, PDFs, Word/Excel files | Not an HTML artifact; use the matching document tool and apply the brand tokens there |

One page per request. If the request has several audiences or questions, use tabs or sections rather than several artifacts.

## 2. Get the data right

1. Use the data the user gave you: pasted text, uploaded CSV/XLSX/PDF, connector results, or earlier turns. Parse it into a JSON array and inline it in the page as `const DATA = [...]`.
2. Never invent figures and present them as real. If no data was provided and the user wants to see the design, generate obviously sample data and mark the page with a "Sample data" badge (`bg-warning-50 text-warning-700`) and a footer line "Illustrative data, not actual figures".
3. Keep units and periods explicit (₱, %, "Jan–Oct 2026"). Aggregate before charting: ≤ 5,000 points, ≤ 8 series, ≤ 6 donut slices.
4. State the source and generation date in the footer: "Source: <file or system> · Generated 29 Oct 2026".

## 3. Artifact runtime constraints

The artifact viewer is a locked-down frame. Build for these limits; they apply to every viewer.

- **One HTML file.** Only the first `<title>` and your `<style>`/markup; the host wraps it in a document skeleton. Do not write `<!doctype>`, `<html>`, `<head>` or `<body>` tags when publishing through Claude's artifact tools (keep them only for a plain `.html` file saved to disk).
- **Scripts** only from `cdn.jsdelivr.net/npm/`, `cdnjs.cloudflare.com`, `unpkg.com`, `cdn.tailwindcss.com`, `code.jquery.com`, pinned to an exact version. **Stylesheets** only from `fonts.googleapis.com`. **Images** must be `data:` URIs (logo in [../assets/logo-data-uris.md](../assets/logo-data-uris.md)). No fetch/XHR to other hosts, no iframes.
- **amCharts 5** loads from **cdnjs** as classic scripts that define globals (`am5`, `am5xy`, `am5percent`, `am5themes_Animated`, `am5themes_Responsive`): `https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/index.js`, then `xy.js`, `percent.js`, `themes/Animated.js`, `themes/Responsive.js` under the same path (also available there: `radar.js`, `hierarchy.js`, `flow.js`, `map.js` with `geodata/philippinesLow.js`, `stock.js`, `timeline.js`, `venn.js`, `wc.js`, `themes/Dark.js`, `plugins/exporting.js`, `locales/en.js`; per-type recipes for donut, funnel, gauge, treemap, sankey, heatmap, map, scatter and radar in [amcharts-recipes.md](amcharts-recipes.md)). Load `index.js` first, the modules next, your inline script last. Verified to render on 2026-10-29. Do **not** use jsDelivr's `+esm` bundles for amCharts: each module bundles its own copy of the core, so colors and percent sizes passed between modules are not recognized and charts render wrong. Pin the version; check `typeof am5` and fall back to a table if the scripts did not load. Without a license key the free build draws a small amCharts logo at the bottom-left of every chart: keep `paddingBottom: 24` on charts so it does not cover axis labels or the legend, and call `am5.addLicense("…")` before `Root.new` when the organization has a key (never paste a key into a shared artifact).
- **No downloads, printing or dialogs:** `<a download>`, the amCharts exporting menu, `window.print()`, `alert()`/`confirm()` do nothing. Do not add Export/Print buttons; offer to produce a file separately. Copy-to-clipboard inside a click handler works.
- **Theme:** the viewer may be light or dark. Define every color as a token on `:root`, redefine under `@media (prefers-color-scheme: dark)` guarded by `:root:not([data-theme="light"])` and again under `:root[data-theme="dark"]`, and give `body` an explicit token background. Read tokens at runtime for chart colors so charts follow the theme.
- **Responsive:** must work at ~400px wide, no horizontal body scroll, 16px side gutter via `max(16px, env(safe-area-inset-left, 0px))`, `min-width: 0` on grid/flex children holding text or tables, tables inside `overflow-x: auto`.
- **Complete at rest:** everything visible on load; no scroll-triggered reveals; charts animate once (500ms) and not under `prefers-reduced-motion`.
- **Title:** a two-to-four-word name ("Q3 Portfolio Review", "Venture KPI Dashboard"), never "Chart" or "Dashboard: …"; explanation goes in the artifact description.
- **Fonts:** Google Fonts link for Unbounded + Montserrat with real fallbacks.

## 4. Brand dark-mode tokens (artifacts only)

The brand is light-only. For the viewer's dark theme use these derived values (Proposed): bg `#0B1220`, card `#142741` (verified navy), text `#F5F7FB`, muted `#A4A6BA`, border `#2A3B57`, accent `#65DDF5` (blue-1 on navy = 9.5:1), accent-strong `#8CC8F5`, canvas band `#0F1B33`. Keep status colors as text-700 on a 12% tint of themselves.

## 5. Starter page

Copy this file, replace `DATA`, the copy and the sections. It contains the token block, logo header, metric cards, an amCharts chart card with brand theme and table fallback, a table card and the footer. Tailwind is not required; the CSS is self-contained. Rendered and checked on 2026-10-29 (Chromium, 1440px and 400px).

```html
<title>Venture KPI Dashboard</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Unbounded:wght@500;600;700&family=Montserrat:wght@400;500;600;700&display=swap">
<style>
  /* Layout: canvas with a white header band, metric row, 2-col chart grid, table. Tokens = 917 Ventures design-tokens.md */
  :root {
    --bg:#FAFBFF; --card:#FFFFFF; --fg:#040815; --fg-2:#323643; --muted:#606471; --border:#D0D5DD;
    --accent:#407DC3; --accent-text:#3A6DBA; --accent-soft:#EEF4FF; --navy:#142741;
    --success:#1E7A3C; --success-bg:#E8F8EE; --warning:#B45309; --warning-bg:#FFF4E0; --error:#D92D20; --error-bg:#FFF5F4;
    --font-display:"Unbounded","Arial Black",Arial,sans-serif; --font-body:"Montserrat","Segoe UI",Roboto,Arial,sans-serif;
    --radius:15px; --radius-sm:8px; --shadow:0 1px 2px rgba(0,0,0,.05);
    --chart-1:#407DC3; --chart-2:#DB6E4F; --chart-3:#3A6DBA; --chart-4:#52BCD5; --chart-5:#DA4E8F; --chart-6:#8EA1BD; --chart-7:#142741; --chart-8:#B45309;
    --chart-grid:#D0D5DD; --chart-text:#606471; --tooltip-bg:#142741; --tooltip-fg:#FFFFFF;
  }
  @media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) {
    --bg:#0B1220; --card:#142741; --fg:#F5F7FB; --fg-2:#DDE3EE; --muted:#A4A6BA; --border:#2A3B57;
    --accent:#65DDF5; --accent-text:#8CC8F5; --accent-soft:#1B2E4D; --success:#6FDB8F; --success-bg:#143D22; --warning:#F5B553; --warning-bg:#3D2A0A; --error:#FF8A80; --error-bg:#4A1A17;
    --chart-7:#A4A6BA; --chart-grid:#2A3B57; --chart-text:#A4A6BA; --tooltip-bg:#F5F7FB; --tooltip-fg:#040815; color-scheme:dark } }
  :root[data-theme="dark"] {
    --bg:#0B1220; --card:#142741; --fg:#F5F7FB; --fg-2:#DDE3EE; --muted:#A4A6BA; --border:#2A3B57;
    --accent:#65DDF5; --accent-text:#8CC8F5; --accent-soft:#1B2E4D; --success:#6FDB8F; --success-bg:#143D22; --warning:#F5B553; --warning-bg:#3D2A0A; --error:#FF8A80; --error-bg:#4A1A17;
    --chart-7:#A4A6BA; --chart-grid:#2A3B57; --chart-text:#A4A6BA; --tooltip-bg:#F5F7FB; --tooltip-fg:#040815; color-scheme:dark }

  body { background:var(--bg); color:var(--fg); font-family:var(--font-body); font-size:14px; line-height:1.5; }
  .wrap { max-width:1200px; margin:0 auto; padding-block:0 32px; padding-left:max(16px, env(safe-area-inset-left, 0px)); padding-right:max(16px, env(safe-area-inset-right, 0px)); }
  header.bar { background:var(--card); box-shadow:0 4px 6px -1px rgba(0,0,0,.1); margin:0 calc(-1 * max(16px, env(safe-area-inset-left, 0px))) 24px; padding:12px max(16px, env(safe-area-inset-left, 0px)); display:flex; align-items:center; gap:16px; flex-wrap:wrap; }
  header.bar .logo { width:160px; height:auto; background:#fff; border-radius:8px; padding:4px 8px; }
  header.bar h1 { font-family:var(--font-display); font-size:20px; font-weight:600; margin:0; color:var(--fg); text-wrap:balance; }
  header.bar .period { color:var(--muted); font-size:12px; margin-left:auto; }
  h2 { font-size:16px; font-weight:700; margin:0; }
  .sub { font-size:12px; color:var(--muted); }
  .kpis { display:grid; grid-template-columns:repeat(auto-fit, minmax(150px, 1fr)); gap:16px; margin-bottom:24px; }
  .card { background:var(--card); border:1px solid var(--border); border-radius:var(--radius); box-shadow:var(--shadow); padding:20px; min-width:0; }
  .kpi .label { font-size:12px; font-weight:500; color:var(--muted); }
  .kpi .value { font-family:var(--font-display); font-size:28px; font-weight:700; line-height:1.2; margin:6px 0 4px; font-variant-numeric:tabular-nums; }
  .kpi .delta { font-size:12px; font-weight:600; } .up { color:var(--success); } .down { color:var(--error); }
  .grid { display:grid; grid-template-columns:repeat(auto-fit, minmax(320px, 1fr)); gap:24px; margin-bottom:24px; }
  .card-head { display:flex; justify-content:space-between; align-items:baseline; gap:12px; margin-bottom:12px; flex-wrap:wrap; }
  .chart { width:100%; height:320px; } @media (max-width:640px) { .chart { height:260px; } }
  .badge { display:inline-flex; align-items:center; border-radius:999px; padding:2px 10px; font-size:12px; font-weight:600; }
  .badge.ok { background:var(--success-bg); color:var(--success); } .badge.warn { background:var(--warning-bg); color:var(--warning); } .badge.bad { background:var(--error-bg); color:var(--error); }
  .table-wrap { overflow-x:auto; } table { width:100%; border-collapse:collapse; font-size:14px; }
  th { text-align:left; font-size:12px; font-weight:600; color:var(--muted); background:var(--accent-soft); padding:10px 12px; border-bottom:1px solid var(--border); }
  td { padding:10px 12px; border-bottom:1px solid var(--border); } td.num, th.num { text-align:right; font-variant-numeric:tabular-nums; }
  tr:hover td { background:var(--accent-soft); }
  footer { font-size:12px; color:var(--muted); display:flex; justify-content:space-between; gap:12px; flex-wrap:wrap; }
  .sample { background:var(--warning-bg); color:var(--warning); }
  @media (prefers-reduced-motion: reduce) { * { animation:none !important; transition:none !important; } }
</style>

<div class="wrap">
  <header class="bar">
    <img class="logo" alt="917Ventures" width="160" height="31" src="data:image/png;base64,PASTE_FROM_assets/logo-data-uris.md">
    <h1>Venture KPI Dashboard</h1>
    <span class="period">Jan–Oct 2026 · ₱ millions</span>
  </header>

  <section class="kpis" aria-label="Key metrics">
    <div class="card kpi"><div class="label">Revenue</div><div class="value">₱1,284</div><div class="delta up">▲ 12.4% vs prior period</div></div>
    <div class="card kpi"><div class="label">Active ventures</div><div class="value">12</div><div class="delta">— unchanged</div></div>
    <div class="card kpi"><div class="label">Ideas vetted</div><div class="value">46</div><div class="delta up">▲ 8 this quarter</div></div>
    <div class="card kpi"><div class="label">Burn vs plan</div><div class="value">96%</div><div class="delta down">▼ 4 pts under plan</div></div>
  </section>

  <section class="grid">
    <div class="card">
      <div class="card-head"><div><h2>Monthly revenue</h2><div class="sub">₱ millions, by month</div></div></div>
      <div id="chart-revenue" class="chart" role="img" aria-label="Column chart of monthly revenue"></div>
      <div id="chart-revenue-fallback" hidden></div>
    </div>
    <div class="card">
      <div class="card-head"><div><h2>Revenue by venture</h2><div class="sub">share of total, Jan–Oct</div></div></div>
      <div id="chart-share" class="chart" role="img" aria-label="Donut chart of revenue share by venture"></div>
      <div id="chart-share-fallback" hidden></div>
    </div>
  </section>

  <section class="card" style="margin-bottom:24px">
    <div class="card-head"><h2>Ventures</h2><span class="sub">5 of 12 shown</span></div>
    <div class="table-wrap"><table>
      <thead><tr><th>Venture</th><th>Status</th><th class="num">Revenue</th><th class="num">Growth</th></tr></thead>
      <tbody id="rows"></tbody>
    </table></div>
  </section>

  <footer><span>Source: Finance ledger extract · Generated 29 Oct 2026</span><span class="badge sample">Sample data</span></footer>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/index.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/xy.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/percent.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/themes/Animated.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/themes/Responsive.js"></script>
<script>
(function () {
  const DATA = {
    monthly: [{m:"Jan",v:98},{m:"Feb",v:104},{m:"Mar",v:111},{m:"Apr",v:120},{m:"May",v:126},{m:"Jun",v:131},{m:"Jul",v:140},{m:"Aug",v:147},{m:"Sep",v:151},{m:"Oct",v:156}],
    share: [{n:"GCash",v:640},{n:"KonsultaMD",v:210},{n:"RUSH",v:180},{n:"AdSpark",v:150},{n:"Other",v:104}],
    rows: [["GCash","ok",640,14.2],["KonsultaMD","ok",210,9.8],["RUSH","warn",180,2.1],["AdSpark","ok",150,6.4],["Inquiro","bad",60,-3.5]],
  };
  const css = v => getComputedStyle(document.documentElement).getPropertyValue(v).trim();
  const fmt = n => new Intl.NumberFormat("en-PH").format(n);

  // Table rows (always rendered — this is also the no-JS-library fallback for data)
  document.getElementById("rows").innerHTML = DATA.rows.map(([n,s,r,g]) =>
    `<tr><td><strong>${n}</strong></td><td><span class="badge ${s}">${{ok:"On track",warn:"Watch",bad:"Below plan"}[s]}</span></td><td class="num">₱${fmt(r)}</td><td class="num ${g<0?"down":"up"}">${g<0?"▼":"▲"} ${Math.abs(g).toFixed(1)}%</td></tr>`).join("");

  function showFallbacks() {
    fallbackTable("chart-revenue", DATA.monthly.map(d=>[d.m,d.v]), ["Month","Revenue (₱M)"]);
    fallbackTable("chart-share", DATA.share.map(d=>[d.n,d.v]), ["Venture","Revenue (₱M)"]);
  }
  function fallbackTable(id, rows, cols) {
    document.getElementById(id).hidden = true;
    const el = document.getElementById(id + "-fallback"); el.hidden = false;
    el.innerHTML = `<div class="table-wrap"><table><thead><tr>${cols.map(c=>`<th>${c}</th>`).join("")}</tr></thead><tbody>${rows.map(r=>`<tr>${r.map((c,i)=>`<td class="${i?"num":""}">${typeof c==="number"?fmt(c):c}</td>`).join("")}</tr>`).join("")}</tbody></table></div>`;
  }

  if (typeof am5 === "undefined" || typeof am5xy === "undefined" || typeof am5percent === "undefined") { showFallbacks(); return; }
  try {
    const reduced = matchMedia("(prefers-reduced-motion: reduce)").matches;

    function brandTheme(root) {
      const t = am5.Theme.new(root);
      t.rule("ColorSet").setAll({ colors: [1,2,3,4,5,6,7,8].map(i => am5.color(css(`--chart-${i}`))), reuse: false });
      t.rule("Label").setAll({ fontFamily: css("--font-body"), fontSize: 12, fill: am5.color(css("--chart-text")) });
      t.rule("Grid").setAll({ stroke: am5.color(css("--chart-grid")), strokeOpacity: 0.6 });
      t.rule("AxisRenderer").setAll({ stroke: am5.color(css("--chart-grid")) });
      t.rule("AxisTick").setAll({ visible: false });
      t.rule("RoundedRectangle", ["series","column"]).setAll({ cornerRadiusTL: 4, cornerRadiusTR: 4, strokeOpacity: 0 });
      return t;
    }
    function makeRoot(id) {
      const root = am5.Root.new(id);
      root.setThemes([...(reduced ? [] : [am5themes_Animated.new(root)]), am5themes_Responsive.new(root), brandTheme(root)]);
      root.numberFormatter.set("numberFormat", "#,###.#");
      root.dateFormatter.set("dateFormat", "dd MMM yyyy");
      return root;
    }
    function brandTooltip(root) {
      const tip = am5.Tooltip.new(root, { getFillFromSprite: false });
      tip.get("background").setAll({ fill: am5.color(css("--tooltip-bg")), stroke: am5.color(css("--tooltip-bg")), cornerRadius: 8 });
      tip.label.setAll({ fill: am5.color(css("--tooltip-fg")) });
      return tip;
    }

    // Column chart
    {
      const root = makeRoot("chart-revenue");
      const chart = root.container.children.push(am5xy.XYChart.new(root, { panX:false, panY:false, wheelX:"none", wheelY:"none", paddingLeft:0, paddingBottom:24 }));
      const x = chart.xAxes.push(am5xy.CategoryAxis.new(root, { categoryField:"m", renderer: am5xy.AxisRendererX.new(root, { minGridDistance: 30 }) }));
      const y = chart.yAxes.push(am5xy.ValueAxis.new(root, { min:0, renderer: am5xy.AxisRendererY.new(root, {}) }));
      const s = chart.series.push(am5xy.ColumnSeries.new(root, { name:"Revenue", xAxis:x, yAxis:y, valueYField:"v", categoryXField:"m", tooltip: brandTooltip(root) }));
      s.get("tooltip").set("labelText", "{categoryX}: ₱{valueY}M");
      s.columns.template.setAll({ width: am5.percent(65), ariaLabel: "{categoryX}: {valueY}" , focusable: true });
      x.data.setAll(DATA.monthly); s.data.setAll(DATA.monthly);
      chart.set("cursor", am5xy.XYCursor.new(root, { behavior:"none" })).lineY.set("visible", false);
    }
    // Donut
    {
      const root = makeRoot("chart-share");
      const chart = root.container.children.push(am5percent.PieChart.new(root, { innerRadius: am5.percent(60), layout: root.verticalLayout, paddingBottom:24 }));
      const s = chart.series.push(am5percent.PieSeries.new(root, { valueField:"v", categoryField:"n", tooltip: brandTooltip(root) }));
      s.get("tooltip").set("labelText", "{category}: ₱{value}M ({valuePercentTotal.formatNumber('#.0')}%)");
      s.labels.template.set("forceHidden", true); s.ticks.template.set("forceHidden", true);
      s.slices.template.setAll({ stroke: am5.color(css("--card")), strokeWidth: 2, focusable: true, ariaLabel: "{category}: {value}" });
      s.data.setAll(DATA.share);
      const total = DATA.share.reduce((a,b)=>a+b.v,0);
      chart.seriesContainer.children.push(am5.Label.new(root, { text:`₱${fmt(total)}M`, centerX: am5.percent(50), centerY: am5.percent(50), fontFamily: css("--font-display"), fontSize: 20, fontWeight: "700", fill: am5.color(css("--fg")) }));
      const legend = chart.children.push(am5.Legend.new(root, { centerX: am5.percent(50), x: am5.percent(50), marginTop: 8 }));
      legend.labels.template.setAll({ fill: am5.color(css("--fg-2")), fontSize: 12 }); legend.valueLabels.template.set("forceHidden", true);
      legend.data.setAll(s.dataItems);
    }
  } catch (err) { showFallbacks(); }
})();
</script>
```

## 6. Deliver

1. Publish the page as an artifact (claude.ai / Claude Desktop create it natively; in Claude Code use the Artifact tool; in Cowork save the `.html` to the outputs folder, wrapped in a full `<!doctype html>` document).
2. In the reply, state in one or two lines: what was visualized, the data source and period, and anything marked sample or Proposed. Do not describe the design or list hex codes.
3. If the user wants a file (PDF, XLSX, PPTX), produce it with the matching document tool using the same tokens; the artifact itself cannot download or print.

## Checklist

- [ ] Correct render type (cards / table / chart / report / dashboard / mockup)
- [ ] Real data inlined, units and period shown, sample data labeled
- [ ] Title is a 2–4 word name; logo as data URI; Unbounded + Montserrat loaded with fallbacks
- [ ] Light and dark tokens complete; `body` background set; charts read tokens at runtime
- [ ] amCharts pinned to 5.21.5 via cdnjs script tags (`index.js` first), brand theme applied, table fallback on failure
- [ ] Works at 400px; no horizontal scroll; tables scroll inside their card
- [ ] No Export/Print/download buttons; no external images or fetches
- [ ] Reduced motion respected; everything visible on load

## Related files

- [interactive-chat-widget.md](interactive-chat-widget.md) (in-chat widgets), [amcharts-recipes.md](amcharts-recipes.md) (every chart type), [../assets/logo-data-uris.md](../assets/logo-data-uris.md), [../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md), [dashboard-guidelines.md](dashboard-guidelines.md), [report-guidelines.md](report-guidelines.md), [../design-system/components.md](../design-system/components.md)
