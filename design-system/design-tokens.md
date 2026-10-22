# Design Tokens

The single machine-readable source for colors, fonts, radii, shadows, spacing
and breakpoints. Other files describe *when* to use a token; this file defines
*what* it is. Copy one of the two blocks below into a project rather than
hard-coding values.

Tokens marked `/* verified */` come from the site's Tailwind config as compiled
in its stylesheet (same names where the site has them). Tokens marked
`/* proposed */` are additions for product UI; see
[../brand/color-palette.md](../brand/color-palette.md) and
[../brand/typography.md](../brand/typography.md) for rationale.

## CSS custom properties

```css
:root {
  /* ---- Color: core (verified) ---- */
  --color-blue: #407DC3;          /* primary */
  --color-blue-1: #65DDF5;        /* gradient start */
  --color-blue-2: #3A6DBA;        /* gradient end; small-text blue */
  --color-black: #040815;         /* default text */
  --color-white: #FFFFFF;
  --color-navy: #142741;          /* hero / dark shell */
  --color-light-blue: #FAFBFF;    /* alt surface, footer */
  --color-light-purple: #F7F5FC;  /* alt surface */

  /* ---- Color: neutrals (verified) ---- */
  --color-dark-grey: #323643;
  --color-grey: #606471;
  --color-light-grey: #8D919F;
  --color-placeholder: #667085;
  --color-border-input: #D0D5DD;
  --color-border-focus: #50A6D8;
  --color-disabled-surface: #D4E0ED;
  --color-secondary-blue: #52BCD5;
  --color-soft-steel-blue: #8EA1BD;
  --color-skeleton-base: #EBEBEB;
  --color-skeleton-highlight: #F5F5F5;

  /* ---- Color: semantic ---- */
  --color-success-green: #34C759; /* verified: filled badges only */
  --color-error-red: #D92D20;     /* verified */
  --color-success-700: #1E7A3C;   /* proposed: success text */
  --color-success-50: #E8F8EE;    /* proposed */
  --color-warning-700: #B45309;   /* proposed */
  --color-warning-50: #FFF4E0;    /* proposed */
  --color-error-50: #FFF5F4;      /* proposed */
  --color-info-700: #3A6DBA;      /* proposed: = blue-2 */
  --color-info-50: #EEF4FF;       /* proposed: also the selected/active surface */

  /* ---- Color: accents (verified) ---- */
  --color-pink-1: #DB6E4F;
  --color-pink-2: #DA4E8F;
  --color-cta-start: #55C0D5;

  /* ---- Gradients (verified) ---- */
  --gradient-brand: linear-gradient(180deg, #65DDF5 0%, #3A6DBA 100%);
  --gradient-cta: linear-gradient(180deg, #55C0D5 0%, #3A6DBA 100%);
  --gradient-stat: linear-gradient(90deg, #DB6E4F 0%, #DA4E8F 100%);
  --gradient-submit: linear-gradient(163.84deg, #8B81FF 43.634%, #0B90FC 96.047%);

  /* ---- Typography (verified families) ---- */
  --font-unbounded: "Unbounded", Arial, sans-serif;
  --font-montserrat: "Montserrat", Arial, sans-serif;
  --font-display: var(--font-unbounded);   /* proposed alias */
  --font-body: var(--font-montserrat);     /* proposed alias */
  --font-mono: ui-monospace, "SF Mono", Menlo, Consolas, monospace; /* proposed */

  /* ---- Radius (verified values) ---- */
  --radius-sm: 4px;    /* rounded */
  --radius-md: 6px;    /* rounded-md */
  --radius-lg: 8px;    /* rounded-lg: buttons, inputs */
  --radius-xl: 12px;   /* about-page cards */
  --radius-2xl: 15px;  /* news cards, portfolio tiles */
  --radius-3xl: 20px;  /* stat cards */
  --radius-full: 9999px;

  /* ---- Shadows (verified: Tailwind defaults in use) ---- */
  --shadow-sm: 0 1px 2px 0 rgba(0,0,0,.05);
  --shadow:    0 1px 3px 0 rgba(0,0,0,.1), 0 1px 2px -1px rgba(0,0,0,.1);
  --shadow-md: 0 4px 6px -1px rgba(0,0,0,.1), 0 2px 4px -2px rgba(0,0,0,.1);   /* sticky header */
  --shadow-lg: 0 10px 15px -3px rgba(0,0,0,.1), 0 4px 6px -4px rgba(0,0,0,.1);
  --shadow-xl: 0 20px 25px -5px rgba(0,0,0,.1), 0 8px 10px -6px rgba(0,0,0,.1); /* stat cards */
  --shadow-2xl: 0 25px 50px -12px rgba(0,0,0,.25);                              /* card hover */
  --shadow-glow-blue: 0 0 37px 13px #BCD6DC;                                     /* decorative glow */

  /* ---- Spacing (verified 4px scale; values in use) ---- */
  --space-1: 4px;  --space-2: 8px;  --space-3: 12px; --space-4: 16px;
  --space-5: 20px; --space-6: 24px; --space-8: 32px; --space-10: 40px;
  --space-12: 48px; --space-14: 56px; --space-16: 64px; --space-20: 80px;
  --space-24: 96px; --space-28: 112px; --space-40: 160px;

  /* ---- Motion (verified) ---- */
  --duration-fast: 150ms;   /* Tailwind `transition` default */
  --delay-hover: 50ms;      /* `delay-50` on all buttons */
  --duration-flip: 800ms;   /* enterprise-card flip */
  --duration-skeleton: 1.5s;
  --ease-ui: linear;        /* `ease-linear` on buttons */
}
```

## Tailwind config

```js
// tailwind.config.js
module.exports = {
  theme: {
    container: { center: true, padding: '2rem', screens: { sm: '640px', md: '768px', lg: '1024px', xl: '1280px', '2xl': '1440px' } }, // verified: 2rem padding; site caps at 1440 then 1536
    extend: {
      colors: {
        // verified names
        blue: '#407DC3', 'blue-1': '#65DDF5', 'blue-2': '#3A6DBA',
        black: '#040815', navy: '#142741',
        'light-blue': '#FAFBFF', 'light-purple': '#F7F5FC',
        'dark-grey': '#323643', grey: '#606471', 'light-grey': '#8D919F',
        'secondary-blue': '#52BCD5', 'soft-steel-blue': '#8EA1BD',
        'success-green': '#34C759', 'error-red': '#D92D20',
        'pink-1': '#DB6E4F', 'pink-2': '#DA4E8F',
        // proposed
        placeholder: '#667085', 'border-input': '#D0D5DD', 'border-focus': '#50A6D8', 'disabled-surface': '#D4E0ED',
        success: { 50: '#E8F8EE', 700: '#1E7A3C' },
        warning: { 50: '#FFF4E0', 700: '#B45309' },
        error:   { 50: '#FFF5F4', 700: '#D92D20' },
        info:    { 50: '#EEF4FF', 700: '#3A6DBA' },
      },
      fontFamily: {
        primary: ['var(--font-unbounded)', 'Arial', 'sans-serif'],       // verified
        'secondary-1': ['var(--font-montserrat)', 'Arial', 'sans-serif'], // verified
        display: ['var(--font-unbounded)', 'Arial', 'sans-serif'],       // proposed alias
        body: ['var(--font-montserrat)', 'Arial', 'sans-serif'],         // proposed alias
      },
      borderRadius: { xl: '12px', '2xl': '15px', '3xl': '20px' },
      backgroundImage: {
        'brand-gradient': 'linear-gradient(180deg,#65DDF5 0%,#3A6DBA 100%)',
        'cta-gradient': 'linear-gradient(180deg,#55C0D5 0%,#3A6DBA 100%)',
        'stat-gradient': 'linear-gradient(90deg,#DB6E4F 0%,#DA4E8F 100%)',
      },
      boxShadow: { 'glow-blue': '0 0 37px 13px #BCD6DC' },
      transitionDelay: { 50: '50ms' },
    },
  },
};
```

Note: the site overrides Tailwind's `black` with `#040815`; keep that override
so `text-black` stays on-brand.

## Breakpoints (verified)

| Name | Min width | Site behavior |
|---|---|---|
| `sm` | 640px | Buttons go from full-width to auto; stat grid stays stacked |
| `md` | 768px | 3-column stat grid, 2-column forms, hero height 710px, decorative side borders appear |
| `lg` | 1024px | 2–3 column card grids, footer rows |
| `xl` | 1280px | Desktop navigation replaces hamburger; logo 240px; hero text 44px |
| `2xl` | 1440px (custom) and 1536px | Header container; wide grids (`2xl:grid-cols-12`) |

The site also uses many arbitrary `min-[…]` breakpoints (270–1890px) for fine
tuning; do not copy those into products. See
[responsive-design.md](responsive-design.md).

## Related files

- [../brand/color-palette.md](../brand/color-palette.md), [../brand/typography.md](../brand/typography.md)
- [layout-and-spacing.md](layout-and-spacing.md), [components.md](components.md)
