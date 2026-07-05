# VitaMAX — Couples Bundle Ad Campaign Spec

> Ready-to-launch specification. Once the Meta Ads connection is stable, this maps
> 1:1 to the campaign → ad set → ad structure. Nothing here spends money until an
> ad set is set to ACTIVE.

---

## 0. Pre-launch checklist (must be true before launch)

- [ ] **VitaMAX Page linked to ad account.** Page `1068097049729243` (VitaMAX) must be
      assigned to ad account `158299606221401` in Meta Business Settings →
      Accounts → Pages → VitaMAX → Assign ad account. Until this is done, Meta will
      reject the creative with "page not available for this ad account."
- [ ] **Payment method active** on account `158299606221401` (already ✅ per account check).
- [ ] **Market & currency confirmed** (see §2 — site shows USD, account is MYR).
- [ ] **Destination URL / pixel** confirmed (see §6).

---

## 1. Account & identity

| Field | Value |
|---|---|
| Ad account | `158299606221401` (MYR) |
| Facebook Page | **VitaMAX** — `1068097049729243` |
| Instagram | Use connected IG, or "Use Facebook Page" as IG identity |
| Product | VitaMAX Couples Bundle – His & Hers |
| Landing page | `product-couples-bundle.html` (host URL TBD — see §6) |

---

## 2. ⚠️ Market & currency decision (needs your call)

The website lists the Couples Bundle at **$339 (was $390, save $51)** and says
"Ships to US, Canada and Europe" — but ad account `158299606221401` is **MYR** and the
domain is `vitamax-my` (Malaysia).

**Pick one before launch:**
- **A — Malaysia (MYR):** Target Malaysia, budget in MYR (RM15/day). Update ad copy to
  RM pricing. Best if you actually fulfil/sell in MYR.
- **B — US/CA/EU (USD site):** Target those countries, keep $339 messaging. Note the ad
  account still bills in MYR — that's fine, Meta converts; only the *audience geo* changes.

*Default assumed below: Option A (Malaysia), RM15/day, since the account is MYR.*
*Change the geo + price copy if you choose B.*

---

## 3. Campaign level

| Setting | Value | Notes |
|---|---|---|
| Campaign name | `VMX | Couples Bundle | Sales | Jul2026` | |
| Objective | **OUTCOME_SALES** | Drives purchases, not just traffic |
| Buying type | AUCTION | |
| Special ad categories | None | (not credit/housing/employment) |
| Campaign budget (CBO) | Off | Budget set at ad set level (§4) |
| Status at creation | **PAUSED** | Flip to ACTIVE only after review |

---

## 4. Ad set level

| Setting | Value |
|---|---|
| Ad set name | `Couples | BroadMY | Purchase | RM15` |
| Daily budget | **RM15/day** (1500 cents; account min is ~415 cents ✅) |
| Optimization goal | Conversions — **Purchase** |
| Conversion location | Website |
| Pixel / dataset | *TBD — confirm pixel is installed on site (see §6)* |
| Attribution | 7-day click, 1-day view (default) |
| Schedule | Start immediately, no end date (monitor daily) |
| **Audience (Option A - MY)** | Malaysia; ages 25–55; all genders (couples angle) |
| Detailed targeting | Start **broad** (no interests) and let the algorithm find buyers; OR layer: wellness, supplements, gifting, relationships |
| Placements | **Advantage+ (automatic)** — recommended for a fresh account |
| Languages | Malay + English (optional) |

> Starting broad + Advantage+ placements is the modern best practice for a small
> daily budget; avoid over-narrowing at RM15/day or the algorithm starves.

---

## 5. Ad level (creative)

| Setting | Value |
|---|---|
| Ad name | `Couples | Image | His&Hers | v1` |
| Page | **VitaMAX** `1068097049729243` |
| Format | Single image (start simple) → later test carousel with couples-1..4 |
| Primary image | `vitamax-images/couples-1.png` (hero) |
| Alt images to test | `couples-2.png`, `couples-3.png`, `couples-4.png` |
| Call to action | **Shop Now** |
| Destination | Couples Bundle product URL (§6) |

### Copy — Primary text (Option A / MY)
> Primary (main): 
> *"His & Hers, in one box. 🍯 Royal Honey VIP for him + Lux Honey for her — the
> couples wellness set that's beautifully packaged and discreetly shipped. Natural
> energy, vitality & closeness, together. Limited bundle — save today."*

> Headline: **"VitaMAX Couples Bundle — His & Hers"**
> Description: **"Discreet shipping · 30-day guarantee · Natural royal honey"**

### Copy — Alt variations (for A/B testing later)
1. Headline: *"The couples' wellness ritual"* — Primary: *"One box for both of you. Royal Honey for him, Lux Honey for her. Natural vitality, discreetly delivered."*
2. Headline: *"Better together 🍯"* — Primary: *"Give your relationship a natural boost. His & Hers honey bundle — premium, discreet, guaranteed."*

*(If Option B / USD, keep "$339 (was $390)" and "Ships to US, Canada & Europe".)*

---

## 6. Open items to confirm before ACTIVE

1. **Landing page URL** — where is `product-couples-bundle.html` hosted publicly?
   (Needed for the ad destination.)
2. **Meta Pixel** — is a pixel installed on the site for Purchase tracking? If not,
   conversion optimization can't work; fall back to Traffic objective or install pixel first.
3. **Market/currency** — confirm Option A (MY) vs B (USD) from §2.
4. **Price accuracy** — confirm the current live price for the chosen market.

---

## 7. Launch sequence (what I'll run once Meta connects)

1. Verify VitaMAX page is linked to account `158299606221401`.
2. Upload `couples-1.png` as ad image → get image hash.
3. Create campaign (OUTCOME_SALES, PAUSED).
4. Create ad set (RM15/day, Purchase, audience per §4).
5. Create creative (page = VitaMAX, image, copy from §5, Shop Now, destination URL).
6. Create ad.
7. Report back campaign/ad set/ad IDs + preview link.
8. **You review** → I flip ad set to ACTIVE (or you do it in Ads Manager).

---

## 8. First-week monitoring plan (after launch)

Check daily; key thresholds at RM15/day:
- **CTR (link)** < 0.8% → creative/copy is weak → swap image (test couples-2..4).
- **Cost per purchase** way above product margin after ~3–4x budget spent → pause & rethink audience.
- **Zero purchases after ~RM60–75 spend** → likely landing page / offer / tracking issue, not the ad.
- **Winner found** → duplicate ad set and scale budget +20–30% every 2–3 days.
