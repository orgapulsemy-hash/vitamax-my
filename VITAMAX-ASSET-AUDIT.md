# VitaMAX Asset Infrastructure Audit

> Priority-1 infrastructure audit of every image, banner, icon, and file path across all 19 HTML pages.
> Cross-checked against the 55 files in `vitamax-site/vitamax-images/`. **No redesign — diagnosis only.**
> Method: extracted every `src`/`href`/`url()` asset reference (base64 data-URIs excluded) and tested each against disk.

**Headline result:** 48 distinct asset references. **1 truly broken live image (404)**, **2 missing product images shown as emoji placeholders**, **2 site-wide missing assets (favicon, social image)**, **1 path-consistency issue**, plus pervasive emoji-as-icon usage. Most product imagery loads correctly.

---

## A. Broken images / 404s (LIVE references — visibly broken in browser)

| # | File name | Current path (as coded) | Expected/real path | Where referenced | Why it fails |
|---|-----------|-------------------------|--------------------|------------------|--------------|
| A1 | `gladiator-3.png` | `/vitamax-images/gladiator-3.png` | *file does not exist* | `product-detox-honey.html:358` (gallery thumbnail, live `<img>` + `setImg()` onclick) | The Gladiator product ships only `gladiator-1/2/4.png`. **No `gladiator-3.png` exists.** This thumbnail renders a broken-image icon, and clicking it loads a 404 into the main viewer. |

**This is the only reference that produces a visibly broken image on a live page.**

---

## B. Missing product images currently masked by emoji placeholders

These are not 404s (the broken refs sit inside HTML comments), but the **product image is missing** and an emoji is shown in its place. A real replacement image **already exists on disk** (`Vitamax-wholesale.png`).

| # | File name (intended) | Current state (rendered) | Expected path | Where | Why it fails |
|---|----------------------|--------------------------|---------------|-------|--------------|
| B1 | `product-wholesale.jpg` | 📦 emoji in `<span class="img-placeholder">` | `/vitamax-images/product-wholesale.jpg` (never created) | `shop.html:576–577` (wholesale product card) | Image was never produced; a developer comment `<!-- REPLACE: ... -->` left the emoji as a stand-in. The wholesale card on the shop grid has no real photo. |
| B2 | `wholesale.jpg` | 📦 emoji in `<span class="emoji-ph" id="mainEmoji">` (+ emoji thumbnails 📦 🍯 🌍) | `vitamax-images/wholesale.jpg` (never created) | `product-wholesale.html:306–307` (main product image + `thumb-row`) | Same pattern — `<!-- TO ADD IMAGE: ... -->` comment, emoji placeholder never replaced. The wholesale **product page has no product photography at all** — its entire gallery is emoji. |

> **Good news:** real wholesale carton imagery exists — `Vitamax-wholesale.png`, `mix-carton-1.png`, `mix-carton-2.png` — so B1/B2 can be fixed by pointing to existing files, no new assets needed.

---

## C. Site-wide missing assets

| # | Asset | Current state | Expected | Why it matters |
|---|-------|---------------|----------|----------------|
| C1 | **Favicon** | **None on any of the 19 pages** — no `<link rel="icon">` anywhere | A favicon file (e.g. `/favicon.ico` or `/vitamax-images/favicon.png`) linked in every `<head>` | Every page silently 404s `/favicon.ico`; browser tabs show a blank/default icon. A premium brand with no favicon reads as unfinished. |
| C2 | **Social share image (OG/Twitter)** | **None** — no `og:image` / `twitter:image` meta on any page | `og:image` pointing to an existing banner (e.g. `/vitamax-images/banner-1.png`) | Links shared on WhatsApp/Facebook/iMessage render with no preview image — hurts the exact word-of-mouth/sharing the brand depends on. |

---

## D. Path consistency / robustness (works now, fragile)

| # | Issue | Current paths | Expected (normalized) | Why it matters |
|---|-------|---------------|------------------------|----------------|
| D1 | Mixed absolute vs relative image paths on `shop.html` | `vitamax-images/mix-carton-1.png`, `vitamax-images/pure-honey-1.png`, `vitamax-images/queens-honey-1.png`, `vitamax-images/royal-gold-1.png` (relative — no leading slash) | `/vitamax-images/...` (root-absolute, matching every other reference site-wide) | These 4 load **only** when the page is served from the exact site root. If `shop.html` is ever opened from a subpath or via `file://`, these four images break while the rest still load. Inconsistent and fragile. |

---

## E. Emoji & placeholder-as-asset usage (by design — flagged per request #6)

Not "broken," but these are emoji/placeholders standing in for real visual assets (relevant to the premium-brand goal; **not** part of this infra fix unless you want them included):

| Location | Usage |
|----------|-------|
| `product-wholesale.html` thumb-row | 📦 🍯 🌍 used as **actual product thumbnails** (clickable gallery) |
| Homepage & shop | Category filters (For Him 💪 / For Her 🌸 / Bundles 🎁 / Honey Herbs 🌿 / Wholesale 📦), badges (🔥 👑 💑 ✨ ⚡ 🍵), wishlist hearts (🤍 ×12), CTA glyphs (🍯 🛒 📱 🛍) |
| All product cards | Emoji used in related-product cards and CTAs |

*(The earlier `emoji-ph`/`img-placeholder` hits on the 9 standard product pages are **CSS class definitions only**, not live placeholders — those pages use real product photos. Only the two wholesale locations in section B actually render emoji in place of a product image.)*

---

## F. Inventory reconciliation

- **Files on disk:** 55 in `vitamax-images/`.
- **Referenced & OK:** 45 distinct image files load correctly.
- **Referenced but missing:** 3 paths (`gladiator-3.png` live; `product-wholesale.jpg` & `wholesale.jpg` commented/emoji).
- **On disk but never referenced (orphans):** the 6 `Gemini_Generated_Image_*.png` source files (incl. duplicate "Copy" files), `black-3.png`/`black-4.png` and several `-3/-4` variants are referenced via gallery onclick handlers — verified present. The Gemini originals appear to be unused raw exports (~35MB) safe to archive.
- **Logo:** inline base64 in nav/footer (not a file path) — loads, but heavy; not broken.

---

## Proposed Fixes (code-only, no redesign)

All fixes use **existing files** or standard `<head>` tags — nothing requires new artwork except optionally a dedicated favicon.

| Ref | Fix | Effort | Risk |
|-----|-----|--------|------|
| **A1** | Remove the broken `gladiator-3.png` thumbnail from `product-detox-honey.html` (gallery becomes 1/2/4). *Alternative:* if a 3rd Gladiator photo exists elsewhere, drop it in as `gladiator-3.png`. | 🟢 | none |
| **B1** | In `shop.html`, replace the 📦 placeholder with `<img src="/vitamax-images/Vitamax-wholesale.png" alt="VitaMAX Wholesale Carton">` | 🟢 | none |
| **B2** | In `product-wholesale.html`, replace the main 📦 emoji with `<img src="/vitamax-images/Vitamax-wholesale.png" ...>` and swap the emoji thumbnails for `mix-carton-1.png` / `mix-carton-2.png` | 🟢 | low |
| **C1** | Add `<link rel="icon" href="/vitamax-images/favicon.png">` to every page's `<head>` (needs one small favicon asset — can be generated from the logo) | 🟢 | none |
| **C2** | Add `og:image` + `twitter:image` meta pointing to `/vitamax-images/banner-1.png` on key pages | 🟢 | none |
| **D1** | Normalize the 4 relative `shop.html` paths to leading-slash `/vitamax-images/...` | 🟢 | none |

**Verification plan (run before any further UI work):**
1. Serve `vitamax-site/` locally (`python3 -m http.server`).
2. Re-run the automated reference-vs-disk checker — target **zero MISSING**.
3. Crawl each page's HTTP requests for any `404` (images, favicon).
4. Manual spot-check: detox gallery, both wholesale locations, browser-tab favicon, and a shared-link preview.

---

## Recommended scope for the fix pass

**In scope (asset infrastructure):** A1, B1, B2, C1, C2, D1 — all code-only, all using existing assets (except an optional favicon file).
**Out of scope (deferred — that's redesign):** replacing emoji icons/badges/filters with a custom icon set (section E) is a Phase-3 visual-system task, not infrastructure.
