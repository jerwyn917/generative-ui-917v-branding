# Layout and Spacing

Verified from the Tailwind utilities and markup on 917ventures.com; product
layouts (app shells, dashboards) are **Proposed** extensions.

## Container (Verified)

- `.container` is 100% wide with **32px horizontal padding** (`padding: 2rem`), centered with `mx-auto`.
- Max widths step through 640 / 768 / 1024 / 1280 / **1440** / 1536px. The header uses `2xl:container`, so above 1536px the header content stops growing and centers.
- Section vertical rhythm: `py-16` (64px) for standard sections; hero `pt-[90px] md:pt-[180px] pb-[50px] md:pb-[100px]`; footer `py-16`.
- Stat cards overlap the hero with a negative margin (`-mt-20 sm:-mt-28 md:-mt-[90px]`), a signature layout move.

## Spacing scale (Verified values in use)

4px base. Values present on the site: 4, 8, 12, 16, 20, 24, 32, 40, 48, 56, 64, 80, 96, 112, 160px (`gap-1` … `gap-40`). Common pairs:

| Context | Spacing |
|---|---|
| Form field stack | `gap-6` (24px) between fields; `gap-1` (4px) label → input; `gap-4` (16px) between grid columns |
| Card internal padding | `p-5` (20px) mobile → `p-8` (32px) desktop (news cards); stat cards `py-8 px-12` |
| Card grids | `gap-3`/`gap-4` for tight tile grids; `gap-10` (40px) for news cards |
| Header | `h-24` (96px) tall; mobile menu `px-12 pb-12`, items `gap-8` |
| Footer columns | `gap-y-8`, `gap-x-14` |
| Social icon row | `gap-x-2.5` (10px) |

**Proposed product spacing:** use 8px as the default gap inside components, 16px between related components, 24px between groups, 32–48px between page sections. Card padding is 24px on desktop and 16px on mobile. Table cell padding is 12px vertical × 16px horizontal.

## Grids (Verified)

- `grid-cols-1 md:grid-cols-3` for the stat trio; `grid-cols-1 sm:grid-cols-2 lg:grid-cols-2 xl:grid-cols-3` for news cards; `grid-cols-3` for portfolio tiles even on mobile (tiles shrink); `md:grid-cols-2` inside forms; `grid-cols-12` / `md:grid-cols-12` for page-level split layouts (e.g. contact form `col-span-full lg:col-span-6` beside a map).
- Alignment is **centered** for marketing sections (titles, intro copy, CTAs) and **left-aligned** inside cards and forms.

**Proposed product grid:** 12-column CSS grid inside the content container, 24px gutters on desktop, 16px on mobile. KPI rows use 2 columns on mobile, 4 on desktop. Chart cards span 6 or 12 columns; tables span 12.

## Page structures

### Marketing page (Verified)

```
Sticky white header (96px, shadow-md)
Hero (navy #142741 or image with dark overlay; white Unbounded headline; optional overlapping stat cards)
Sections (white / light-blue alternating, py-16, centered uppercase blue title + intro paragraph + content)
CTA band (cta-gradient, white headline, white button)
Footer (light-blue, logo + blurb + socials + contact + links)
```

### Application shell (Proposed)

```
┌───────────────────────────────────────────────────────────────┐
│ Top bar: white, 64px, shadow-md, logo left (135px), user menu │
├──────────┬────────────────────────────────────────────────────┤
│ Sidebar  │ Page header: h1 (Unbounded 24px), breadcrumbs,     │
│ 256px    │   primary action right                              │
│ white or │ Content canvas: light-blue #FAFBFF, padding 24–32px │
│ navy     │   KPI row → filters → charts/tables in white cards  │
│ collapses│                                                     │
│ to 64px  │                                                     │
└──────────┴────────────────────────────────────────────────────┘
```

- Sidebar: white with `border-r` `#D0D5DD` by default (closest to the site's white header). A navy `#142741` sidebar with white text is an acceptable alternative for admin portals; keep the logo in a white rounded container there.
- Content max width: 1440px, matching the site's largest named container; center on ultra-wide screens.
- Sticky elements: top bar always; table headers and filter bars may stick below it (`top-16`).

### Report and document layout (Proposed)

- Cover: navy or brand-gradient block with the logo, report title (Unbounded 32px), period and author.
- Body: single column, 720–960px measure, 32px between sections, each section starting with an `h2` and optional KPI row.
- Print: A4/Letter portrait, 20mm margins, charts constrained to the column width, page-break before each `h2`. See [../examples/report-guidelines.md](../examples/report-guidelines.md).

## Z-index layers (Verified values)

`z-10` content accents, `z-20` section content over backgrounds, `z-30` overlapping cards, `z-40` sticky header, `z-50` overlays, `z-[999]` modals. **Proposed:** keep this order: header 40 → dropdowns 50 → modals 999 → toasts 1000.

## Related files

- [responsive-design.md](responsive-design.md) — how these layouts collapse.
- [components.md](components.md) — the pieces placed in these layouts.
- [design-tokens.md](design-tokens.md) — spacing and breakpoint tokens.
