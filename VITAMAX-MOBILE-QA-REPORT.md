# VitaMAX Mobile-First QA Report

> Complete mobile-readiness audit of the entire site, performed **before** merging the homepage redesign.
> Audited state: `master` + the proposed Phase 1 homepage (hero H1, free-shipping banner, trust strip, SVG logo) — i.e. the current preview content.

**Method.** Two complementary techniques:
1. **Code-level analysis** of all 19 pages — the reliable signal for horizontal scroll, viewport config, breakpoints, overflow guards, and layout collapse.
2. **Rendered screenshots** at real device widths (375 / 768 px shown; 412 & 430 px behave identically to 375 since all share the same `≤480px` and `≤768px` breakpoints). Rendering engine is QtWebKit (wkhtmltoimage) — layout/scroll is trustworthy; minor anti-aliasing/font quirks in shots are renderer artifacts, not site bugs.

**Widths required & how they map to the site's breakpoints**

| Device | Width | Active breakpoint | Notes |
|--------|-------|-------------------|-------|
| iPhone | 375px | `≤480` + `≤768` | primary phone target |
| Android | 412px | `≤480` + `≤768` | same rules as 375 |
| Large iPhone | 430px | `≤480` + `≤768` | same rules as 375 |
| Tablet | 768px | `≤768` | 2-col / intermediate |

---

## Verdict (read this first)

**The site is NOT yet "fully mobile-ready" — but it is close.** The **core commerce flow (product → cart → checkout) is mobile-ready and good.** There is **one High-severity issue** that must be fixed before the homepage redesign is merged, plus one Medium accessibility issue and minor items.

- 🔴 **1 High** — homepage hero banner text is cropped on phones.
- 🟠 **2 Medium** — pinch-zoom disabled site-wide; top promo bar cramped on small phones.
- 🟡 **3 Low** — cart overflow guard, logo inconsistency on cart, promo-bar negative margins.

**Recommendation:** fix H1 (hero banner) and M1 (zoom) before merge. Everything else can follow.

---

## Issues Found

### 🔴 HIGH

**H1 — Homepage hero banner is cropped on phones; baked-in text cut off**
- **Where:** `index.html` hero slider (`banner-1.png`, `banner-2.png`), phones ≤ ~480px.
- **Observed:** The hero banners are wide **landscape images with text baked into the image**. On a 375px phone the image is scaled/cropped to fill, so the right side is cut: "FEEL MORE CONNE**[CTED]**", the sub-line "PREMIUM HONEY **[CRAFTED FOR VITALITY,]** WELLNESS & MEA**[NINGFUL…]**", and the **"SHOP NOW" button is partially clipped**. At 768px (tablet) the same banner renders fully and reads correctly — so this is phone-specific.
- **Impact:** Breaks the hero's emotional message and readability on the most common device class; the primary visual promise is unreadable; a CTA is clipped. Directly fails the "banner text readable / no cropped text / hero preserves emotional impact" requirements.
- **Screenshot:** `idx-375-top.png` (cropped banner) vs `idx-768-top.png` (correct).
- **Recommended fix:** Stop relying on text-in-image for the hero. Either (a) provide a **mobile-specific portrait/square banner crop** via `<picture>`/`srcset` with the text composed for phone aspect, or (b) **overlay live HTML text** (headline + SHOP NOW button) on a plain honey background image so it reflows. Option (b) also helps SEO/accessibility and matches the new live `<h1>` approach. Until fixed, at minimum set the banner to show its center safely and ensure the button isn't clipped.

### 🟠 MEDIUM

**M1 — Pinch-zoom disabled on 14 of 19 pages (accessibility)**
- **Where:** viewport meta `width=device-width, initial-scale=1.0, maximum-scale=1.0` on `index`, `shop`, all product pages, `wholesale`, `product-wholesale`, `cart`, `checkout`. (`maximum-scale=1.0` prevents users from zooming.)
- **Impact:** Fails WCAG 1.4.4 (Resize Text); users who need to zoom in (low vision) can't. Common, easily-overlooked defect.
- **Recommended fix:** Remove `maximum-scale=1.0` (and any `user-scalable=no`) from the viewport meta on all pages → `content="width=device-width, initial-scale=1.0"`.

**M2 — Top promo bar is cramped on small phones**
- **Where:** `.hero-promo-banner` / top announcement bar, 375px.
- **Observed:** Three messages ("FREE Shipping over $60 · SAVE 25% on 3-Packs today · Discreet packaging on every order") wrap into a dense 2-line block on narrow phones.
- **Impact:** Busy/cluttered first impression; not broken, but not premium. Low-Medium.
- **Recommended fix:** On ≤480px, rotate/condense to a single message (or a simple carousel), e.g. just "Free shipping over $60 · Discreet packaging."

### 🟡 LOW

**L1 — `cart.html` lacks an `overflow-x:hidden` guard**
- The other key pages (`index`, `shop`, product, `checkout`, `wholesale`) set `overflow-x:hidden` on `body`; `cart.html` does not. Its layout **does** collapse correctly to one column ≤768px, so no scroll was observed — but add the guard for defense-in-depth.

**L2 — Logo inconsistency on the cart page**
- Most pages now use the new **SVG wordmark** ("VitaMAX.MY", mixed case). The cart header renders a different **text logo** ("VITAMAX.MY", all-caps styling). Minor brand inconsistency; align cart (and verify thank-you) to the SVG wordmark.

**L3 — `.hero-promo-banner` uses negative horizontal margins (`margin:-32px … -32px`)**
- A full-bleed technique that *could* cause horizontal scroll, but it's currently contained by the page's `overflow-x:hidden`. Safe today; worth replacing with a cleaner full-bleed pattern during the redesign.

---

## Verification Checklist Results

| Check | 375 / 412 / 430 (phone) | 768 (tablet) | Notes |
|-------|:--:|:--:|-------|
| Hero banners display correctly | ⚠️ | ✅ | Phone: cropped (H1). Tablet: OK. |
| Banner text readable | 🔴 | ✅ | Phone: baked text cut (H1). |
| Product images display | ✅ | ✅ | Crisp, correct. |
| Product galleries function | ✅ | ✅ | Thumbnail row + main image OK. |
| Navigation menu works | ✅ | ✅ | Hamburger on main pages; cart/checkout/thank-you use minimal headers (by design). |
| Buttons fully visible/clickable | ⚠️ | ✅ | Only the in-banner "SHOP NOW" is clipped on phone (part of H1). All HTML buttons OK & full-width. |
| Trust strip displays | ✅ | ✅ | 2×2 with icons on phone; row on tablet. |
| No horizontal scrolling | ✅ | ✅ | Overflow guards + responsive grids; no fixed widths ≥380px. |
| No overlapping elements | ✅ | ✅ | None observed. |
| No cropped text | 🔴 | ✅ | Hero banner only (H1). |
| No cropped images | ⚠️ | ✅ | Hero banner (H1); all product imagery fine. |
| No layout shift (CLS) | ⚠️ | ⚠️ | Images lack width/height attrs + no lazy-load (heavy PNGs) → shift/slow on mobile data. Pre-existing; separate perf task. |
| No broken icons | ✅ | ✅ | SVG icons (trust strip, shipping, nav) render. |
| No missing assets | ✅ | ✅ | 0 missing (per asset audit); favicon + logos load. |
| Forms work correctly | ✅ | ✅ | Checkout: full-width labeled fields, country select, single column. |
| Checkout/cart usable | ✅ | ✅ | Cart collapses to 1-col (summary on top); checkout step flow clean. |

Legend: ✅ pass · ⚠️ minor/conditional · 🔴 fail.

---

## Visual Consistency & Brand (mobile vs desktop)

| Criterion | Result |
|-----------|--------|
| Same brand message on mobile | ✅ The live `<h1>` ("Crafted for the couples who refuse to slow down"), free-shipping banner, and trust strip all carry the desktop message to mobile. |
| Hero preserves emotional impact | ⚠️ The **text intro** preserves it, but the **banner image** undercuts it on phones via cropping (H1). Fixing H1 restores full impact. |
| Banners remain prominent/professional | ⚠️ Free-shipping banner & trust strip look great on phone; the main hero banner needs the H1 fix. |
| Product presentation premium | ✅ Product pages are clean and premium on mobile (image, gallery, rating, buy box). |
| Trust signals above the fold | ✅ Trust strip (Halal / Guarantee / Discreet / Rating) and the free-shipping banner sit high on phones. |

---

## Page-by-Page Summary

| Page(s) | Phone result | Key note |
|---------|:--:|----------|
| **Homepage** | ⚠️ | Hero banner cropping (H1); everything else (intro, shipping banner, trust strip, products 3→2→1) is good. |
| **Product pages (×11)** | ✅ | Premium and clean; gallery + buy box work. Zoom-disabled (M1) applies. |
| **Shop** | ✅ | Category tabs + product grid responsive. |
| **Wholesale** | ✅ | Dark hero is **live text** (reads perfectly on mobile — the model the homepage hero should follow); pills wrap; CTAs full-width. |
| **Cart** | ✅ | Collapses to 1-col, summary on top, CTA full-width; logo inconsistency (L2), missing overflow guard (L1). |
| **Checkout** | ✅ | Single-column form, labeled fields, progress steps; mobile-ready. |
| **Policy / thank-you** | ✅ | Simple, readable; these pages already allow zoom (no M1). |

---

## Recommended Fix Order (before/after merge)

**Before merging the homepage redesign:**
1. **H1** — make the hero banner mobile-safe (mobile crop via `<picture>`/`srcset`, or overlay live text + button on a plain background).
2. **M1** — remove `maximum-scale=1.0` from the viewport meta site-wide.

**Soon after (non-blocking):**
3. **M2** — condense the top promo bar on ≤480px.
4. **L1/L2/L3** — add `overflow-x:hidden` to cart; unify cart logo to the SVG wordmark; replace promo-bar negative margins.

**Separate performance track (pre-launch):**
5. Add `width`/`height` + `loading="lazy"` and convert the heavy PNGs to WebP to remove layout shift and speed up mobile (noted in earlier audits).

---

## Conclusion

The commerce core (product, cart, checkout, wholesale) is **mobile-ready and professional**. The homepage's new text elements (H1, shipping banner, trust strip, SVG logo) also render well on phones. **The one blocker to declaring full mobile-readiness is H1 — the cropped hero banner on phones** — with M1 (zoom) as a close second. Fix those two and the site can be confirmed mobile-ready for the homepage merge.
