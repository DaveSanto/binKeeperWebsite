# BinKeeper — Pricing & Tier System
*Handoff context for app and website development*

---

## Tiers at a Glance

| Feature | Basic | Keeper | Keeper Pro | BinKeeper Move |
|---|---|---|---|---|
| Bins | 2 | 25 | Unlimited | Unlimited |
| Items per bin | 10 | 40 | Unlimited | Unlimited |
| Item capacity add-on | ✅ | ✅ | N/A | N/A |
| QR scanning | ✅ | ✅ | ✅ | ✅ |
| Public shareable QR | ✅ | ✅ | ✅ | ✅ |
| Manual entry | ✅ | ✅ | ✅ | ✅ |
| AI item identification | 5 scans (one-time) | ❌ | 50 scans/mo | 150 scans (one-time) |
| Household/group sharing | ❌ | ✅ | ✅ | ✅ |
| Seasonal reminders | ❌ | ✅ | ✅ | ✅ |
| Check-in / check-out | ✅ | ✅ | ✅ | ✅ |
| Monthly price | $0 | $3.99/mo | $12/mo | N/A |
| Annual price | — | $43/year (~$3.58/mo) | $122/year (~$10.17/mo) | N/A |
| Annual savings | — | 10% | 15% | N/A |
| One-time price | — | — | — | $25 |

---

## Tier Details

### Basic
- **Bins:** 2 maximum
- **Items per bin:** 10 maximum (expandable via add-on)
- **AI scanning:** 5 scans, one-time lifetime allotment (not per month)
- **Public shareable QR:** Yes — any bin can be made publicly viewable without a BinKeeper account
- **Check-in / check-out:** Yes
- **Household sharing:** No
- **Seasonal reminders:** No
- **Intent:** Acquisition / taste of the product. Intentionally limited to create upgrade pressure. No credit card required.

### Keeper — $3.99/month or $43/year
- **Bins:** 25 maximum
- **Items per bin:** 40 maximum (expandable via add-on)
- **AI scanning:** Not available
- **Public shareable QR:** Yes
- **Household/group sharing:** Yes
- **Seasonal reminders:** Yes
- **Check-in / check-out:** Yes
- **Annual discount:** 10% off monthly rate
- **Intent:** Core paying tier for regular household and group users.

### Keeper Pro — $12/month or $122/year
- **Bins:** Unlimited
- **Items per bin:** Unlimited (add-on not needed)
- **AI scanning:** 50 scans per calendar month, resets on the 1st
- **Public shareable QR:** Yes
- **Household/group sharing:** Yes
- **Seasonal reminders:** Yes
- **Check-in / check-out:** Yes
- **Annual discount:** 15% off monthly rate
- **Intent:** Power users, large households, heavy AI users.

### BinKeeper Move — $25 one-time
- **Bins:** Unlimited
- **Items per bin:** Unlimited
- **AI scanning:** 150 scans (one-time allotment, valid for 30 days)
- **Duration:** 30 days from date of purchase
- **After expiry:** Account returns to Basic plan. All bins and items preserved in read-only state.
- **Repeatable:** Yes — can be purchased again for a future move
- **Cannot be subscribed to:** This is a one-time purchase only, not a recurring subscription
- **Public shareable QR:** Yes
- **Household/group sharing:** Yes
- **Seasonal reminders:** Yes
- **Check-in / check-out:** Yes
- **Intent:** Full-featured access during a move. 150 scans covers even a large 4BR+ move comfortably. Natural upsell moment to Keeper or Pro occurs at expiry.

---

## Item Capacity Add-On

Available on Basic and Keeper plans. Not needed on Pro or Move (both have unlimited items).

- **Price:** $0.99 per bin
- **What it does:** Adds 50 items to that specific bin, permanently
- **Purchase type:** One-time, per bin
- **Stackable:** Yes — purchase multiple times on the same bin for very large bins (e.g. a Christmas ornament bin with 200+ items)
- **Use case messaging:** "Got a bin with 200 ornaments? Expand it for less than a dollar."

---

## Enforcement Rules

- **Bin limit:** Enforced at bin creation. User cannot create a new bin if at their tier maximum. App surfaces an upgrade prompt at this moment.
- **Item limit:** Enforced at item creation within a bin. Basic plan cap is 10 items/bin; Keeper cap is 40 items/bin. App surfaces an upgrade prompt or add-on purchase option at this moment.
- **AI scan limit (Basic):** 5 scans lifetime. Once used, AI scanning is locked permanently unless user upgrades to Pro or purchases Move.
- **AI scan limit (Pro):** 50 scans per calendar month. Resets on the 1st. Remaining count visible in app ("34 AI scans remaining this month"). Clear message shown when limit is reached.
- **AI scan limit (Move):** 150 scans valid for 30 days from purchase. Counter visible in app. Scans do not roll over after expiry.
- **Move expiry:** At 30 days, account silently downgrades to Basic. Data preserved read-only. Neutral message shown: "Your BinKeeper Move period has ended. Visit binkeeperapp.com to keep your data accessible."
- **Downgrade behavior:** If a user downgrades from a higher tier, existing bins and items are preserved in read-only state. They cannot add new bins/items beyond their new tier's limits until within bounds.

---

## Promotional Pricing & Coupons

Coupon and promo codes exist as a separate lever. Used sparingly and intentionally to avoid eroding perceived value.

**Approved use cases:**
- Early access / beta user rewards (one-time)
- App launch PR push (limited time, limited quantity)
- Win-back campaigns for churned subscribers
- Strategic partnership or cross-promotion deals

**Rules:**
- All coupons must have an expiration date — no open-ended codes
- Limit redemptions per code (e.g., first 100 users only)
- Never publish codes broadly — avoid coupon aggregator site exposure
- No more than one active coupon campaign at a time where possible
- Coupon discounts applied via Stripe's built-in coupon system — no custom discount logic in the app

---

## Cost Rationale (Internal Reference)

| Tier | COGS est./user/mo | Gross margin |
|---|---|---|
| Keeper monthly | ~$0.15 | ~$3.84 (~96%) |
| Keeper annual | ~$0.15 | ~$3.43/mo effective (~96%) |
| Pro monthly | ~$0.55–$0.85 | ~$11.15–$11.45 (~93%) |
| Pro annual | ~$0.55–$0.85 | ~$9.32–$9.62/mo effective (~92%) |
| Move (one-time) | ~$2.25 | ~$22.75 (~91%) |

Move COGS breakdown: Anthropic API at 150 scans × $0.008 = $1.20 + Stripe fee ~$1.03 + negligible Firebase.

---

## Naming Conventions (for UI copy)

- Basic tier: **"Basic"**
- Middle tier: **"Keeper"**
- Top tier: **"Keeper Pro"**
- Moving tier: **"BinKeeper Move"**
- AI feature: **"AI Scanning"** (use consistently everywhere)
- Scan counter label: **"AI scans remaining"**
- Add-on label: **"Expand this bin"** or **"Add 50 items — $0.99"**

---

*Last updated: April 2026*
*See also: BinKeeper Payment & Entitlement document, BinKeeper Features document*
