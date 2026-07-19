# WooCommerce Backend Update Guide — Carton Pricing Model

The site now sells **full cartons only (10 / 30 / 60 boxes)** with **free delivery included** and a
**"+1 free box on first order"** promise, anchored against the $89/box single-retail value.
The front-end reuses your existing WooCommerce variation IDs, so until you update the variations
in WP Admin, add-to-cart will show the OLD pack names and prices.

## Retail pricing ladder (uniform across products)

| Tier | Price | Per box | vs $89 retail |
|---|---|---|---|
| 10 Boxes — Full Carton | $390 | $39 | Save 56% |
| 30 Boxes — Full Carton (Most Popular) | $1,020 | $34 | Save 62% |
| 60 Boxes — Full Carton (Best Value) | $1,680 | $28 | Save 69% |
| Couples Bundle (sets = his + hers) | $780 / $2,040 / $3,360 | $78 / $68 / $56 per set | — |

Wholesale channel (wholesale.html + product-wholesale pages): 30 / 60 / 90 boxes at $34 / $28 / $26 per box,
500+ boxes $24/box via WhatsApp — matches the June pricing already in place.

## 1. Update product variations (WP Admin → Products → edit → Variations)

Rename each variation and set its new price. The 4th (old 6-pack) variation of each product should be
**disabled or deleted** — the site never sends it.

| Product (ID) | Variation ID | New name | New price |
|---|---|---|---|
| Royal Honey VIP Pack (76) | 77 | 10 Boxes — Full Carton | $390 |
| | 78 | 30 Boxes — Full Carton | $1,020 |
| | 79 | 60 Boxes — Full Carton | $1,680 |
| | 80 | *(disable)* | — |
| Black Horse Vital Honey (81) | 82 / 83 / 84 | 10 / 30 / 60 Boxes | $390 / $1,020 / $1,680 |
| | 85 | *(disable)* | — |
| Etumax Royal Honey For Her (86) | 87 / 88 / 89 | 10 / 30 / 60 Boxes | $390 / $1,020 / $1,680 |
| | 90 | *(disable)* | — |
| Lux Honey For Her (91) | *(page sends no variation ID — set product default to the same ladder, 10 Boxes default)* | | |
| VitaMAX Couples Bundle (93) | 94 / 95 / 96 | 10 / 30 / 60 Sets | $780 / $2,040 / $3,360 |
| | 97 | *(disable)* | — |
| ICE ENERGY (98) | 99 / 100 / 101 | 10 / 30 / 60 Boxes | $390 / $1,020 / $1,680 |
| | 102 | *(disable)* | — |
| HoneyMax (103) | 104 / 105 / 106 | 10 / 30 / 60 Boxes | $390 / $1,020 / $1,680 |
| | 107 | *(disable)* | — |
| Gladiator (108) | 109 / 110 / 111 | 10 / 30 / 60 Boxes | $390 / $1,020 / $1,680 |
| | 112 | *(disable)* | — |
| Chobe Pure Honey (113) | 114 / 115 / 116 | 10 / 30 / 60 Jars | $390 / $1,020 / $1,680 |
| | 117 | *(disable, if it exists)* | — |

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
