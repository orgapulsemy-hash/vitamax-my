# WooCommerce Backend Update Guide — Carton Pricing Model

The site now sells **full cartons only (10 / 30 / 60 boxes)** with **free delivery included** and a
**"+1 free box on first order"** promise. The front-end reuses your existing WooCommerce variation IDs,
so until you update the variations in WP Admin, add-to-cart will show the OLD pack names and prices.

## Retail pricing ladder (uniform across products)

| Tier | Price | Per box | Savings vs 10-box rate |
|---|---|---|---|
| 10 Boxes — Full Carton (min. order) | $890 | $89 | — |
| 30 Boxes — Full Carton (Most Popular) | $2,370 | $79 | Save $300 |
| 60 Boxes — Full Carton (Best Value) | $4,140 | $69 | Save $1,200 |
| Couples Bundle (sets = his + hers) | $1,780 / $4,740 / $8,280 | $178 / $158 / $138 per set | — / $600 / $2,400 |

Wholesale channel (wholesale.html + product-wholesale pages): 30 / 60 / 90 boxes at $79 / $69 / $59 per box
($2,370 / $4,140 / $5,310), 500+ boxes via WhatsApp. Note: the $59/box 90-box rate extends your
89→79→69 progression — adjust if you want a different rate there.

## 1. Update product variations (WP Admin → Products → edit → Variations)

Rename each variation and set its new price. The 4th (old 6-pack) variation of each product should be
**disabled or deleted** — the site never sends it.

**Now includes the 5-box "Try It" entry tier.** The site's 5-box option reuses each product's
**4th variation** (the old "6-pack" that was previously disabled) — so instead of disabling it,
**re-price it to $495 ($990 for Couples), rename it "5 Boxes", and make sure it's enabled/purchasable.**
Every product's 4 variations are now all live.

Full ladder per box product: **5 = $495 ($99/box) · 10 = $890 ($89) · 30 = $2,370 ($79) · 60 = $4,140 ($69)**.

| Product (ID) | Variation ID | Name | Price |
|---|---|---|---|
| Royal Honey VIP Pack (76) | 80 | **5 Boxes** (was 6-pack) | **$495** |
| | 77 / 78 / 79 | 10 / 30 / 60 Boxes | $890 / $2,370 / $4,140 |
| Black Horse Vital Honey (81) | 85 | **5 Boxes** | **$495** |
| | 82 / 83 / 84 | 10 / 30 / 60 Boxes | $890 / $2,370 / $4,140 |
| Etumax Royal Honey For Her (86) | 90 | **5 Boxes** | **$495** |
| | 87 / 88 / 89 | 10 / 30 / 60 Boxes | $890 / $2,370 / $4,140 |
| Lux Honey For Her (91) | 134 | **5 Boxes** | **$495** |
| | 131 / 132 / 133 | 10 / 30 / 60 Boxes | $890 / $2,370 / $4,140 |
| VitaMAX Couples Bundle (93) | 97 | **5 Sets** | **$990** |
| | 94 / 95 / 96 | 10 / 30 / 60 Sets | $1,780 / $4,740 / $8,280 |
| ICE ENERGY (98) | 102 | **5 Boxes** | **$495** |
| | 99 / 100 / 101 | 10 / 30 / 60 Boxes | $890 / $2,370 / $4,140 |
| HoneyMax (103) | 107 | **5 Boxes** | **$495** |
| | 104 / 105 / 106 | 10 / 30 / 60 Boxes | $890 / $2,370 / $4,140 |
| Gladiator (108) | 112 | **5 Boxes** | **$495** |
| | 109 / 110 / 111 | 10 / 30 / 60 Boxes | $890 / $2,370 / $4,140 |
| Chobe Pure Honey (113) | 117 | **5 Jars** | **$495** |
| | 114 / 115 / 116 | 10 / 30 / 60 Jars | $890 / $2,370 / $4,140 |

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

## 5. Stripe backend

Card payments on checkout.html call `https://vitamax.my/create-payment-intent.php`.
Make sure that endpoint exists and uses your live secret key, or card payments will fail
(PayPal and WhatsApp still work without it).

## 6. Wholesale flow

wholesale.html carton cards and product-wholesale*.html pages check out through checkout.html
with `variant=half/full` or `v30/v60/v90` — quote-style orders; fulfill manually or create
matching Woo products later. 500+ boxes route to WhatsApp.
