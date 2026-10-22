# Components

Visual and behavioral rules for reusable UI. Each component states what is
**Verified** on 917ventures.com and what is **Proposed** for products. Tokens
are defined in [design-tokens.md](design-tokens.md); states in
[interaction-states.md](interaction-states.md); forms in
[forms-and-inputs.md](forms-and-inputs.md); tables in
[tables-and-data-display.md](tables-and-data-display.md).

Always prefer an existing component in the host project (e.g. its Button or
Card) and restyle it with these tokens, rather than adding a new component
library.

## Buttons (Verified)

The site's exact button classes:

```html
<!-- Primary -->
<button class="bg-blue text-white px-8 py-3 font-bold rounded-lg inline-flex items-center justify-center text-center
               transition ease-linear delay-50 hover:opacity-70 disabled:opacity-80 cursor-pointer w-full sm:w-auto">
  Send Message
</button>

<!-- Secondary / outline -->
<button class="bg-white text-blue border border-blue px-8 py-3 font-bold rounded-lg inline-flex items-center justify-center
               transition ease-linear delay-50 hover:opacity-70 disabled:opacity-80 cursor-pointer w-full sm:w-auto">
  Partner With Us
</button>

<!-- Inverse (on gradient / navy) -->
<button class="bg-white text-blue px-8 py-3 font-bold rounded-lg ... hover:opacity-70">Explore 917V Life</button>

<!-- Icon button (carousel arrows) -->
<button class="bg-blue text-white w-6 h-6 p-1 rounded-full transition ease-linear delay-50 hover:brightness-125">…</button>
<button disabled class="bg-[#D4E0ED] text-blue w-6 h-6 p-1 rounded-full">…</button>
```

Spec: Montserrat 700 16px/24px, padding 12px × 32px (48px tall), radius 8px, full width below `sm`. The header's primary button can hold an image (Velocity wordmark) at `h-6`.

**Proposed additions for products** (same tokens):

| Variant | Style | Use |
|---|---|---|
| Primary | as verified | One per view: save, submit, create |
| Secondary | as verified outline | Cancel, back, secondary actions |
| Ghost | transparent, `text-blue-2`, hover `bg-light-blue` | Toolbar actions, table row actions |
| Destructive | `bg-error-red text-white`, hover opacity 70% | Delete / irreversible; always behind a confirm dialog |
| Link button | `text-blue-2 font-semibold underline-offset-4 hover:underline` | Inline actions in text or cells |
| Sizes | `sm` 36px (`px-4 py-2 text-sm`), `md` 44px (`px-6 py-2.5`), `lg` 48px (verified) | Products default to `md`; marketing uses `lg` |
| Icon-only | 40 × 40px, `rounded-lg` or `rounded-full`, `aria-label` required | Toolbars |
| Loading | keep width, replace label with a 20px spinner + "Saving…", `aria-busy="true"` | Async submit |

Rules: verb-first labels; never two primary buttons side by side; destructive actions never use blue; minimum touch target 44px on mobile.

## Action links (Verified)

- Nav links: Montserrat 500 16px `dark-grey`; active: 700, `blue`, `underline` with `text-underline-offset: 6px` (`.active-menu-item`).
- Inline "Learn More" links: `blue`, 700, no underline at rest.
- Footer links: `text-sm font-medium uppercase`; email link `text-blue font-bold`.

**Proposed:** in body text, links are `blue-2` 600 with underline on hover and focus; `blue` 700 without underline is reserved for standalone CTA links.

## Navigation bar (Verified)

- `sticky top-0 z-40 bg-white shadow-md`, max height 96px, `2xl:container mx-auto`.
- Logo left (135px → 240px at `xl`). Items right-aligned in a row at `xl`+ with `gap-8`; below `xl` a hamburger (Heroicons `bars-3`, 24px) opens a full-height stacked menu (`px-12 pb-12 gap-8`, scrollable).
- Order: Home · Portfolio · 917V Life · News · Careers · About Us · [outline button] · [primary button].
- Active page: bold blue underlined.

**Proposed app top bar:** 64px tall (`h-16`), same white + `shadow-md`, logo at 135px, page/product name in Montserrat 600 after a 1px `#D0D5DD` divider, right side: search (optional), notifications bell (Heroicons `BellIcon` 24 outline), user avatar (32px `rounded-full`) with a dropdown.

## Sidebar (Proposed)

- 256px wide, white, `border-r border-[#D0D5DD]`; collapses to 64px icon rail below `lg` or on toggle.
- Item: 40px tall, `px-3`, `rounded-lg`, 24px outline icon + Montserrat 500 14px label, `gap-3`. Hover `bg-light-blue`; active `bg-[#EEF4FF] text-blue-2 font-semibold` with the icon switched to solid; focus ring per [interaction-states.md](interaction-states.md).
- Group headings: 12px Montserrat 600 uppercase `grey` with 0.04em tracking, 24px top margin.
- Navy variant: `bg-navy`, items `text-white/80`, active `bg-white/10 text-white`, logo inside a white 8px-radius container.

## Cards (Verified)

| Card | Spec |
|---|---|
| Stat card | `bg-white rounded-[20px] shadow-xl py-8 px-12 flex flex-col gap-y-4 items-center`, max 418px wide; number Unbounded 700 36px stat-gradient; label Montserrat 700 16px uppercase `#232323` |
| News card | `bg-white rounded-[15px] hover:shadow-2xl cursor-pointer`, image 400 × 217 on top, body `p-5 xl:p-8` with `gap-y-3 xl:gap-y-6`; category 14–16px 600; title 16–18px 700; excerpt 12–16px 400; date 12–14px 300 |
| Portfolio tile | `rounded-[15px] border border-[#50A6D8] aspect-[2.77/1] p-2.5 sm:p-6 hover:opacity-40`, logo centered; optional diagonal ribbon (see Badges) |
| Testimonial card | White, 1px `secondary-blue`-family border, ~20px radius, quote glyph top-left tinted coral, Montserrat 400 16px quote, 48px round avatar, name 700 `blue`, role 400 `grey` |
| Team / about card | `bg-white rounded-[12px] px-6 py-10 flex flex-col items-center gap-6` wrapped in a `p-[1px] rounded-[12px]` gradient border |
| Life card | `rounded-[15px] border border-secondary-blue py-4 px-5` |

**Proposed product card:** `bg-white rounded-2xl (15px) shadow-sm border border-[#D0D5DD]/60 p-6`, header row with title (Montserrat 700 16px) and optional action, body, optional footer. Hover elevation (`shadow-lg`) only when the whole card is clickable.

## Summary metric cards (stat cards)

Verified anatomy applied to data (Proposed details):

```
┌──────────────────────────────┐
│ Label (12px 500 grey)   icon │  icon: 24px outline in a 40px #EEF4FF tinted circle (optional)
│ 1,284,900 (Unbounded 700 28) │  flat text-black; gradient text only on cover/hero cards
│ ▲ 12.4% vs last month        │  delta: 12px 600, success-700 / error-red, with arrow icon
└──────────────────────────────┘
```

- Four across on desktop, two on tablet, one or two on mobile (`grid-cols-2 lg:grid-cols-4 gap-4`).
- Use tabular numerals; format per [charts-and-visualizations.md](charts-and-visualizations.md#number-date-and-currency-formatting).
- Keep the marketing version (gradient numerals, uppercase labels, 20px radius, `shadow-xl`) for landing pages and report covers only.

## Badges and status indicators

**Verified:** diagonal ribbon `bg-success-green text-white font-bold uppercase text-[8px] sm:text-[11px] rotate-45 w-40` for "EXITED" / "SPUN IN". Pagination pill: `w-10 h-10 rounded-full bg-blue text-white font-semibold`.

**Proposed status badges:** `inline-flex items-center gap-1 rounded-full px-2.5 py-0.5 text-xs font-semibold` with a 6px dot:

| Status | Classes |
|---|---|
| Success / Active | `bg-success-50 text-success-700` |
| Warning / Pending | `bg-warning-50 text-warning-700` |
| Error / Failed | `bg-error-50 text-error-700` |
| Info / In progress | `bg-info-50 text-info-700` |
| Neutral / Draft | `bg-gray-100 text-dark-grey` |
| Brand / Featured | `bg-blue text-white` |

Counts use the same pill in `bg-blue text-white` at 20px tall. Never use the ribbon for status in product tables.

## Tabs, accordions, breadcrumbs (Proposed; not on the site)

- **Tabs:** horizontal list under the page header; tab = Montserrat 500 14px `grey`, 40px tall, `px-4`; active = `text-blue font-semibold` with a 2px `bg-blue` bottom border; hover `text-dark-grey`. Use `role="tablist"`, arrow-key navigation.
- **Accordion:** header 48px, Montserrat 600 14px, chevron (Heroicons `ChevronDownIcon` 20) rotating 180° in 150ms; body `pb-4 text-sm text-dark-grey`; dividers `border-[#D0D5DD]`.
- **Breadcrumbs:** 12px `grey`, separator `ChevronRightIcon` 16px `light-grey`, current page `dark-grey` 500, not a link.

## Dropdowns and menus (Proposed)

- Trigger: secondary button or ghost button with `ChevronDownIcon` 20px.
- Panel: `bg-white rounded-lg shadow-lg border border-[#D0D5DD] py-1 min-w-[12rem]`, 8px offset, z-index 50.
- Item: 36px, `px-3 text-sm text-black`, hover/focus `bg-light-blue`, selected `text-blue font-semibold` with a `CheckIcon`; destructive items `text-error-red`.
- Keyboard: arrow keys, Enter, Esc; focus returns to the trigger.

## Dialogs, modals, confirmation prompts

**Verified:** the About page opens a management-team modal (`.management-modal`) with a custom thin scrollbar (5px, thumb `#A5A1A1`, radius 49px) over an illustrated background, at `z-[999]`.

**Proposed:**

- Overlay `bg-black/50` (site uses `bg-opacity-40`/`50` overlays); panel `bg-white rounded-2xl shadow-2xl p-6 w-full max-w-lg` (`max-w-2xl` for forms), centered; full-screen sheet below `sm`.
- Header: title Montserrat 700 20px + close icon button (`XMarkIcon` 24) top-right. Footer: actions right-aligned, secondary then primary, `gap-3`.
- Confirmation prompt: short title ("Delete report?"), one-sentence consequence, Cancel (secondary) + the action (primary, or destructive style for deletions). Focus the least destructive button first.
- `role="dialog"`, `aria-modal`, `aria-labelledby`; trap focus; Esc closes; return focus to the trigger.

## Alerts and notifications (Proposed)

- Inline alert: `rounded-lg border px-4 py-3 flex gap-3 text-sm`, left icon 20px solid, colors from the badge table (surface 50 + text 700 + border = text color at 30% opacity). Dismissible alerts get an `XMarkIcon` ghost button.
- Toast: same styling on white with `shadow-xl`, top-right, 4–6 seconds, `role="status"` (errors `role="alert"` and persistent until dismissed).
- Banner (page-level): full-width `bg-info-50` strip under the top bar.

## Tooltips and contextual help (Proposed)

- `bg-navy text-white text-xs rounded-md px-2 py-1 shadow-md`, max 240px, 150ms fade, 6px offset, arrow optional.
- Show on hover and focus; never put essential actions inside a tooltip.
- Help icon: `InformationCircleIcon` 16px `light-grey` next to a label, with the tooltip text also available as `aria-describedby`.

## Empty states

**Verified:** none observed (CMS content always present). **Proposed:**

- Centered in the card/table area, `py-12`: 48px outline icon or one brand illustration in `light-grey`, title Montserrat 600 16px `black`, one-sentence explanation 14px `grey`, optional primary action.
- Copy pattern: "No reports yet" / "Reports you generate will appear here." / [Create report]. For filtered-empty: "No results match your filters" + [Clear filters] (secondary).

## Loading indicators

**Verified:** `react-loading-skeleton` blocks (base `#EBEBEB`, highlight `#F5F5F5`, 1.5s shimmer, 4px radius, disabled under `prefers-reduced-motion`) sized to the content they replace (e.g. 400 × 217 image, text lines).

**Proposed:** skeletons for cards, table rows (5 rows), KPI numbers and chart areas; a 20px `blue` circular spinner (`border-2 border-blue border-t-transparent rounded-full animate-spin`) for buttons and small areas; a top progress bar for page navigation. Never show a spinner over a skeleton.

## Error messages

- Field-level: see [forms-and-inputs.md](forms-and-inputs.md#validation-and-error-messages).
- Page/section-level: inline error alert with a retry button; keep the layout (do not collapse the card).
- Full-page error: logo mark, Unbounded 24px title ("Something went wrong"), 14px `grey` explanation, primary "Try again", secondary "Go to dashboard".

## Carousels (Verified)

keen-slider with a visually hidden "Slide N of M" text, previous/next 24px round blue buttons (disabled state `#D4E0ED`), one slide visible at a time, no autoplay observed. **Proposed:** avoid carousels in products; use tabs or a scrollable row.

## Dashboard layouts and report filters

See [../examples/dashboard-guidelines.md](../examples/dashboard-guidelines.md) and [../examples/report-guidelines.md](../examples/report-guidelines.md).

## Related files

- [forms-and-inputs.md](forms-and-inputs.md), [tables-and-data-display.md](tables-and-data-display.md), [interaction-states.md](interaction-states.md), [../brand/icons.md](../brand/icons.md)
