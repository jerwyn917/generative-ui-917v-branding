# Logo and Brand Assets

Verified from the files served by https://917ventures.com. Download the
originals from the site rather than redrawing them; never recreate the logo in
CSS or as text.

## Logo variations (Verified)

| Asset | URL path | Format / size | What it is | Where the site uses it |
|---|---|---|---|---|
| Primary horizontal logo | `/917ventures-logo.png` | PNG, 964 × 184, transparent | "917V" in near-black (`#040815`-like) with a blue "V" (`#407DC3`-like, a lighter overlap wedge where the 7 meets the V), followed by "ENTURES" in blue, thin caps | Sticky header on every page (135px wide on mobile, 240px on ≥ xl) |
| App icon / mark | `/android-chrome-512x512.png`, `/android-chrome-192x192.png` | PNG, square, white background | "917V" mark only (no "ENTURES"), centered with generous padding | PWA icons (`/site.webmanifest`) |
| Apple touch icon | `/apple-touch-icon.png` | PNG, 180 × 180 | Same mark | iOS home screen |
| Favicon | `/favicon-32x32.png`, `/favicon-16x16.png` | PNG | Same mark, reduced | Browser tab |
| Safari pinned tab | `/safari-pinned-tab.svg` | Monochrome SVG (potrace outline) | Single-color "917V" silhouette | Safari pinned tabs (`color="#5bbad5"` in the link tag — a generator default, not a brand color) |
| Social preview | `/seo-home-thumbnail.jpg` | JPG | Open Graph / Twitter card image | `og:image` |
| Velocity wordmark | `/velocity_logo_trim.png` | PNG, 107 × 16, grey+alpha | Italic "VELOCITY" wordmark, white | Inside the primary header button |

Manifest values: `name` / `short_name` = "917Ventures", `theme_color` and `background_color` = `#ffffff`, `msapplication-TileColor` = `#da532c` (generator default, not brand).

Not found on the site (and therefore **not verified**): a white/reverse logo file, a vertical lockup, an SVG version of the full logo, or published clear-space rules. The hero and footer place the logo only on white.

## Logo usage rules

Verified behaviors are marked; the rest are **Proposed** rules that follow standard practice and the way the site treats the mark.

1. **Placement (Verified):** top-left of a white header, vertically centered in a 96px (`h-24`) bar, linking to the home page.
2. **Sizes (Verified):** 135px wide on small screens, 240px wide from 1280px up. Proposed minimum: 100px wide for the horizontal logo; 24px for the square mark.
3. **Background (Verified on site, Proposed as rule):** always on white or `light-blue #FAFBFF`. If a dark surface is unavoidable (navy sidebar, report cover), place the logo inside a white rounded container (12px radius, 12px padding) rather than recoloring it — no reverse version is published.
4. **Clear space (Proposed):** keep at least the height of the "V" free on all sides.
5. **Do not** stretch, rotate, recolor, add shadows or outlines, or set it in a gradient. Do not type "917Ventures" in Unbounded as a logo substitute; if an image is impossible (plain-text email, ASCII context), write "917Ventures".
6. **Mark vs. logo (Proposed):** use the square "917V" mark for favicons, app icons, avatars, loading screens and collapsed sidebars; use the horizontal logo for headers, report covers and footers.
7. **Sub-brands:** portfolio company logos are loaded from the CMS (S3 bucket) as PNGs. Show them inside white tiles with a 1px `#50A6D8` border and 15px radius at a fixed 2.77:1 aspect ratio (Verified portfolio tile spec) so mixed logo shapes stay aligned. Never place a partner logo directly on the brand gradient.

## Supporting imagery and decorative assets (Verified)

| Asset | Path | Use |
|---|---|---|
| Globe / dot-grid world map video and image | hero video with `mix-blend-color-dodge`, feathered into `#142741` by `.hero-video-feather` | Homepage hero |
| `/circle.webp` | Large decorative circle | About page ("logo-circle-height": 300–530px by viewport height) |
| `/gradient-bg.webp`, `/wave.png`, `/ripples.svg` | Soft gradient and wave backgrounds, faded to white with `linear-gradient(180deg, #fff 0, transparent 70%)` | Section backgrounds |
| `/about-no-astronaut.png`, `/management-modal-bg.png` | Space-themed illustration backgrounds | About page, team modal |
| `/partner-banner.webp`, `/partner-person.webp`, `/partner-left-border.svg`, `/partner-right-border.svg` | Partner page banner, cut-out person, decorative side borders at 70% opacity | Partner page and footer CTA band |
| `/quote-icon.svg` | 58 × 43 double quotation mark glyph (black path; shown tinted coral/pink on the site) | Testimonial cards |
| `/facebook-icon.svg`, `/instagram-icon.svg`, `/linkedin-icon.svg` | 32 × 32 social glyphs, `#272E38` on a white rounded square (`rx` 9.14) | Footer |

Imagery style (Verified): photography is of real people (customers, team) in
rounded crops; illustration is cosmic/space themed (globe, astronaut, planet)
with soft gradients. **Proposed:** in products, use imagery only in empty
states and onboarding; keep it to one illustration per screen, and prefer the
abstract dot-grid/gradient treatment over stock photos.

## Asset handling in projects (Proposed)

- Store copies under `public/brand/` (`917ventures-logo.png`, `917v-mark.png`, social SVGs) and reference them by path; do not hotlink to 917ventures.com in production.
- Provide `alt="917Ventures"` on the logo; mark decorative images `alt=""`.
- Serve PNG logos at 2× for retina (the source is 964px wide, which covers up to ~480px display width).
- Request an SVG master and a reverse (white) version from the brand owner before building dark surfaces; until then, follow rule 3 above.

## Related files

- [icons.md](icons.md) — UI icon system (distinct from brand assets).
- [color-palette.md](color-palette.md) — colors that appear in the logo.
