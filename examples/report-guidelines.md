# Report Guidelines

For analytical reports that are read top-to-bottom and often exported to PDF or
printed: monthly performance reports, board packs, venture reviews. **Proposed**
pattern built on verified brand tokens and amCharts.

## Structure

1. **Cover** — full-bleed `navy #142741` (or the brand gradient `#65DDF5 → #3A6DBA` with a navy 30% overlay for text contrast): 917Ventures logo in a white rounded container (top-left), report title in Unbounded 700 32–40px white, subtitle (period, e.g. "October 2026") Montserrat 500 16px white at 80%, prepared-by / date / confidentiality line 12px at the bottom.
2. **Summary** — one paragraph of plain-language findings (max 120 words) followed by a KPI row (3–5 marketing-style stat cards are acceptable here: gradient numerals, uppercase labels).
3. **Sections** — one per topic, each: `h2` (Montserrat 700 20px `blue-2`, with a 2px `blue` underline bar 48px wide beneath), 1–2 sentences of insight, a chart or table, a caption. Insight before evidence.
4. **Appendix** — data tables, definitions, methodology, sources.
5. **Back page / footer** — contact (hello@917ventures.com), address, page numbers.

## Page and layout

- Screen version: single column, 960px max width, white on `light-blue` canvas, 48px between sections.
- Print/PDF: A4 portrait (Letter for US audiences), 20mm margins, running header (report title, 10px `grey`) and footer (917Ventures · Confidential · Page n of N). Use `@media print` with `break-before: page` on each `h2` section and `break-inside: avoid` on cards, charts and tables.
- Landscape pages only for wide tables (> 7 columns).
- Typography: body 12pt print / 16px screen Montserrat; captions 10pt / 12px `grey`; numbers tabular.

## Charts in reports

- Follow [../design-system/charts-and-visualizations.md](../design-system/charts-and-visualizations.md). Report-specific:
  - Data labels on for columns/bars (values are read, not hovered).
  - Legends below the chart; direct labels at line ends for ≤ 4 lines.
  - Height 300–360px on screen; 90–110mm in print at 2× pixel ratio.
  - No interactivity assumptions: anything shown on hover must also be visible (labels, annotations).
  - Every chart has a numbered caption: "Figure 3. Monthly revenue by venture, ₱ millions, Jan–Oct 2026. Source: Finance ledger."
  - Disable animations for export (`root.setThemes` without Animated when `print` or `export` mode).
- Export chart PNGs with the amCharts exporting plugin at `minWidth: 1600`, white background, and embed them in the PDF; keep CSV appendices for the underlying data.

## Tables in reports

- Compact style: 12px text, 8px × 12px cell padding, header `bg-light-blue`, horizontal rules `#D0D5DD`, zebra striping allowed in print (`#FAFBFF`).
- Totals row in 600 weight with a top border 2px `#D0D5DD`.
- Max 7 columns portrait; wrap long tables with repeated headers (`thead { display: table-header-group }`).

## Callouts

- Insight callout: `bg-[#EEF4FF] border-l-4 border-blue rounded-r-lg p-4`, title 14px 700 `blue-2`, text 14px.
- Risk callout: `bg-error-50 border-l-4 border-error-red`.
- Positive callout: `bg-success-50 border-l-4 border-success-700`.

## Exporting a report

- Page-level "Export" menu: PDF (print stylesheet or server-side render), XLSX/CSV (all tables), PNG per chart.
- File naming: `917V_<Report-Name>_<YYYY-MM>.pdf`.
- Set `document.title` to the report title for the PDF metadata; include the generation timestamp (Asia/Manila) on the cover.
- Confidentiality label on every page when the data is internal.

## Example section markup

```html
<section class="report-section">
  <h2 class="text-xl font-bold text-blue-2">Revenue performance</h2>
  <div class="h-0.5 w-12 bg-blue mt-2 mb-4" aria-hidden="true"></div>
  <p class="text-base text-black max-w-prose">Revenue grew 12.4% month on month, led by GCash merchant fees; two ventures missed target.</p>
  <figure class="mt-6 bg-white rounded-2xl border border-[#D0D5DD]/60 p-6">
    <div id="chart-revenue" class="h-[340px] w-full"></div>
    <figcaption class="mt-3 text-xs text-grey">Figure 1. Monthly revenue, ₱ millions, Jan–Oct 2026. Source: Finance ledger, extracted 22 Oct 2026.</figcaption>
  </figure>
</section>
```

## Checklist

- [ ] Cover with logo, title, period, author, confidentiality
- [ ] Summary paragraph and KPI row before any chart
- [ ] Insight sentence above every chart/table; numbered captions with units and source
- [ ] Brand theme on all charts; labels visible without hover; animations off for export
- [ ] Print stylesheet: page breaks, repeated table headers, margins, page numbers
- [ ] Contrast checked on cover text; table alternatives for charts
- [ ] Export menu: PDF + data files; correct file naming

## Related files

- [dashboard-guidelines.md](dashboard-guidelines.md), [../design-system/layout-and-spacing.md](../design-system/layout-and-spacing.md#report-and-document-layout-proposed), [../brand/logo-and-assets.md](../brand/logo-and-assets.md)
