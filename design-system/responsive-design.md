# Responsive Design

917ventures.com is mobile-first Tailwind: base styles target phones and
`sm`/`md`/`lg`/`xl`/`2xl` prefixes add desktop behavior. Verified behaviors
are listed first; product rules are **Proposed**.

## Breakpoints (Verified)

| Token | Min width | Role on the site |
|---|---|---|
| base | 0 | Single column, full-width buttons, hamburger nav, logo 135px |
| `sm` | 640px | Buttons become auto width; hero 640px tall; font steps (hero 32px) |
| `md` | 768px | 3-column stat row, 2-column form rows, hero 710px, side decorations appear, hero video feather switches from vertical to horizontal |
| `lg` | 1024px | 2–3 column card grids; footer becomes rows; split layouts (`lg:col-span-6`) |
| `xl` | 1280px | **Desktop navigation appears** (until then the hamburger menu is used); logo 240px; hero headline 44px; news cards 586px tall |
| `2xl` | 1440px / 1536px | Container caps; 12-column grids; wide gaps |

The site also uses arbitrary breakpoints (`min-[275px]`, `min-[415px]`, `min-[460px]`, 1700–1890px) to fine-tune tile sizes and hero text. **Do not** replicate those in products; stay on the named scale.

## Verified responsive behaviors

- **Header:** sticky at all sizes; below `xl` the nav collapses into a full-height stacked menu with 32px gaps and the two CTA buttons at the bottom.
- **Hero:** height 520 → 640 → 710px; headline 24 → 32 → 44px; the globe video shifts right on desktop and is feathered into the navy background on all sizes.
- **Stat cards:** stack vertically with 12px gap, then 3 across at `md` with 16px gap; they overlap the hero at every size.
- **Portfolio tiles:** always a 3-column grid; tile height scales from 20px (tiny screens) to 85px (`sm`+) and radius from 6px to 15px; ribbon text 8px → 11px.
- **Cards:** padding 20px → 32px; image and typography step up at `xl`.
- **Forms:** one column, then two for short fields at `md`; submit button full width below `sm`.
- **Footer:** stacked and centered, then two columns at `md`, rows at `lg`.
- **Decorative side borders and large illustrations:** hidden below `md`.

## Proposed product rules

### Layout by breakpoint

| Area | < 640 (phone) | 640–1023 (tablet) | 1024–1279 (small desktop) | ≥ 1280 (desktop) |
|---|---|---|---|---|
| App shell | Top bar + bottom sheet / drawer nav (hamburger) | Top bar + icon rail (64px) | Sidebar 256px (collapsible) | Sidebar 256px |
| Content padding | 16px | 24px | 24px | 32px |
| KPI cards | 1–2 per row | 2 | 4 | 4 |
| Chart cards | 1 per row, 240–280px tall | 1–2 | 2 | 2–3 (6/12 col) |
| Tables | Card list per row | Horizontal scroll, sticky first column | Full table | Full table |
| Filters | Collapsed into a "Filters" button opening a sheet | Inline row, wraps | Inline row | Inline row |
| Dialogs | Full-screen sheet | Centered, max 512px | Centered | Centered |
| Page title | 20px | 24px | 24px | 24px |

### Rules

1. **Mobile-first.** Write base styles for 360px and add `md`/`lg`/`xl` overrides, as the site does.
2. **Container:** reuse the verified 32px padding on desktop; reduce to 16px below `sm` (the site keeps 32px even on phones, which is generous for product screens — this is a deliberate product adjustment).
3. **Touch targets:** ≥ 44 × 44px on phones; table actions move into a menu.
4. **Typography:** never scale body text below 14px on phones; headlines step down one level per breakpoint (24 → 20px).
5. **Images and charts:** fluid width, fixed aspect ratio or explicit height; charts re-render on resize (amCharts handles this; keep the responsive theme on).
6. **Navigation:** the verified pattern is a hamburger below `xl`; for products with a sidebar, collapse to an icon rail at `md` and a drawer below `md`.
7. **Sticky elements:** header only on phones; header + table header on desktop.
8. **Orientation and zoom:** support 200% browser zoom without horizontal scroll (equivalent to a 640px layout at 1280px).
9. **Test at:** 360 × 740, 390 × 844, 768 × 1024, 1024 × 768, 1280 × 800, 1440 × 900, 1920 × 1080.

### Tailwind snippet

```html
<div class="container mx-auto px-4 md:px-6 xl:px-8 py-6 md:py-8">
  <div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4"> <!-- KPI row --> </div>
  <div class="mt-6 grid grid-cols-1 lg:grid-cols-2 gap-6"> <!-- charts --> </div>
</div>
```

## Related files

- [layout-and-spacing.md](layout-and-spacing.md), [design-tokens.md](design-tokens.md#breakpoints-verified), [tables-and-data-display.md](tables-and-data-display.md#responsive-tables), [charts-and-visualizations.md](charts-and-visualizations.md#responsive-layouts-and-mobile-behavior)
