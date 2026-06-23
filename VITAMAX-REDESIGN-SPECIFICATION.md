# VitaMAX Redesign Specification

> Translates the **VitaMAX Master Brand Blueprint** into concrete, buildable website changes.
> Companion document — read the Blueprint first for the *why behind the why*. This is the *what to build*.

**Scope:** all 19 pages of `vitamax-site/`.
**Legend — Difficulty:** 🟢 Easy (hours) · 🟡 Medium (days) · 🔴 Hard (weeks).
**Legend — Impact:** ★ low · ★★ moderate · ★★★ high · ★★★★ critical/revenue-blocking.

**Sequencing rule (from Blueprint §Appendix):** Trust-hygiene fixes first (they block everything), then visual system, then story/architecture. Each section below is ordered to support that.

---

## 0. Cross-Cutting Foundations (do before page work)

These touch every page and unblock the rest.

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 0.1 | **Replace all `wa.me/601XXXXXXXX` and `+60 1X-XXXXXXX` placeholders** with the real number (`60177809767`) on `checkout`, `privacy-policy`, `refund-policy`, `product-queens-honey`, `thank-you` | Checkout's primary order channel is currently a dead link — active revenue leak (Blueprint §9 Layer 1) | ★★★★ | 🟢 |
| 0.2 | **Branded support email** (`support@vitamax.my`) replacing `orgapulse.my@gmail.com` everywhere | A Gmail address breaks premium credibility instantly | ★★★ | 🟢 |
| 0.3 | **Extract shared CSS** into one stylesheet; create design tokens as the single source of truth | 18 pages duplicate ~20–29KB inline CSS; impossible to restyle consistently otherwise | ★★ | 🟡 |
| 0.4 | **Image pipeline:** convert all PNG→WebP/AVIF, ≤200KB, add `loading="lazy"` + explicit width/height | 183MB payload, 8MB single images, zero lazy-loading = broken mobile speed & CLS (Blueprint §10) | ★★★★ | 🟡 |
| 0.5 | **Reconcile pricing + product count** across home/shop/product (ICE ENERGY shows $109 vs $149; "14 products" vs 10 shown) | Price mismatches mid-funnel are hard trust-breakers | ★★★ | 🟢 |

---

## 1. Homepage

**Strategic goal:** Lead with the **"Together" couples ESP** (Blueprint §5, §8), answer the Reconnecting Couple's four objections above the fold, and shed the AI-template look.

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 1.1 | **Add a real, live `<h1>`** (currently the hero headline is baked into an image). Master line: *"Crafted for the couples who refuse to slow down."* | No H1 = invisible to SEO, screen readers, and AI search; the brand's biggest page has no machine-readable headline | ★★★★ | 🟢 |
| 1.2 | **Rebuild hero around "Together."** Editorial couple-focused image (not a Gemini render), one primary CTA ("Shop Together Bundles"), one secondary ("Explore the Range") | The couples angle is the only true differentiator and is currently buried below the product grid | ★★★★ | 🟡 |
| 1.3 | **Split the mashed CTA** (`🍯 Shop All Products → 📱 WhatsApp Us`) into two distinct buttons | One button can't carry two actions; dilutes the primary conversion path | ★★★ | 🟢 |
| 1.4 | **Above-the-fold trust strip:** Halal certified (with body) · 30-day guarantee · discreet shipping · verified reviews | Answers "scam? / discreet? / worth it?" before scroll (Blueprint §4) | ★★★ | 🟢 |
| 1.5 | **Reorder sections:** Hero(Together) → Trust strip → Hero product (Couples Bundle) → How it works/ritual → Why VitaMAX (differentiators) → Verified reviews → Story teaser → FAQ → CTA | Current order has 7 same-weight H2s with no narrative; needs an emotional arc to the sale | ★★★ | 🟡 |
| 1.6 | **Replace emoji category filters** (For Him 💪 / For Her 🌸) with custom line icons + line names from product hierarchy | Emoji = the AI-template tell (Blueprint §10) | ★★ | 🟡 |
| 1.7 | **Make social-proof numbers verifiable** ("10,000+ customers", "4.9 from 1,842") via a real review widget, or remove | Unverifiable stats discredit the real ones (Blueprint §9 rule) | ★★★ | 🟡 |

---

## 2. Navigation

**Strategic goal:** One consistent, premium nav reflecting the product hierarchy; separate the consumer world from wholesale.

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 2.1 | **Unify the nav across all templates** (home, shop, product currently differ in items and order) | Inconsistency reads as unfinished/untrustworthy | ★★★ | 🟢 |
| 2.2 | **Restructure to the hierarchy:** Shop ▸ (For Him / For Her / Together / Pure / Herbal) · Our Story · Reviews · Wholesale · Help | Mirrors Blueprint §8; helps users and SEO | ★★ | 🟡 |
| 2.3 | **Add a Story link** (new brand-story page, §6) | Premium brands lead with story; currently absent | ★★ | 🟢 |
| 2.4 | **Visually demote Wholesale** to a quieter slot (it's B2B) and keep its pages stylistically separate | Stops B2B pricing/voice from contaminating the consumer premium feel | ★★ | 🟢 |
| 2.5 | **Add breadcrumbs site-wide** (only shop has a partial one) | Orientation + SEO | ★ | 🟢 |
| 2.6 | **Persistent, accurate cart count** + consistent cart entry across pages | Current cart UI/labels differ per template | ★★ | 🟡 |

---

## 3. Product Pages

**Strategic goal:** Earn the premium price through specificity and transparency (Blueprint §6 reason-to-believe), and make each page unique.

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 3.1 | **De-duplicate testimonials.** Every product currently shares the identical "I've tried everything… by week 2" quote. Unique, verified, per-product reviews via a review platform | The single clearest authenticity failure; savvy buyers spot it instantly | ★★★★ | 🟡 |
| 3.2 | **Real ingredient transparency:** actual ingredient list **with dosages**, sourcing origin per SKU (replace "Natural ingredients, clearly listed") | Specificity *is* the premium signal for a $139 product (Blueprint §3, §6) | ★★★ | 🟡 |
| 3.3 | **Rename under VitaMAX architecture** (lead with VitaMAX line name; legacy names like Etumax/Gladiator become sub-descriptors or retire) | Customer must buy "a VitaMAX," not a mystery reseller SKU (Blueprint §8) | ★★★ | 🟡 |
| 3.4 | **Real product photography system:** macro (honey texture), in-hand/scale, lifestyle, packaging detail — retire flat renders | Flat PNG renders look dropship; premium needs editorial imagery (§10) | ★★★ | 🔴 |
| 3.5 | **Honest "What you'll feel" + timeline** ("most feel a difference within two weeks") replacing vague "real outcomes reported consistently" | Plain, specific, on-voice; sets expectations and reduces refunds | ★★ | 🟢 |
| 3.6 | **Clarify variant pricing + "Most Popular"/"Best Value"** with honest savings math; consistent across home/shop | Removes confusion and price mismatch (0.5) | ★★★ | 🟡 |
| 3.7 | **Sticky add-to-cart** with price + selected variant on mobile; reassurance microcopy (guarantee, discreet) near the button | Reduces friction at the decision moment | ★★ | 🟡 |
| 3.8 | **Cross-sell to the Couples Bundle** from single SKUs ("Get both — save as a couple") | Pushes toward the hero, higher-AOV product (§8) | ★★ | 🟢 |

---

## 4. Wholesale Page

**Strategic goal:** Serve the B2B reseller avatar (Blueprint §4 tertiary) with a credible, margin-and-reliability pitch — kept visually distinct from the consumer brand.

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 4.1 | **B2B-specific value prop:** margins, MOQ, lead times, reliability, recognizable brand — not consumer emotional copy | B2B buyers decide on economics, not "feel like a younger couple" | ★★★ | 🟡 |
| 4.2 | **Wholesale pricing tiers / margin table** + clear "request a quote" flow (real WhatsApp/email, not placeholder) | Removes ambiguity; current wholesale variants ("Custom — Inquire") are vague | ★★★ | 🟡 |
| 4.3 | **Reseller trust block:** business registration, fulfillment reliability, sample/onboarding process | B2B needs different proof than consumers | ★★ | 🟡 |
| 4.4 | **Consolidate `wholesale.html` + `product-wholesale.html`** (currently two overlapping pages) into one clear path | Two competing wholesale pages confuse and dilute | ★★ | 🟡 |
| 4.5 | **Distinct but on-brand styling** (quieter, more corporate) so it doesn't undercut consumer premium feel | Keeps the two audiences cleanly separated (§2.4) | ★ | 🟢 |

---

## 5. Trust Section

**Strategic goal:** Build the 4-layer trust framework (Blueprint §9) — VitaMAX's lowest score and highest-leverage fix.

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 5.1 | **Footer credibility block:** registered business name, address, country, branded email, real WhatsApp | Anonymous brand selling $139 USD products = scam signal (§9 Layer 1) | ★★★★ | 🟢 |
| 5.2 | **Integrate a third-party review platform** (Judge.me / Trustpilot / Loox) with verified + photo reviews | Replaces fake/duplicate quotes with provable proof (§9 Layer 2) | ★★★★ | 🟡 |
| 5.3 | **Show certifications properly:** Halal cert number + issuer, lab/ingredient test docs, sourcing documentation | Claims must be backed or removed (§9 rule) | ★★★ | 🟡 |
| 5.4 | **Guarantee block signed by the founder** (30-day money-back, plain language) + discreet-packaging shown, not just told | Reassurance layer; founder signature adds authority (§9 Layer 3) | ★★★ | 🟢 |
| 5.5 | **Region-specific honest shipping windows** (the brand's own "they warned me upfront" review proves honesty converts) | Sets expectations, reduces disputes/chargebacks | ★★ | 🟢 |
| 5.6 | **Reframe checkout trust:** lead with Stripe/PayPal/Apple-Google Pay; reposition "we'll WhatsApp a payment link" as optional concierge, not default | "DM us for a payment link" reads as a scam to US/EU buyers (§7 audit) | ★★★★ | 🟡 |

---

## 6. Brand Story Section

**Strategic goal:** Give the brand a human origin (Blueprint §6, §7) — a new "Our Story" page plus story touchpoints.

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 6.1 | **Create an "Our Story" page** following the 6-beat arc (tradition → personal moment → the problem → the decision → the standard → mission) | Premium brands lead with story; none exists today | ★★★ | 🟡 |
| 6.2 | **Homepage story teaser** linking to the full page | Plants the narrative early in the emotional arc (§1.5) | ★★ | 🟢 |
| 6.3 | **Founder presence:** name, photo, signature on guarantee; short founder note | Faces build trust; "signed by a real person" beats anonymity (§7) | ★★★ | 🟡 |
| 6.4 | **Sourcing/heritage block** with real rainforest/beekeeping imagery (not AI renders) | Substantiates "Malaysian rainforest" claim with proof (§9 Layer 4) | ★★ | 🔴 |
| 6.5 | **Authenticity guardrail:** only ship facts that are true; build the farm/founder reality or use only what's genuine | One exposed invented claim collapses all trust (§7 rule) | ★★★★ | — (policy) |

---

## 7. Mobile UX

**Strategic goal:** Redesign for mobile, not shrink desktop (Blueprint §10; current mobile score 3/10).

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 7.1 | **Ship the image pipeline (0.4) first** — it's the dominant mobile problem | 183MB un-lazy-loaded images destroy LCP on mobile data | ★★★★ | 🟡 |
| 7.2 | **Fluid type via `clamp()`** (currently only 2 uses); raise body to 17–18px | Tiny 13px fixed type hurts thumb readability; type doesn't scale | ★★★ | 🟡 |
| 7.3 | **Add intermediate breakpoints** (currently only 1024/768/480) and genuinely re-lay-out key sections | Layout is desktop-shrunk, not designed | ★★ | 🟡 |
| 7.4 | **Mobile sticky add-to-cart + bottom action bar** with variant + price | Keeps the primary action thumb-reachable | ★★★ | 🟡 |
| 7.5 | **Tap targets ≥44px, single-action screens** (Blueprint §10 principle 3) | Reduces misfires, raises mobile conversion | ★★ | 🟢 |
| 7.6 | **Test full mobile funnel** hero → product → cart → checkout for the broken WhatsApp/payment path (ties to 0.1, 5.6) | The conversion path currently dead-ends on placeholder links | ★★★★ | 🟢 |

---

## 8. Visual Design System

**Strategic goal:** Replace the "AI-generated wellness template" with a distinctive, restrained, premium system (Blueprint §10). This underpins every other section, so define tokens early (0.3).

| # | Change | Why it matters | Impact | Difficulty |
|---|--------|----------------|--------|------------|
| 8.1 | **Replace the Playfair Display + Plus Jakarta Sans pairing** — the #1 AI tell. Choose a distinctive high-contrast display serif + a refined warm sans | Type pairing alone makes the site read as AI-generated | ★★★ | 🟡 |
| 8.2 | **Evolve the palette:** deeper amber/honey-gold + deep warm near-black + soft cream + one restrained accent; gold used as thin foil-like accent only | Current orange/gold flat fills read cheap; restraint signals luxury | ★★★ | 🟡 |
| 8.3 | **Proper tonal/elevation system** (current single shadow + 14px radius everywhere is flat) | Depth and hierarchy distinguish premium from template | ★★ | 🟡 |
| 8.4 | **Custom line-icon set** replacing all emoji | Emoji-as-iconography is the template giveaway (§10) | ★★ | 🟡 |
| 8.5 | **Disciplined spacing scale + one card component**; generous whitespace | "Whitespace is a feature" (§10 principle 2); fixes same-y rhythm | ★★ | 🟡 |
| 8.6 | **Retire all `Gemini_Generated_Image_*` assets**; adopt the editorial photography system | AI renders are a literal AI-generated signal (§1 audit) | ★★★ | 🔴 |
| 8.7 | **Subtle, slow motion** (gentle fades; no bouncy/hype animation) | Calm motion reads "expensive" (§10) | ★ | 🟡 |
| 8.8 | **Codify everything in the shared stylesheet/tokens (0.3)** as the design system of record | Without a token system, consistency can't hold across 19 pages | ★★★ | 🟡 |

---

## Build Roadmap (recommended order)

**Phase 1 — Stop the bleeding (🟢, days):** 0.1, 0.2, 0.5, 1.1, 1.3, 5.1, 7.6 — fix broken links, Gmail, pricing, missing H1, footer credibility, dead funnel.

**Phase 2 — Trust + performance (🟡, 1–3 weeks):** 0.3, 0.4, 3.1, 5.2, 5.3, 5.6, 7.1, 7.2 — image pipeline, review platform, certifications, checkout trust, real testimonials.

**Phase 3 — Brand system (🟡/🔴, 3–6 weeks):** 8.1–8.5, 8.8, 1.2, 1.5, 1.6, 2.x — new type/palette/tokens, "Together" homepage, unified nav.

**Phase 4 — Story + product depth (🔴, 4–8 weeks):** 6.x, 3.2–3.4, 3.7, 4.x, 8.6 — Our Story page, ingredient transparency, product renaming, editorial photography, wholesale rebuild.

**Filter for every task (Blueprint §Appendix):** Does it serve the Reconnecting Couple? Ladder to the ESP? Build or break trust? Pass the premium/voice tests?
