# Interactive Chat Widgets

How to render a 917 Ventures visual **inside the conversation** so the user can
control it from the chat: toggle periods, filter, drill down, and hand actions
back to Claude. Use this file when the request is interactive or exploratory
("show me the numbers", "let me filter this", "chart this so I can poke at it")
or when the user asks for something they want to steer from the chat rather
than a finished page. Full pages, reports and shareable dashboards stay
artifacts ([generative-ui-artifact.md](generative-ui-artifact.md)).

Everything here is **Proposed**: the website has no widgets. Host rules come
from the Claude Desktop / claude.ai inline widget tool (the "visualize" or
"show widget" tool that renders HTML in the chat); the brand tokens are the
verified ones from [../design-system/design-tokens.md](../design-system/design-tokens.md).

## 1. Widget or artifact?

| Signal | Render |
|---|---|
| The user wants to interact or decide from the chat: compare periods, filter a list, pick a segment, ask "what about Q2" | **Widget** |
| One view fits in ~680px: a KPI row plus one or two charts, a comparison grid, a single record | **Widget** |
| A list of real-world things to pick from (things to do, places, venues, products, people), up to ~8 items | **Widget** with media cards and photos ([media-cards.md](media-cards.md)) |
| A full report or dashboard, many sections, a page the user will share or keep, a page mockup, print-like output | **Artifact** |
| A table with many rows or columns | Markdown table in the reply, or an artifact; never inside a widget |
| No inline widget tool in the current surface (Claude Code, Cowork) | Artifact |

Both can be combined: start with a widget for exploration and give it an
"Open as full dashboard ↗" button that asks Claude for the artifact.

## 2. The control loop

A widget is a function of two objects: `DATA` (the rows) and `STATE` (what the
user has chosen). Keep both at the top of the script.

**Chat → widget.** When the user asks for a change in the chat ("switch to
quarterly", "only Velocity", "add cost of sales"), re-render the widget with
`STATE` updated and everything else unchanged, so it reads as the same widget.
Do not answer with prose only when the user asked for a change in the visual.
Preserve earlier choices unless the user changes them.

**Widget → chat.** Handle cheap interactions in JavaScript: toggles, filters,
sorting, computed totals, hover values. Anything that needs Claude goes through
`sendPrompt(text)`, a global the host provides that sends `text` to the chat as
if the user typed it:

- explain or interpret ("Explain the dip ↗")
- drill down into data the widget does not have ("Break down by venture ↗", a click on a column)
- change the data source or period beyond the inlined data
- produce something else: a full dashboard artifact, a file, a comparison

Prompt text rules: one self-contained sentence written the way the user would
say it, naming the subject and the selection ("Break down 917 Ventures revenue
for Q2 2026 by venture"). Buttons that call `sendPrompt` end with ↗. Guard every
call with `typeof sendPrompt === "function"` so the fragment also works where
the host does not provide it.

**On return.** When one of those messages arrives, answer it in the reply and,
if the view should change, re-render the widget with `STATE` carrying the
selection (`selected: "Q2"`, `period: "quarterly"`), same ids and layout.

Prefix every id with `v917-` so several widgets can live in one conversation.

## 3. Widget runtime constraints (host rules)

- **Fragment only.** No `<!doctype>`, `<html>`, `<head>` or `<body>`. Order for streaming: a short `<style>`, then content, then `<script>` tags last. Scripts run after streaming completes, so a library `<script src>` must precede the inline script that uses its global.
- **Flat and transparent.** No gradients, shadows, blur or background colors on the outer container; the host supplies the page background. No `position: fixed`, no nested scrolling, no tabs or `display: none` sections during streaming (hidden content streams invisibly).
- **Host tokens, not brand hex, for surfaces and text.** `--surface-1` (card), `--surface-2` (white), `--text-primary`, `--text-secondary`, `--text-muted`, `--text-accent`, `--bg-accent`, `--border-accent`, `--text-success`, `--text-danger`, `--border`, `--border-strong`, `--radius`. They flip with the host theme (light/dark), so text stays readable; a hard-coded `#040815` would vanish in dark mode. Brand hex is for series colors and the wordmark only.
- **Type.** Host sans by default; headings 22/18/16px at weight 500, body 14–16px, two weights only (400, 500), sentence case, no font below 11px. Load Unbounded from Google Fonts for the wordmark and KPI numerals only (the CSP allows `fonts.googleapis.com`); do not load Montserrat.
- **Icons.** The widget host preloads the Tabler outline webfont (`<i class="ti ti-chart-bar" aria-hidden="true"></i>`). Use it inside widgets; this is the one documented exception to the Heroicons rule in [../brand/icons.md](../brand/icons.md). Never the `-filled` variants, never emoji.
- **Scripts** only from `cdnjs.cloudflare.com` (preferred), `cdn.jsdelivr.net`, `unpkg.com`, `esm.sh`, pinned. amCharts 5 loads exactly as in artifacts: `https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/index.js`, then the modules the chart needs (`xy.js`, `percent.js`, `radar.js`, `hierarchy.js`, `flow.js`, `map.js` + `geodata/philippinesLow.js`), then `themes/Responsive.js`; skip the Animated theme in widgets. Every chart family works in a widget, not only columns and lines: recipes for donut, funnel, gauge, treemap, sankey, heatmap, map, scatter and radar are in [amcharts-recipes.md](amcharts-recipes.md). Keep `paddingBottom: 20` for the free-build watermark. If `am5` is missing, draw HTML bars instead.
- **Width.** The container is 680px wide and full-width on phones; use `repeat(auto-fit, minmax(160px, 1fr))` grids and `minmax(0, 1fr)` columns.
- **Content.** No titles, headings or paragraphs of explanation inside the widget; the reply text carries them. Exception: a media card carries its item's name and one summary sentence ([media-cards.md](media-cards.md)). Round every displayed number (`toFixed`, `toLocaleString`). Validate any input before acting on it and show an inline error in `var(--text-danger)`.
- **Accessibility.** First element is `<h2 class="sr-only">` with a one-sentence summary. Chart container has `role="img"` and an `aria-label`. Toggles use `aria-pressed`; buttons have visible text.
- **Images.** Photos of the thing shown are welcome in widgets: `<img src="https://…" alt="…" loading="lazy" referrerpolicy="no-referrer">` with the URL copied from a search result, the official site or Wikimedia Commons, a source credit on the photo and an `error` handler that swaps in a branded placeholder tile. Never decorative stock photos, never data URIs (too heavy to stream). Pattern and starter in [media-cards.md](media-cards.md).
- **No comments** in the HTML, CSS or JavaScript (they break streaming).
- Links: plain `<a href="https://…">` open through the host's link dialog.

## 4. What carries over from the brand

| Element | Artifact | Widget |
|---|---|---|
| Logo | Data URI from [../assets/logo-data-uris.md](../assets/logo-data-uris.md) | Text wordmark "917 Ventures" in Unbounded 600, `#407DC3`, 13px (the data URI is too heavy to stream) |
| Surfaces, borders, body text | Brand tokens, light + dark | Host tokens (`--surface-1`, `--text-primary`, …) |
| KPI numerals | Unbounded, gradient allowed at display sizes | Unbounded 24px, no gradient |
| Primary series | `#407DC3` | `#407DC3` (reads in both themes) |
| Series order | blue, coral `#DB6E4F`, blue-2, cyan, pink, steel, navy, amber | same order |
| Status colors | success-700 / error-red tokens | `--text-success` / `--text-danger` host tokens, paired with a sign or icon |
| Buttons | Brand button classes | Bare host-styled `<button>`; pressed state `--bg-accent` + `--text-accent` + `--border-accent` |
| Icons | Heroicons v2 | Tabler outline (host) |
| Radius | 8px controls / 15px cards | `var(--radius)` controls / 12px cards |

## 5. Starter widget (verified 2026-10-29)

KPI row, period toggle handled in JavaScript, amCharts column chart, click a
column to drill down through `sendPrompt`, three prompt buttons (the last hands
off to the artifact flow), HTML bar fallback if amCharts does not load. Tested
in Chromium at 1024px and 375px with the host tokens shimmed: the toggle
recomputes the cards and the chart, the buttons and the column click fire
`sendPrompt`, no console errors, no horizontal overflow. Replace `DATA`, the
labels and the prompt sentences; keep the structure.

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Unbounded:wght@600&display=swap">
<style>
.v917 { font-size: 14px; color: var(--text-primary); }
.v917 .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); }
.v917 .display { font-family: Unbounded, "Arial Black", Arial, sans-serif; font-weight: 600; }
.v917 .seg button { padding: 6px 12px; }
.v917 .seg button[aria-pressed="true"] { background: var(--bg-accent); color: var(--text-accent); border-color: var(--border-accent); }
.v917 .kpis { display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 12px; }
.v917 .kpi { background: var(--surface-1); border-radius: var(--radius); padding: 1rem; min-width: 0; }
.v917 .up { color: var(--text-success); } .v917 .down { color: var(--text-danger); }
.v917 .bar { display: grid; grid-template-columns: 56px minmax(0, 1fr) 72px; gap: 8px; align-items: center; font-size: 13px; }
.v917 .bar i { display: block; height: 10px; background: #407DC3; border-radius: 0 4px 4px 0; }
</style>
<div class="v917">
<h2 class="sr-only">Monthly revenue for the 917 Ventures portfolio, January to October 2026, with totals and a period filter.</h2>
<div style="display:flex; flex-wrap:wrap; align-items:center; justify-content:space-between; gap:12px; margin:0 0 1rem;">
  <div style="display:flex; align-items:center; gap:8px;">
    <span class="display" style="font-size:13px; color:#407DC3; letter-spacing:0.02em;">917 Ventures</span>
    <span style="font-size:13px; color:var(--text-secondary);">Portfolio revenue · 2026 · ₱ millions · sample data</span>
  </div>
  <div class="seg" style="display:flex; gap:6px;" role="group" aria-label="Period">
    <button type="button" data-period="monthly" aria-pressed="true">Monthly</button>
    <button type="button" data-period="quarterly" aria-pressed="false">Quarterly</button>
  </div>
</div>
<div class="kpis" style="margin:0 0 1.25rem;">
  <div class="kpi"><div style="font-size:13px; color:var(--text-secondary);">Revenue YTD</div><div class="display" style="font-size:24px; margin-top:4px;" id="v917-kpi-total">—</div><div style="font-size:13px; margin-top:4px;" id="v917-kpi-total-d"></div></div>
  <div class="kpi"><div style="font-size:13px; color:var(--text-secondary);">Best month</div><div class="display" style="font-size:24px; margin-top:4px;" id="v917-kpi-best">—</div><div style="font-size:13px; margin-top:4px; color:var(--text-secondary);" id="v917-kpi-best-d"></div></div>
  <div class="kpi"><div style="font-size:13px; color:var(--text-secondary);">Last vs previous</div><div class="display" style="font-size:24px; margin-top:4px;" id="v917-kpi-mom">—</div><div style="font-size:13px; margin-top:4px; color:var(--text-secondary);" id="v917-kpi-mom-d"></div></div>
</div>
<div id="v917-chart" style="width:100%; height:240px;" role="img" aria-label="Column chart of monthly revenue"></div>
<div id="v917-fallback" style="display:grid; gap:6px;"></div>
<div style="display:flex; flex-wrap:wrap; gap:8px; margin:1.25rem 0 0;">
  <button type="button" onclick="sendPrompt('Explain the revenue dip in the 917 Ventures chart and what drove it')">Explain the dip ↗</button>
  <button type="button" onclick="sendPrompt('Break the 917 Ventures revenue chart down by venture')">Break down by venture ↗</button>
  <button type="button" onclick="sendPrompt('Turn this 917 Ventures revenue view into a full dashboard artifact')">Open as full dashboard ↗</button>
</div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/index.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/xy.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/amcharts5/5.21.5/themes/Responsive.js"></script>
<script>
(function () {
  const STATE = { period: "monthly", unit: "₱M", sample: true };
  const DATA = [
    { m: "Jan", q: "Q1", v: 42.1 }, { m: "Feb", q: "Q1", v: 45.8 }, { m: "Mar", q: "Q1", v: 51.2 },
    { m: "Apr", q: "Q2", v: 48.9 }, { m: "May", q: "Q2", v: 55.4 }, { m: "Jun", q: "Q2", v: 58.0 },
    { m: "Jul", q: "Q3", v: 61.3 }, { m: "Aug", q: "Q3", v: 57.7 }, { m: "Sep", q: "Q3", v: 64.9 },
    { m: "Oct", q: "Q4", v: 68.2 }
  ];
  const BRAND = { blue: "#407DC3", coral: "#DB6E4F" };
  const root = document.documentElement;
  const isDark = root.dataset.mode ? root.dataset.mode === "dark" : matchMedia("(prefers-color-scheme: dark)").matches;
  const ink = { text: isDark ? "#c3c2b7" : "#52514e", grid: isDark ? "#2c2c2a" : "#e1e0d9" };
  const fmt = n => "₱" + Math.round(n * 10) / 10 + "M";
  const $ = id => document.getElementById(id);

  function series() {
    if (STATE.period === "monthly") return DATA.map(d => ({ label: d.m, value: d.v }));
    const q = {};
    DATA.forEach(d => { q[d.q] = q[d.q] || { sum: 0, n: 0 }; q[d.q].sum += d.v; q[d.q].n += 1; });
    return Object.keys(q).map(k => ({ label: q[k].n < 3 ? k + " to date" : k, value: Math.round(q[k].sum * 10) / 10 }));
  }

  function kpis(rows) {
    const total = rows.reduce((s, r) => s + r.value, 0);
    const best = rows.reduce((a, b) => (b.value > a.value ? b : a), rows[0]);
    const last = rows[rows.length - 1], prev = rows[rows.length - 2] || last;
    const pct = prev.value ? ((last.value - prev.value) / prev.value) * 100 : 0;
    $("v917-kpi-total").textContent = fmt(total);
    $("v917-kpi-total-d").textContent = rows.length + " periods · " + STATE.unit;
    $("v917-kpi-best").textContent = fmt(best.value);
    $("v917-kpi-best-d").textContent = best.label;
    const el = $("v917-kpi-mom");
    el.textContent = (pct >= 0 ? "+" : "") + pct.toFixed(1) + "%";
    el.className = "display " + (pct >= 0 ? "up" : "down");
    $("v917-kpi-mom-d").textContent = last.label + " vs " + prev.label;
  }

  function fallback(rows) {
    const max = Math.max.apply(null, rows.map(r => r.value));
    $("v917-fallback").innerHTML = rows.map(r =>
      '<div class="bar"><span style="color:var(--text-secondary)">' + r.label + '</span><i style="width:' + Math.round((r.value / max) * 100) + '%"></i><span style="text-align:right">' + fmt(r.value) + '</span></div>'
    ).join("");
  }

  let chart = null, xAxis = null, colSeries = null;
  function makeChart() {
    const r = am5.Root.new("v917-chart");
    r.setThemes([am5themes_Responsive.new(r)]);
    chart = r.container.children.push(am5xy.XYChart.new(r, { panX: false, panY: false, wheelX: "none", wheelY: "none", paddingLeft: 0, paddingRight: 0, paddingBottom: 20 }));
    const xr = am5xy.AxisRendererX.new(r, { minGridDistance: 28 });
    xr.grid.template.set("visible", false);
    xr.labels.template.setAll({ fill: am5.color(ink.text), fontSize: 12 });
    xAxis = chart.xAxes.push(am5xy.CategoryAxis.new(r, { categoryField: "label", renderer: xr }));
    const yr = am5xy.AxisRendererY.new(r, {});
    yr.grid.template.setAll({ stroke: am5.color(ink.grid), strokeOpacity: 1 });
    yr.labels.template.setAll({ fill: am5.color(ink.text), fontSize: 12 });
    const yAxis = chart.yAxes.push(am5xy.ValueAxis.new(r, { min: 0, numberFormat: "₱#M", renderer: yr }));
    colSeries = chart.series.push(am5xy.ColumnSeries.new(r, { xAxis, yAxis, valueYField: "value", categoryXField: "label", fill: am5.color(BRAND.blue), stroke: am5.color(BRAND.blue) }));
    colSeries.columns.template.setAll({ width: am5.percent(60), maxWidth: 24, cornerRadiusTL: 4, cornerRadiusTR: 4, tooltipText: "{categoryX}: ₱{valueY}M", cursorOverStyle: "pointer" });
    colSeries.columns.template.events.on("click", ev => {
      const c = ev.target.dataItem.get("categoryX");
      if (typeof sendPrompt === "function") sendPrompt("Break down 917 Ventures revenue for " + c + " 2026 by venture");
    });
  }
  function draw(rows) {
    if (!chart) makeChart();
    xAxis.data.setAll(rows);
    colSeries.data.setAll(rows);
  }

  function render() {
    const rows = series();
    kpis(rows);
    if (typeof am5 === "undefined" || typeof am5xy === "undefined") { $("v917-chart").style.height = "0"; fallback(rows); return; }
    try { draw(rows); } catch (e) { $("v917-chart").style.height = "0"; fallback(rows); }
  }

  document.querySelectorAll(".v917 .seg button").forEach(b => b.addEventListener("click", () => {
    STATE.period = b.dataset.period;
    document.querySelectorAll(".v917 .seg button").forEach(x => x.setAttribute("aria-pressed", String(x === b)));
    render();
  }));
  render();
})();
</script>
```

## 6. Deliver

1. Call the inline widget tool with the fragment. Give it a specific snake_case title (`v917_revenue_2026_by_month`).
2. In the reply, one or two lines: what is shown, source and period, and whether the data is sample. Do not list the buttons or describe the design.
3. When a `sendPrompt` message comes back, answer it, then re-render the widget with `STATE` updated if the view should change.
4. When the user asks for the full version, switch to the artifact flow in [generative-ui-artifact.md](generative-ui-artifact.md) and carry `STATE` (period, filters, selection) into it.

## Artifacts that talk back (optional, Proposed)

Where the artifact runtime offers capabilities (Claude Code's Artifact tool
does; the chat on Claude Desktop iterates artifacts from the conversation
instead), a published page can declare `sample` so a button inside the page
asks Claude directly, or `artifact` so the page saves its own state as a new
version. Use them only when the user asks for a page that must keep state or
answer questions on its own; the default remains "control it from the chat".

## Checklist

- [ ] Widget chosen because the user wants to interact from the chat and one view fits 680px; otherwise artifact
- [ ] `DATA` and `STATE` objects at the top of the script; ids prefixed `v917-`
- [ ] Cheap interactions in JS; Claude-needed actions through `sendPrompt` with specific, self-contained sentences; buttons end with ↗; calls guarded
- [ ] Host tokens for surfaces and text; brand blue only for series and the wordmark; Unbounded only for wordmark and numerals
- [ ] Fragment only, style → content → scripts; no comments; no gradients, shadows, fixed positioning or hidden sections
- [ ] amCharts via cdnjs script tags, `index.js` first, only the modules the chart type needs, Responsive theme, HTML fallback, `paddingBottom: 20`; chart type picked from [amcharts-recipes.md](amcharts-recipes.md#2-pick-the-type)
- [ ] `sr-only` summary first; `role="img"` + `aria-label` on the chart; `aria-pressed` on toggles
- [ ] Numbers rounded; partial periods labeled ("Q4 to date"); sample data stated in the header line and the reply
- [ ] Tables and explanations go in the reply, not the widget
- [ ] Discovery lists use media cards: photos with credits, name, one-line summary, two tips, one ↗ action per card

## Related files

- [generative-ui-artifact.md](generative-ui-artifact.md) (full pages), [media-cards.md](media-cards.md) (places, products, people with photos), [amcharts-recipes.md](amcharts-recipes.md) (every chart type), [../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md) (chart choice, series order), [../brand/typography.md](../brand/typography.md), [../brand/icons.md](../brand/icons.md), [dashboard-guidelines.md](dashboard-guidelines.md)
