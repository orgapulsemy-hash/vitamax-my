# VitaMAX Visual QA & Asset Verification Report

> Project documentation for the Priority-1 asset-infrastructure fix.
> Records the root causes, the fixes applied, how they were verified, and the final results.
> Verified against branch `master` after merge of the asset fixes (commit `e544cc7`).

**Note on screenshots:** automated screenshots could not be produced in the build environment — every headless-browser binary source (Playwright CDN, Google Chrome-for-Testing storage, chromelabs) is blocked by the network policy, and the only installable Chromium (`apt chromium-browser`) is a non-functional snap stub. Verification was therefore performed at the **code level**: parsing every asset reference in the HTML/CSS and cross-checking each against the files on disk, plus an HTTP crawl of every page and asset served locally.

---

## 1. Root Cause Analysis

The pre-fix site had broken and missing imagery for five distinct reasons:

| Issue | Symptom | Root cause |
|-------|---------|-----------|
| **Broken gallery thumbnail** | `gladiator-3.png` rendered a broken-image icon on the detox/Gladiator product page; clicking it loaded a 404 into the main viewer | The product gallery was built from a **fixed 4-thumbnail template** (`-1`/`-2`/`-3`/`-4`), but only **three** Gladiator photos were ever produced (`-1`, `-2`, `-4`). The template was never reconciled with the delivered asset set, leaving a reference to a file that never existed. |
| **Wholesale card placeholder (shop)** | The wholesale product card on `shop.html` showed a 📦 emoji instead of a photo | The intended image `product-wholesale.jpg` was **never produced**. A developer left an emoji plus an HTML comment (`<!-- REPLACE: ... -->`) as a stub that was never resolved. |
| **Wholesale gallery placeholder (product page)** | `product-wholesale.html` had an all-emoji gallery (📦 🍯 🌍) and no product photography | Same pattern — `wholesale.jpg` was never produced; a `<!-- TO ADD IMAGE: ... -->` stub remained. **Compounding cause:** real wholesale imagery *did* exist on disk under **different names** (`Vitamax-wholesale.png`, `mix-carton-1.png`, `mix-carton-2.png`), so a naming mismatch hid available assets. |
| **No favicon** | Every page silently 404'd `/favicon.ico`; blank browser-tab icon | A favicon was **never added** to the page `<head>` template. |
| **No social-share image** | Shared links rendered with no preview image | `og:image` / `twitter:image` meta were **never added** to the `<head>` template. |
| **Fragile relative paths** | 12 image refs in `wholesale.html` used `vitamax-images/…` (no leading slash) while the rest of the site used `/vitamax-images/…` | **Authoring drift** — `wholesale.html` was templated separately and used relative paths, which load only when served from the exact site root. |

**Underlying theme:** the failures were not corrupt files — they were **reference-vs-reality drift**: templated references that were never reconciled with the actual delivered asset set (missing files, naming mismatches, and inconsistent path conventions).

---

## 2. Asset Fixes Applied

All fixes are **code-only** and use **existing files** (no new artwork except a hand-authored SVG favicon).

| Ref | File(s) | Fix |
|-----|---------|-----|
| **A1** | `product-detox-honey.html` | Removed the broken `gladiator-3.png` thumbnail; gallery now shows views 1 / 2 / 4 (all existing files). |
| **B1** | `shop.html` | Replaced the 📦 `img-placeholder` span with `<img src="/vitamax-images/Vitamax-wholesale.png">`. |
| **B2** | `product-wholesale.html` | Replaced the emoji main image + emoji thumbnails with real photography (`Vitamax-wholesale.png` main; `mix-carton-1.png` / `mix-carton-2.png` thumbnails). Added a `setWholesaleImg()` handler and a `.thumb img` CSS rule so the gallery swaps real images. |
| **C1** | new `vitamax-images/favicon.svg` + all 19 pages | Authored a brand SVG favicon (dark tile, orange "V", gold honey-drop) and linked it via `<link rel="icon" type="image/svg+xml">` in every page `<head>`. |
| **C2** | all 19 pages | Added `og:type`, `og:site_name`, per-page `og:title` / `og:description` / `og:url`, `og:image` → `/vitamax-images/banner-1.png` (absolute, canonical domain `https://vitamax.my`), plus `twitter:card` / `twitter:image`. |
| **D1** | `wholesale.html` | Normalized all 12 `vitamax-images/…` references (in `src` and `data-img`) to root-absolute `/vitamax-images/…`, matching the rest of the site. |

---

## 3. Verification Methodology

1. **Inventory the disk** — enumerated all 55 files in `vitamax-site/vitamax-images/` (plus the new `favicon.svg`).
2. **Extract every reference** — parsed all 19 HTML pages for `src`, `href`, `data-img`, and CSS `url()` references; excluded inline base64 data-URIs and HTML-commented references.
3. **Cross-check reference → disk** — URL-decoded each path and tested for file existence; target **zero missing**.
4. **HTTP crawl** — served `vitamax-site/` locally (`python3 -m http.server`) and requested every page and every distinct image URL, asserting HTTP `200`.
5. **Placeholder sweep** — searched for any remaining live `img-placeholder` / `emoji-ph` elements and any reference to the three previously-dead files.
6. **Role mapping** — mapped each homepage asset to its function (banner / logo / product / step icon) to confirm no section renders empty.

---

## 4. Final Verification Results

| Check | Result |
|-------|--------|
| Local asset references resolved on disk | **90 / 90** (0 missing) |
| Distinct image URLs returning HTTP 200 | **49 / 49** (0 broken) |
| Pages returning HTTP 200 | **19 / 19** |
| Favicon linked on every page + serves 200 | **19 / 19**, `favicon.svg` → 200 |
| Previously-dead refs in live markup (`gladiator-3.png`, `wholesale.jpg`, `product-wholesale.jpg`) | **0** |
| Live emoji placeholders where real images now exist | **0** |

### Homepage asset roll-call
- **Banners:** `banner-1.png`, `banner-2.png` — both load.
- **Product grid (10):** `vip-1`, `black-1`, `lux-1`, `couples-1`, `chobe-1`, `Vitamax-wholesale`, `honeymax-1`, `gladiator-1`, `ice-1`, `etumax-1` — all load.
- **Step graphics:** `step-1/2/3.png` — all load.
- **Logos:** nav + footer (inline base64) — embedded, not broken (heavy; optimization noted as future work).
- **Icons:** 18 inline SVGs in the benefits/steps sections — vector, cannot 404.

### Known, intentional non-issues
- **Decorative emoji icons remain** (wishlist `🤍`, category filters `💪 🌸 🎁 🌿 📦`, badges `🔥 👑 💑 ✨ ⚡ 🍵`, CTA glyphs `🍯 🛒 📱 🛍`). Replacing these with a custom icon set is a deferred **Phase-3 redesign** task, not an asset-infrastructure defect.
- **Logos as inline base64** load correctly but are heavy; image-weight optimization is a separate performance task.
- One inert `img-placeholder` **CSS class definition** remains in `shop.html`'s stylesheet (no element uses it).

---

## 5. Conclusion

The asset infrastructure is **fully resolved** on `master`: every banner, product image, logo, step graphic, and wholesale image is referenced correctly and exists on disk; the favicon is universal; social-share meta is present; and no broken references, 404s, empty sections, or product-image emoji placeholders remain. The root cause — reference-vs-reality drift — has been closed by reconciling every reference with the actual asset set and standardizing path conventions.
