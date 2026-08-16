# WooCommerce Backend Update Guide — Carton Pricing (35% Off)

The site sells **cartons of 5 / 10 / 30 / 60** with **free delivery included**, a
**"+1 free box on first order"** promise, and a **free Royal Honey Handbook (PDF)**.
Prices were cut **35%** site-wide. The front-end reuses your existing WooCommerce
variation IDs, so **until you update the variations in WP Admin, customers will see the
new sale prices on the page but be charged the OLD prices at checkout.**

## ⚠️ Do this first — the prices changed

| Tier | Was | **Now (−35%)** | Per box | Customer saves |
|---|---|---|---|---|
| 5 Boxes — Starter | $495 | **$325** | $65 | $170 |
| 10 Boxes — Most Popular | $890 | **$580** | $58 | $310 |
| 30 Boxes — Best Value | $2,370 | **$1,530** | $51 | $840 |
| 60 Boxes — Max Savings | $4,140 | **$2,700** | $45 | $1,440 |
| Couples 5 / 10 / 30 / 60 sets | $990 / $1,160 / $3,090 / $5,400 | **$645 / $1,160 / $3,090 / $5,400** | $129 / $116 / $103 / $90 per set | $345 / $620 / $1,650 / $2,880 |

The struck-through "was" prices shown on the site are the prices you genuinely charged
until this change — keep it that way. If the sale ends, put the old prices back rather
than inventing a higher RRP.

**Wholesale channel is unchanged** (30/60/90 boxes at $79/$69/$59 per box). It was already
at reseller margin, so a further 35% cut would likely put it below cost. Tell me if you
want it discounted too.

## 1. Update product variations (WP Admin → Products → edit → Variations)

Rename each variation and set its new price. The 4th (old 6-pack) variation of each product should be
**disabled or deleted** — the site never sends it.

**Now includes the 5-box "Try It" entry tier.** The site's 5-box option reuses each product's
**4th variation** (the old "6-pack" that was previously disabled) — so instead of disabling it,
**re-price it to $325 ($645 for Couples), rename it "5 Boxes", and make sure it is enabled/purchasable.**
Every product's 4 variations are now all live.

Full ladder per box product: **5 = $325 ($65/box) · 10 = $580 ($58) · 30 = $1,530 ($51) · 60 = $2,700 ($45)**.

| Product (ID) | Variation ID | Name | Price |
|---|---|---|---|
| Royal Honey VIP Pack (76) | 80 | **5 Boxes** (was 6-pack) | **$325** |
| | 77 / 78 / 79 | 10 / 30 / 60 Boxes | $580 / $1,530 / $2,700 |
| Black Horse Vital Honey (81) | 85 | **5 Boxes** | **$325** |
| | 82 / 83 / 84 | 10 / 30 / 60 Boxes | $580 / $1,530 / $2,700 |
| Etumax Royal Honey For Her (86) | 90 | **5 Boxes** | **$325** |
| | 87 / 88 / 89 | 10 / 30 / 60 Boxes | $580 / $1,530 / $2,700 |
| Lux Honey For Her (91) | 134 | **5 Boxes** | **$325** |
| | 131 / 132 / 133 | 10 / 30 / 60 Boxes | $580 / $1,530 / $2,700 |
| VitaMAX Couples Bundle (93) | 97 | **5 Sets** | **$645** |
| | 94 / 95 / 96 | 10 / 30 / 60 Sets | $1,160 / $3,090 / $5,400 |
| ICE ENERGY (98) | 102 | **5 Boxes** | **$325** |
| | 99 / 100 / 101 | 10 / 30 / 60 Boxes | $580 / $1,530 / $2,700 |
| HoneyMax (103) | 107 | **5 Boxes** | **$325** |
| | 104 / 105 / 106 | 10 / 30 / 60 Boxes | $580 / $1,530 / $2,700 |
| Gladiator (108) | 112 | **5 Boxes** | **$325** |
| | 109 / 110 / 111 | 10 / 30 / 60 Boxes | $580 / $1,530 / $2,700 |
| Chobe Pure Honey (113) | 117 | **5 Jars** | **$325** |
| | 114 / 115 / 116 | 10 / 30 / 60 Jars | $580 / $1,530 / $2,700 |

*(Lux/Queen's is fully wired — the site sends variations 131/132/133/134 directly.)*

## 2. Shipping → free everywhere

WooCommerce → Settings → Shipping: in every zone, remove flat rates and leave a single
**Free shipping** method (no minimum). Checkout no longer charges shipping and the paid
Express DHL upsell has been removed.

## 3. Coupons

- **Delete or deactivate the `BOGO` coupon** — it is no longer advertised anywhere.
- Checkout still accepts optional codes: `VITAMAX10` ($10), `WELCOME15` ($15), `SAVE20` ($20),
  `ROYAL5` ($5). Keep them in Woo only if you want them honored, or ask Claude to remove them.

## 4. First-order free box (the hook — must be honored)

The site promises **+1 free box added automatically to every first order**. Operationally:
- Easiest: fulfillment SOP — if the customer email has no prior orders, add 1 box of the same
  product and a small welcome card.
- Optional automation: AutomateWoo (or a snippet) that tags first-time customers and prints
  "ADD +1 FREE BOX 🎁" on the packing slip.

## 4b. Free Royal Honey Handbook (PDF) — must be delivered

Every product page and the landing page now promise a **free Royal Honey Handbook**. The book
exists at **`/royal-honey-handbook.html`** — an 8-chapter guide (what royal honey is, the
ingredients, how to take it, building the ritual, week-by-week expectations, spotting fakes,
storage, FAQ). It has a "Save as PDF" button that prints cleanly.

To deliver it, pick one:
- **Simplest:** put the link `https://vitamax.my/royal-honey-handbook.html` in your order
  confirmation email. Costs nothing, works immediately.
- **Better:** open it, Print → Save as PDF, and attach the PDF to the confirmation email.
- Either way it must actually reach buyers — it's advertised as part of the offer.

## 4c. Subscribe & Save — every 3 weeks (manual for now)

Each product page now shows a **Subscribe & Save** block: an extra **10% off** the sale price
for a recurring 5-box delivery every 3 weeks (**$292 per delivery**, $58.40/box; couples
**$580**, $116/set). You confirmed there is **no WooCommerce Subscriptions plugin**, so the
button routes to **WhatsApp** with a pre-filled message rather than pretending to bill
automatically. Nothing silently fails.

To run it manually: take the customer's details on WhatsApp, then either send a payment link
every 3 weeks or take payment upfront for a fixed number of cycles. If volume grows, install
**WooCommerce Subscriptions** and I'll wire the button to real recurring checkout.

> ⚠️ **Worth reconsidering:** 5 boxes every 3 weeks is roughly double what one person consumes
> (5 boxes ≈ 60–120 sachets ≈ 2–4 months at one a day). Customers will over-accumulate and
> cancel. A 5-box cycle every **8–10 weeks**, or a **2-box** cycle every 3 weeks, matches real
> consumption far better and will retain much longer. Say the word and I'll change it.

## 5. Stripe backend

Card payments on checkout.html call `https://vitamax.my/create-payment-intent.php`.
Make sure that endpoint exists and uses your live secret key, or card payments will fail
(PayPal and WhatsApp still work without it).

## 6. Wholesale flow

wholesale.html carton cards and product-wholesale*.html pages check out through checkout.html
with `variant=half/full` or `v30/v60/v90` — quote-style orders; fulfill manually or create
matching Woo products later. 500+ boxes route to WhatsApp.
