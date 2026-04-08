# BinKeeper — Payment Processing & Entitlement System
*Handoff context for app and website development*

---

## Payment Strategy Overview

BinKeeper uses **Stripe exclusively** for payment processing. There is no in-app purchase (IAP) integration with Apple or Google, and none is planned at this time. All subscription and one-time purchases happen on the BinKeeper website. The app reads entitlement state from Firestore and unlocks features accordingly — it never handles payment directly.

This approach avoids the Apple/Google 15–30% revenue cut and keeps margin intact.

---

## Domain & Website

- **Primary domain:** binkeeperapp.com
- **Also owned:** binkeeper.us
- binkeeperapp.com points to binkeeper.us
- All website references, App Store descriptions, and support links should use **binkeeperapp.com**

---

## How It Works (End to End)

1. User downloads the app and creates a free account (Firebase Auth)
2. User hits a feature limit (bins, items, or AI scanning)
3. App shows a neutral "limit reached" message — no payment UI, no external links
4. User visits **binkeeperapp.com** (or finds it via App Store description) to upgrade or purchase
5. User subscribes or purchases via Stripe on the website
6. Stripe webhook fires → Firebase Cloud Function updates the user's Firestore record
7. App reads updated entitlement from Firestore and unlocks features immediately

---

## Entitlement Fields (Firestore)

Each user document in Firestore contains the following fields that control feature access:

### `tier` — primary entitlement
| Value | Tier | Bins | Items/bin | AI scans |
|---|---|---|---|---|
| `0` | Free | 2 | 10 | 5 lifetime |
| `1` | Keeper | 25 | 40 | None |
| `2` | Keeper Pro | Unlimited | Unlimited | 50/month |
| `3` | BinKeeper Move | Unlimited | Unlimited | 150 (30-day) |

### Additional user-level fields:
- `tierExpiry` — subscription or Move expiration (ISO timestamp)
- `aiScansUsed` — integer, context-dependent (see below)
- `aiScansResetDate` — date of last Pro reset (1st of month)
- `freeAiScansUsed` — integer (0–5), lifetime counter for Free tier, never resets
- `moveAiScansUsed` — integer (0–150), resets only on new Move purchase
- `movePurchaseDate` — timestamp of Move purchase, used to calculate 30-day expiry
- `stripeCustomerId` — Stripe customer ID

### Per-bin fields (stored on each bin document):
- `itemLimit` — base limit per tier, overridden by add-on purchases
- `itemLimitAddOns` — integer, number of add-on packs purchased (each = +50 items)
- `effectiveItemLimit` — computed: base limit + (itemLimitAddOns × 50)

---

## Stripe Integration

### Stripe Products to Create

| Product | Type | Amount |
|---|---|---|
| Keeper Monthly | Recurring | $3.99/month |
| Keeper Annual | Recurring | $43.00/year |
| Keeper Pro Monthly | Recurring | $12.00/month |
| Keeper Pro Annual | Recurring | $122.00/year |
| BinKeeper Move | One-time | $25.00 |
| Item Capacity Add-On | One-time | $0.99 |

### Stripe Coupons
Manage all promotional discounts via Stripe's built-in coupon system. Do not implement custom discount logic in the app or backend. See the Pricing & Tier document for coupon policy.

### Stripe Checkout Flow (Website)
- User selects a plan or product on binkeeperapp.com
- Site creates a Stripe Checkout Session via a Firebase Cloud Function
- User completes payment on Stripe-hosted checkout page
- On success, Stripe redirects user back to binkeeperapp.com with confirmation
- Stripe webhook fires and Cloud Function updates Firestore

### Customer Portal
Enable Stripe's Customer Portal for self-managed subscriptions (upgrade, downgrade, cancel). Link from the binkeeperapp.com account page. Note: Move and Add-On are one-time purchases and do not appear in the subscription portal.

---

## Firebase Cloud Functions Required

### 1. `createCheckoutSession`
- Triggered by: website with `userId`, `priceId`, and optionally `binId` (for add-on purchases)
- Creates Stripe customer if needed, stores `stripeCustomerId`
- Returns Stripe Checkout Session URL
- For add-on purchases, passes `binId` as metadata so webhook knows which bin to update

### 2. `stripeWebhook`
- Triggered by: Stripe webhook POST
- Verifies Stripe webhook signature before processing
- Handles the following events:

| Event | Action |
|---|---|
| `customer.subscription.created` | Set `tier` (1 or 2) and `tierExpiry` |
| `customer.subscription.updated` | Update `tier` and `tierExpiry` |
| `customer.subscription.deleted` | Set `tier` to `0`, clear `tierExpiry` |
| `payment_intent.succeeded` (Move) | Set `tier` to `3`, set `movePurchaseDate`, reset `moveAiScansUsed` to `0` |
| `payment_intent.succeeded` (Add-on) | Increment `itemLimitAddOns` on the specified bin document |

### 3. `resetAiScans` (scheduled)
- Triggered: Firebase scheduled function, runs on the 1st of each month
- Resets `aiScansUsed` to `0` for all Pro (`tier: 2`) users
- Updates `aiScansResetDate`
- Does NOT reset Free lifetime scans or Move scans

### 4. `checkMoveExpiry` (scheduled)
- Triggered: Firebase scheduled function, runs daily
- Checks all users with `tier: 3`
- If `movePurchaseDate` is more than 30 days ago, sets `tier` to `0` and clears Move fields
- Preserves all bin and item data in read-only state

---

## Apple App Store Compliance

BinKeeper deliberately avoids IAP. To remain compliant with App Store guidelines:

- **No payment UI inside the app** — ever
- **No links to binkeeperapp.com for purchasing** inside the app
- **No text implying an external purchase path** inside the app
- When a user hits a limit, show neutral messages only:
  - ✅ "You've reached your bin limit."
  - ✅ "AI scanning is not available on your current plan."
  - ✅ "You've used all 5 of your free AI scans."
  - ❌ "Upgrade at binkeeperapp.com to get more bins."
  - ❌ "Visit our website to unlock Pro features."
- App Store listing and website can and should explain pricing and plans clearly
- Users discover binkeeperapp.com via App Store description or web search

---

## Subscription Lapse Handling

- If `tierExpiry` is in the past and Stripe has not renewed, treat as `tier: 0`
- Preserve all data in read-only state
- Show neutral message: "Some features are unavailable. Visit binkeeperapp.com to manage your subscription."
- Check at app launch and on each session resume

## Move Expiry Handling

- At 30 days post `movePurchaseDate`, `checkMoveExpiry` function sets `tier` to `0`
- All bins and items preserved read-only
- Show neutral message: "Your BinKeeper Move period has ended. Visit binkeeperapp.com to keep your data accessible."
- This is also a natural upsell moment — user has a fully organized move inventory and may want to keep it on Keeper or Pro

---

## IAP — Future Consideration

IAP is not implemented and not planned for the current release. May be revisited in the future.

Because the app only ever reads entitlement fields from Firestore, adding IAP later requires zero changes to app feature-gating logic. A new Cloud Function handling Apple/Google server notifications would write to the same Firestore fields. The app behaves identically regardless of payment source.

Do not build toward IAP now.

---

## Testing

- Use Stripe test mode and test card `4242 4242 4242 4242`
- Test the Add-On flow specifically — ensure `binId` metadata passes correctly through checkout to the webhook
- Test Move purchase: verify `tier: 3` sets correctly, 30-day expiry triggers correctly
- Test all subscription events: create, update, downgrade, cancel
- Firebase emulator suite for local Cloud Function testing
- Verify webhook signature verification before going live

---

*Last updated: April 2026*
*See also: BinKeeper Pricing & Tier System document, BinKeeper Features document*
