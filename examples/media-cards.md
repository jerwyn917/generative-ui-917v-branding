# Media Cards (places, activities, venues, products, people)

How to answer a **discovery or recommendation** request in 917 Ventures
branding without losing what a plain answer would show: photos of the thing,
a one-line description, two practical details and a source. Use this file for
"what can I do in Bali", "best coworking spaces in BGC", "compare these three
laptops", "who are the speakers", "restaurants near the office", news
round-ups, and any list of real-world things that a photo identifies faster
than an icon does.

Everything here is **Proposed** (the website has no media cards); the tokens
are the verified ones from [../design-system/design-tokens.md](../design-system/design-tokens.md).

## 1. Why this pattern exists

Without it the skill classifies such a request as "metric cards" or a
"mockup" and renders an icon grid with a title and a location line, while the
unbranded answer shows three photos with source credits, a description and
tips per item. Branding is a presentation layer: **the branded view carries
everything the plain answer would carry, plus the brand.** Never trade
content (photos, descriptions, practical details, sources) for styling.

## 2. Gather the content first

1. If the conversation does not already hold the facts, **search the web
   before rendering**, exactly as you would for a plain answer. Do not skip
   the search because a visual was requested.
2. For every item keep:

   | Field | Example |
   |---|---|
   | `name` | Mount Batur sunrise trek |
   | `where` / `when` | Kintamani · 2 AM start |
   | `summary` (one sentence, ≤ 160 characters) | A guided pre-dawn hike up the volcano with sunrise over the lake and Mount Agung. |
   | `tips` (two short practical details) | Leave around 2 AM · Book a guide |
   | `tags` (for filters) | Outdoors, Rainy-day friendly |
   | `images` (0–3) | `{ src, source, alt }` where `source` is the site the photo comes from |
   | `url` (optional) | the page to read more |

3. **Image sources.** Use image URLs that already appeared in web or image
   search results, the official site of the place or product, or Wikimedia
   Commons. Copy the image URL exactly as the search result, the page or the
   Commons API (`prop=imageinfo&iiprop=url&iiurlwidth=960` → `thumburl`) gives
   it; never assemble a thumbnail URL by hand (Commons serves only sizes it
   has rendered, so a made-up `640px-` path returns 400). Prefer landscape
   photos 640–960 px wide; never link a full-resolution original. Keep the
   source domain for the credit overlay. Skip an image rather than guess a
   URL: a broken photo is worse than none.
4. **Honesty.** Photos illustrate; they do not verify. If an opening time or
   price comes from a search result, say so in the reply, not in the card.

## 3. Pick the surface

| Signal | Render |
|---|---|
| Up to ~8 items, the user may filter, pick or ask for a plan | **Widget** (default) |
| More than ~8 items, or the user wants to keep or share it (an itinerary, a venue shortlist for a deck) | **Artifact** |
| No inline widget tool in the surface (Claude Code, Cowork) | Artifact |

## 4. Widget pattern (verified 2026-10-29 in the Claude Desktop widget host)

Rules on top of [interactive-chat-widget.md](interactive-chat-widget.md#3-widget-runtime-constraints-host-rules):

- **Photos are allowed in widgets.** Use plain `<img src="https://…" alt="…" loading="lazy" referrerpolicy="no-referrer">`. External photo hosts are not on the host's script allowlist, so treat each photo as optional: on `error` the tile is replaced by a branded placeholder (Tabler icon on `var(--surface-1)`) and the card stays intact. The three-photo strip, source credit at the bottom-right of each photo, title, one-line summary and two tips mirror the host's own result cards, so the branded card reads as a native one.
- **Exception to "no prose inside the widget":** each media card carries its name and one summary sentence. Longer explanations stay in the reply.
- Layout: a strip of 1–3 photos (`aspect-ratio: 16 / 10`, `object-fit: cover`, 8 px radius), then name (16 px / 500), location line (`var(--text-secondary)`), summary (14 px), tips as plain text separated by ` · `, and one `sendPrompt` button ending with ↗ ("Plan this ↗"). Filters above the list as `aria-pressed` toggles handled in JavaScript; the count on the right.
- Credit overlay: 11 px white text on `rgba(0,0,0,.55)`, bottom-right, no gradient. `alt` describes the photo, never the source.
- Brand: the `917 Ventures` wordmark line as in every widget; `#407DC3` only for the wordmark and the pressed filter; everything else host tokens so dark mode works.

Starter (replace `ITEMS`; keep the structure):

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Unbounded:wght@600&display=swap">
<style>
.v917m { font-size: 14px; color: var(--text-primary); }
.v917m .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); }
.v917m .wm { font-family: Unbounded, "Arial Black", Arial, sans-serif; font-weight: 600; font-size: 13px; color: #407DC3; letter-spacing: 0.02em; }
.v917m .filters button { padding: 6px 12px; }
.v917m .filters button[aria-pressed="true"] { background: var(--bg-accent); color: var(--text-accent); border-color: var(--border-accent); }
.v917m .card { background: var(--surface-2); border: 0.5px solid var(--border); border-radius: 12px; padding: 12px; margin: 0 0 12px; }
.v917m .strip { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 6px; margin: 0 0 12px; }
.v917m .ph { position: relative; aspect-ratio: 16 / 10; border-radius: 8px; overflow: hidden; background: var(--surface-1); }
.v917m .ph img { width: 100%; height: 100%; object-fit: cover; display: block; }
.v917m .ph .credit { position: absolute; right: 6px; bottom: 6px; font-size: 11px; color: #fff; background: rgba(0,0,0,.55); padding: 2px 6px; border-radius: 4px; }
.v917m .ph .ico { display: none; position: absolute; inset: 0; align-items: center; justify-content: center; color: var(--text-muted); font-size: 24px; }
.v917m .ph.missing img, .v917m .ph.missing .credit { display: none; }
.v917m .ph.missing .ico { display: flex; }
.v917m .name { font-size: 16px; font-weight: 500; margin: 0 0 2px; }
.v917m .where { font-size: 13px; color: var(--text-secondary); margin: 0 0 6px; }
.v917m .tips { font-size: 13px; color: var(--text-secondary); margin: 8px 0 10px; }
</style>
<div class="v917m">
<h2 class="sr-only">Things to do in Bali during the start of the wet season, as cards with photos, filters by type and a button to plan each one.</h2>
<div style="display:flex; flex-wrap:wrap; align-items:center; gap:8px; margin:0 0 12px;">
  <span class="wm">917 Ventures</span>
  <span style="font-size:13px; color:var(--text-secondary);">Bali · things to do · wet season starting · photos from the credited sites</span>
</div>
<div style="display:flex; flex-wrap:wrap; align-items:center; justify-content:space-between; gap:8px; margin:0 0 16px;">
  <div class="filters" style="display:flex; flex-wrap:wrap; gap:6px;" role="group" aria-label="Filter by type">
    <button type="button" data-tag="all" aria-pressed="true">All</button>
    <button type="button" data-tag="Outdoors" aria-pressed="false">Outdoors</button>
    <button type="button" data-tag="Culture" aria-pressed="false">Culture</button>
    <button type="button" data-tag="Rainy-day friendly" aria-pressed="false">Rainy-day friendly</button>
  </div>
  <span id="v917m-count" style="font-size:13px; color:var(--text-secondary);"></span>
</div>
<div id="v917m-list"></div>
</div>
<script>
(function () {
  const STATE = { tag: "all" };
  const ITEMS = [
    { name: "Mount Batur sunrise trek", where: "Kintamani · 2 AM start", summary: "A guided pre-dawn hike up the volcano, with sunrise over the lake and Mount Agung; usually clearest early in the morning.", tips: ["Leave around 2 AM", "Book a guide"], tags: ["Outdoors"], prompt: "Plan a Mount Batur sunrise trek for me from where I am staying in Bali",
      images: [
        { src: "https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Sunrise_at_Mt_Batur%2C_Bali.jpg/960px-Sunrise_at_Mt_Batur%2C_Bali.jpg", source: "wikimedia", alt: "Sunrise over Lake Batur seen from the summit" },
        { src: "https://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Sunrise_from_Mount_Batur.jpg/960px-Sunrise_from_Mount_Batur.jpg", source: "wikimedia", alt: "Hikers on Mount Batur at first light" },
        { src: "https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sunrise_on_Mount_Batur_-_panoramio.jpg/960px-Sunrise_on_Mount_Batur_-_panoramio.jpg", source: "wikimedia", alt: "Clouds below the Mount Batur crater rim" }
      ] },
    { name: "Tegallalang rice terraces", where: "Ubud · morning", summary: "The classic terraced paddies north of Ubud; the wet season makes them especially green.", tips: ["Go early", "Bring a rain jacket"], tags: ["Outdoors", "Culture"], prompt: "Plan a morning at the Tegallalang rice terraces near Ubud",
      images: [
        { src: "https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Tegallalang_Rice_Terraces_Bali.jpg/960px-Tegallalang_Rice_Terraces_Bali.jpg", source: "wikimedia", alt: "Terraced rice paddies at Tegallalang" },
        { src: "https://upload.wikimedia.org/wikipedia/commons/thumb/8/8a/Rice_terraces%2C_Bali.jpg/960px-Rice_terraces%2C_Bali.jpg", source: "wikimedia", alt: "Green rice terraces with palm trees" },
        { src: "https://upload.wikimedia.org/wikipedia/commons/thumb/c/c5/Tegallalang_rice_terraces_SF0001.jpg/960px-Tegallalang_rice_terraces_SF0001.jpg", source: "wikimedia", alt: "Steps between rice terraces" }
      ] }
  ];
  const esc = s => String(s).replace(/[&<>"]/g, c => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;" }[c]));
  const $ = id => document.getElementById(id);
  function photo(p) {
    return '<div class="ph"><img src="' + esc(p.src) + '" alt="' + esc(p.alt) + '" loading="lazy" referrerpolicy="no-referrer"><span class="credit">' + esc(p.source) + '</span><i class="ti ti-photo ico" aria-hidden="true"></i></div>';
  }
  function card(it, i) {
    return '<div class="card">' +
      (it.images.length ? '<div class="strip">' + it.images.map(photo).join("") + '</div>' : "") +
      '<p class="name">' + esc(it.name) + '</p><p class="where">' + esc(it.where) + '</p>' +
      '<p style="margin:0;">' + esc(it.summary) + '</p>' +
      '<p class="tips">' + it.tips.map(esc).join(" · ") + '</p>' +
      '<button type="button" data-i="' + i + '">Plan this ↗</button></div>';
  }
  function wire() {
    document.querySelectorAll("#v917m-list .ph img").forEach(img => {
      const mark = () => { if (!img.naturalWidth) img.parentNode.classList.add("missing"); };
      img.addEventListener("error", () => img.parentNode.classList.add("missing"));
      img.addEventListener("load", mark);
      if (img.complete) mark();
    });
    document.querySelectorAll("#v917m-list button[data-i]").forEach(b => b.addEventListener("click", () => {
      const it = ITEMS[Number(b.dataset.i)];
      if (typeof sendPrompt === "function") sendPrompt(it.prompt);
    }));
  }
  function render() {
    const rows = ITEMS.filter(it => STATE.tag === "all" || it.tags.indexOf(STATE.tag) !== -1);
    $("v917m-list").innerHTML = rows.map(it => card(it, ITEMS.indexOf(it))).join("");
    $("v917m-count").textContent = rows.length + " of " + ITEMS.length + " shown";
    wire();
  }
  document.querySelectorAll(".v917m .filters button").forEach(b => b.addEventListener("click", () => {
    STATE.tag = b.dataset.tag;
    document.querySelectorAll(".v917m .filters button").forEach(x => x.setAttribute("aria-pressed", String(x === b)));
    render();
  }));
  render();
})();
</script>
```

## 5. Artifact pattern

Artifacts cannot hotlink images. Two ways to keep the photos:

- **Claude Code (Artifact tool):** download each photo to the scratchpad, upload it with `asset: true` to the artifact, and reference the returned asset URL in `<img>`. Keep ≤ 3 photos per item at ≤ 960 px.
- **Claude Desktop / claude.ai / Cowork:** images must be `data:` URIs, which are too heavy for a list of photos. Render the same card without the strip: a 56 px tile with a Heroicons glyph on `--accent-soft`, the name, summary, tips, and a "Photos on <source> ↗" link to the source page. Say in the reply that the photos are one click away.

Card spec for artifacts: white card, 15 px radius, `shadow-sm` + light border, 20 px padding, name in Montserrat 700 18 px, summary 14 px `--fg-2`, tips 13 px `--muted`, the primary button classes from [../design-system/components.md](../design-system/components.md). Light and dark tokens as in [generative-ui-artifact.md](generative-ui-artifact.md#4-brand-dark-mode-tokens-artifacts-only).

## 6. Deliver

1. Render the widget (title such as `v917_bali_things_to_do`) or publish the artifact.
2. Reply in one or two lines: what was found, the sources searched, which photos could not be shown, and anything time-sensitive to double-check (hours, prices, weather).
3. On "Plan this ↗", answer with the plan in prose and re-render only if the user asks for a changed view.

## Checklist

- [ ] Web search done first when the facts are not in the conversation
- [ ] Every item has name, where/when, one-sentence summary, two tips and a source; the branded view shows no less than the plain answer would
- [ ] 0–3 photos per item from search results, the official site or Wikimedia Commons, URLs copied not constructed, 640–960 px, with a source credit and descriptive `alt`
- [ ] Widget: `error` handler swaps a missing photo for the branded placeholder; artifact: assets (Claude Code) or icon tile plus source link
- [ ] Filters as `aria-pressed` toggles; count shown; one `sendPrompt` action per card ending with ↗
- [ ] Host tokens for text and surfaces; brand blue only for the wordmark and pressed filter

## Related files

- [interactive-chat-widget.md](interactive-chat-widget.md) (widget host rules), [generative-ui-artifact.md](generative-ui-artifact.md) (artifact runtime), [../brand/logo-and-assets.md](../brand/logo-and-assets.md) (content vs decorative imagery), [../design-system/components.md](../design-system/components.md) (news card, cards)
