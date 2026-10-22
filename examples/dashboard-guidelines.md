# Dashboard Guidelines

How to assemble a 917 Ventures dashboard or admin portal from the skill's
components. The website has no dashboards, so this is a **Proposed** pattern
that reuses verified tokens and the verified stat-card anatomy.

## Page blueprint

```
┌ Top bar (64px, white, shadow-md): logo · product name · search · bell · avatar ┐
├ Sidebar 256px ┬───────────────────────────────────────────────────────────────┤
│ Overview      │ Breadcrumb: Dashboards › Portfolio performance                │
│ Portfolio  ●  │ h1 "Portfolio performance" (Unbounded 24)   [Export ▾] [+ New] │
│ Ventures      │ Filter bar: [Date range ▾] [Venture ▾] [Status ▾]  Apply Reset │
│ Reports       │ KPI row: 4 metric cards (grid-cols-2 lg:grid-cols-4 gap-4)     │
│ Settings      │ Charts: 2 cards per row (lg:grid-cols-2 gap-6), 360px tall     │
│               │ Table card: "Ventures" with toolbar, 25 rows, pagination       │
└───────────────┴───────────────────────────────────────────────────────────────┘
Canvas: bg-light-blue #FAFBFF · padding 32px (16px mobile) · max-width 1440px centered
```

Order content by decision priority: KPIs → trends → breakdowns → detail table. Keep a dashboard to one scroll of ~3 sections; move the rest to tabs or sub-pages.

## Filters

- Place a single filter bar under the page header; it applies to every widget on the page unless a widget says otherwise in its subtitle ("All time").
- Controls: `sm` size (36px), left to right by impact: date range (default "Last 30 days", presets 7d / 30d / 90d / YTD / Custom), primary dimension (venture, region), status. At most 5 visible; the rest behind a "More filters" button.
- Apply explicitly for server-backed data (`Apply` primary `sm`, `Reset` ghost); apply instantly for client-side filtering of small data.
- Show active filters as chips with × when the bar is collapsed (mobile) and keep them in the URL.
- Comparison toggle ("vs previous period") lives in the filter bar and drives the KPI deltas and dashed comparison series.

## Metric (KPI) cards

Anatomy and tokens in [../design-system/components.md](../design-system/components.md#summary-metric-cards-stat-cards). Dashboard rules:

- 3–5 cards; the first is the north-star metric.
- Value in Unbounded 700 28px `black`; unit via prefix/suffix (₱, %, K); delta with arrow + `success-700`/`error-red`; comparison basis in 12px `grey`.
- Optional 80 × 24 sparkline (amCharts) on the right for trend context.
- Clicking a card scrolls/links to its detail chart; the card then shows a hover `shadow-lg`.

## Charts on a dashboard

- Each chart lives in a white card with header (title 16px 700, unit/period 12px `grey`, actions: export, expand), body with the chart (320–400px), optional footer note ("Source: CRM, updated hourly").
- Default pairing: left = time trend (line/area of the north-star metric), right = breakdown (column or donut by venture/region). Below: a secondary comparison (stacked column) and a table.
- Apply the brand theme and the categorical order from [../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md#applying-the-palette-to-series); the same venture must keep its color across every chart.
- "Expand" opens the chart in a dialog at full width with the data table beneath.

## Tables on a dashboard

- One table card at the bottom with the entity list; follow [../design-system/tables-and-data-display.md](../design-system/tables-and-data-display.md). Row click opens a detail page or side drawer (480px wide on desktop, full-screen on mobile).

## States

Dashboards must implement every row of the state matrix in [../design-system/interaction-states.md](../design-system/interaction-states.md#loading-empty-and-error-proposed-state-matrix-for-data-views). First paint shows skeletons for KPI values, chart areas and five table rows; widgets load independently so one failing query does not blank the page.

## Admin portal specifics

- CRUD pages: list (table card) → create/edit (form in a dialog for ≤ 6 fields, otherwise a page with the form at max 640px) → detail (key–value list + related tables in tabs).
- Destructive actions confirm in a dialog; bulk actions show a count ("Delete 3 ventures?").
- Permissions: hide what the user cannot do; disable (with a tooltip explaining why) what they can see but not act on.
- Audit info ("Last edited by … on 22 Oct 2026") in 12px `grey` at the bottom of detail pages.

## Example: Tailwind + React skeleton

```tsx
export function DashboardPage() {
  return (
    <main className="bg-light-blue min-h-screen">
      <div className="mx-auto max-w-[1440px] px-4 md:px-6 xl:px-8 py-6 md:py-8 space-y-6">
        <header className="flex flex-col gap-2 md:flex-row md:items-end md:justify-between">
          <div>
            <nav aria-label="Breadcrumb" className="text-xs text-grey">Dashboards › Portfolio performance</nav>
            <h1 className="font-primary text-2xl font-semibold text-black mt-1">Portfolio performance</h1>
          </div>
          <div className="flex gap-3">
            <button className="bg-white text-blue border border-blue px-6 py-2.5 font-bold rounded-lg hover:opacity-70 transition ease-linear delay-50">Export</button>
            <button className="bg-blue text-white px-6 py-2.5 font-bold rounded-lg hover:opacity-70 transition ease-linear delay-50">New venture</button>
          </div>
        </header>
        <FilterBar />
        <section aria-label="Key metrics" className="grid grid-cols-2 lg:grid-cols-4 gap-4">
          <MetricCard label="Revenue" value="₱1.28M" delta={+12.4} basis="vs last 30 days" />
          {/* … */}
        </section>
        <section className="grid grid-cols-1 lg:grid-cols-2 gap-6">
          <ChartCard title="Revenue trend" unit="₱, monthly"><RevenueLineChart /></ChartCard>
          <ChartCard title="Revenue by venture" unit="₱, last 30 days"><VentureColumnChart /></ChartCard>
        </section>
        <VenturesTable />
      </div>
    </main>
  );
}
```

## Checklist

- [ ] Shell: white top bar with logo, sidebar with Heroicons outline/solid states
- [ ] Canvas `#FAFBFF`, white cards 15px radius, 24px padding
- [ ] KPI row first; Unbounded numerals; deltas in semantic tokens
- [ ] amCharts with brand theme; consistent entity colors; export per card or page
- [ ] Filter bar with presets, URL state, chips on mobile
- [ ] Every widget has loading, empty, error states
- [ ] Keyboard and screen-reader pass; contrast of all new pairings checked

## Related files

- [report-guidelines.md](report-guidelines.md), [../design-system/layout-and-spacing.md](../design-system/layout-and-spacing.md#application-shell-proposed), [../design-system/responsive-design.md](../design-system/responsive-design.md)
