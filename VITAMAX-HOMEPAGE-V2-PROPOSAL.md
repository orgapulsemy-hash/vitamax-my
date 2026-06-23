# VitaMAX Homepage V2 — Mockup Proposal

> A visual + narrative proposal comparing the **current homepage** with the **proposed Homepage V2**.
> Wireframes are ASCII mockups (layout intent, not pixel design). No website files have been edited.
> Grounded in the real `index.html` structure and the Master Brand Blueprint + Redesign Specification.

**Phase tags in this doc:**
`[P1]` = Phase 1 (build now — quick, high-impact) · `[P2]` = trust/performance · `[P3]` = brand/visual system.

---

## 1. Side-by-Side Layout (Before → After)

### CURRENT homepage (V1)
```
┌───────────────────────────────────────────────┐
│ NAV: Shop Benefits Reviews FAQ Wholesale …  🛒 │
├───────────────────────────────────────────────┤
│                                                │
│   [ IMAGE SLIDER — banner-1 / banner-2 ]       │  ← headline is BAKED INTO
│   (no real text headline / no <h1>)            │     the image. Page has 0 <h1>.
│                                                │
├───────────────────────────────────────────────┤
│ OUR PRODUCTS · "Shop Royal Honey"              │
│ [Filter chips: For Him 💪 For Her 🌸 …]        │  ← emoji as icons
│ [ 10 product cards in a grid ]                 │
│ "View All 14 Products →"   (10 vs 14 mismatch) │
├───────────────────────────────────────────────┤
│ "Good for Him. Good for Her. Better Together." │  ← the couples story is
│  (couples positioning — BURIED here)           │     hidden below products
├───────────────────────────────────────────────┤
│ "The VitaMAX Promise" — 5 testimonials         │  ← unverified, no names
├───────────────────────────────────────────────┤
│ FAQ                                            │
├───────────────────────────────────────────────┤
│ FINAL CTA "Your Best Energy Is Still Ahead"    │
│ [🍯 Shop All Products]  [📱 WhatsApp Us]        │  ← Shop button tag is
│ Free shipping · 30-day guarantee · Discreet    │     malformed (</button>)
├───────────────────────────────────────────────┤
│ FOOTER                                         │
└───────────────────────────────────────────────┘
 Sticky: [🛒 Shop Now →]
```

### PROPOSED homepage (V2)
```
┌───────────────────────────────────────────────┐
│ NAV: Shop ▸  Our Story  Reviews  Wholesale  🛒 │  [P3] unified + Story link
├───────────────────────────────────────────────┤
│  PREMIUM ROYAL HONEY · SINCE 2024              │  [P1] eyebrow
│                                                │
│  Crafted for the couples who                   │  [P1] ◄ REAL <h1>, live text
│  *refuse to slow down*                         │
│                                                │
│  Premium royal honey for energy, vitality &    │  [P1] supporting subhead
│  connection — together. Halal certified,       │
│  shipped discreetly worldwide.                 │
│                                                │
│   [ IMAGE SLIDER — banner-1 / banner-2 ]       │      (slider retained)
├───────────────────────────────────────────────┤
│ ✓ Halal Certified  ✓ 30-Day Guarantee          │  [P1] ◄ TRUST STRIP
│ ✓ Discreet Shipping  ✓ 4.9★ Rated              │
├───────────────────────────────────────────────┤
│ ★ FEATURED: VitaMAX Together (Couples Bundle)  │  [P3] hero product = the
│   "Feel like a younger couple again"           │       differentiator, lifted up
├───────────────────────────────────────────────┤
│ OUR PRODUCTS · "Shop Royal Honey"              │
│ [Filter chips with custom icons]               │  [P3] emoji → line icons
│ [ product cards ]   "View All Products →"      │  [P1] fix 10/14 count
├───────────────────────────────────────────────┤
│ OUR STORY (teaser) → full story page           │  [P2] brand origin
├───────────────────────────────────────────────┤
│ VERIFIED REVIEWS (named, photo, per-product)   │  [P2] real review widget
├───────────────────────────────────────────────┤
│ FAQ                                            │
├───────────────────────────────────────────────┤
│ FINAL CTA "Your Best Energy Is Still Ahead"    │
│ [🍯 Shop All Products]  [📱 WhatsApp Us]        │  [P1] fix malformed tag
│ Free shipping · 30-day guarantee · Discreet    │
├───────────────────────────────────────────────┤
│ FOOTER: business name · address · support@…    │  [P1/P2] credibility block
└───────────────────────────────────────────────┘
 Sticky: [🛒 Shop Now →]
```

**What Phase 1 actually ships now:** the `[P1]` items only — real `<h1>`, trust strip, CTA tag fix (and the small "14 products" count fix). The `[P2]`/`[P3]` blocks are shown so you can see where V2 is heading, but they are later phases.

---

## 2. The Before → After User Experience

**Current journey (V1):**
A visitor lands and sees a sliding picture with no readable headline. They don't immediately know what the brand promises or whether it's trustworthy. To find reassurance (guarantee, Halal, discreet) they must scroll past ten products. The couples angle — the one thing that makes VitaMAX different — is hidden halfway down. By the time trust signals appear, a skeptical buyer has often already left.

**Proposed journey (V2):**
A visitor immediately reads a confident headline that names exactly who it's for — *couples who refuse to slow down* — and a one-line promise of energy, vitality, and connection. A beat below, a trust strip answers their three fears (legit? discreet? worth it?) before they scroll. The picture slider still sets the mood, but it now supports a message instead of carrying it alone. The path from "what is this?" to "I trust this, show me products" happens in the first screen, not the fourth.

---

## 3. Highlighted Changes

### 🔶 New Headline
| | |
|---|---|
| **Before** | Headline baked into a banner image; page has **no `<h1>`** at all. |
| **After** | Live text headline: **"Crafted for the couples who refuse to slow down."** |
| **Why it matters** | Readable by Google, screen readers, and AI search; states the brand's emotional promise in its own voice; frames the whole page. |
| **Phase** | `[P1]` — ships now |

### 🔶 Trust Strip
| | |
|---|---|
| **Before** | Reassurances scattered far down the page; nothing above the fold. |
| **After** | A slim band under the hero: **Halal Certified · 30-Day Guarantee · Discreet Shipping · 4.9★ Rated.** |
| **Why it matters** | Answers a nervous buyer's fears at the exact moment doubt appears, reducing early bounce. |
| **Phase** | `[P1]` ships now; verifying the claims (review widget, visible certificate) is `[P2]`. |

### 🔶 CTA Improvements
| | |
|---|---|
| **Before** | Bottom "Shop" button has malformed HTML (`<a>…</button>`) — can misbehave on some browsers. |
| **After** | Tag corrected; button always behaves. (Visual look unchanged in P1.) |
| **Later** | `[P3]` restyle CTAs in the new visual system; replace emoji with refined icons. |
| **Phase** | `[P1]` fix now |

### 🔶 Brand Positioning Changes
| | |
|---|---|
| **Before** | Generic "Premium honey supplements for men and women." Couples angle buried below products. Reads as a reseller catalogue. |
| **After** | Positioning leads with the **"Together / couples"** ESP from the first screen; the Couples Bundle becomes the featured hero product `[P3]`; story and verified reviews give it substance `[P2]`. |
| **Why it matters** | "Built for couples, not secrecy" is VitaMAX's only true differentiator (Blueprint §6). V2 makes it the spine of the page instead of a hidden section. |
| **Phase** | Positioning *language* starts in `[P1]` (the new headline); full structural lift is `[P2]/[P3]`. |

---

## 4. Expected Impact Summary

| Change | Trust | Conversion | SEO/Discoverability | Effort |
|---|---|---|---|---|
| Real `<h1>` headline | ★★★ | ★★★ | ★★★★ | 🟢 |
| Trust strip | ★★★ | ★★★ | ★ | 🟢 |
| CTA tag fix | ★ | ★★ | — | 🟢 |
| Product-count fix | ★★ | ★ | — | 🟢 |
| *(later)* Featured couples bundle | ★★ | ★★★ | ★ | 🟡 |
| *(later)* Verified reviews | ★★★★ | ★★★ | ★★ | 🟡 |
| *(later)* New type/visual system | ★★★ | ★★ | ★ | 🟡 |

---

## 5. Risks & Guardrails

- **Phase 1 is additive and low-risk:** it adds a text intro and a trust band, fixes a broken tag, and corrects a number. It does not touch the slider mechanics, fonts, or layout system.
- **Honesty guardrail:** the trust strip surfaces existing claims (Halal, 4.9★) more prominently — so making them *provable* (Blueprint §9) becomes a higher-priority `[P2]` task. Do not invent new numbers.
- **Positioning shift is staged:** V2 changes the *message* first (P1 headline), then the *structure* (P2/P3). This avoids a risky big-bang redesign and lets each step be measured.

---

## 6. Recommended Next Step

Approve **Phase 1** (the four `[P1]` changes) for `index.html` only. Once live and reviewed, proceed to `[P2]` (verified reviews + footer credibility + image performance), then `[P3]` (visual system + featured couples bundle). Every step is filtered through: *Does it serve the Reconnecting Couple? Ladder to the ESP? Build trust? Pass the premium test?*
