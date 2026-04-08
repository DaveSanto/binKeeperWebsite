# BinKeeper — Claude Code Context

## What This App Is

**BinKeeper** is a standalone React Native / Expo mobile app for household and group inventory management. Users label physical bins with QR codes, photograph and catalog contents using AI, and find items instantly by searching. The app also supports shared group bins (sports teams, game night, vacation homes, community boxes) and a dedicated moving mode.

Website: **binkeeperapp.com**
Repos: `binkeeper-app` (this repo), `binkeeper-site` (marketing site)

---

## Tech Stack

| Layer | Choice |
|---|---|
| Framework | React Native / Expo (Expo Router) |
| Database | Firebase Firestore |
| Storage | Firebase Storage (photos) |
| Auth | Firebase Auth |
| AI Identification | Anthropic Claude API — `claude-sonnet-4-20250514` — vision |
| QR Scanning | expo-camera or expo-barcode-scanner |
| QR Generation | react-native-qrcode-svg |
| Payments | Stripe (web only — no IAP) |
| Backend functions | Firebase Cloud Functions |

**Root layout pattern** (from prior sessions):
```tsx
<GestureHandlerRootView style={{ flex: 1 }}>
  <SafeAreaProvider>
    <ThemeProvider ...>
      <Stack screenOptions={{ headerShown: false }}>
        <Stack.Screen name="(auth)" />
        <Stack.Screen name="(app)" />
      </Stack>
    </ThemeProvider>
  </SafeAreaProvider>
</GestureHandlerRootView>
```

---

## Tier System

All feature gating reads from the `tier` field on the user's Firestore document. The app never knows how payment happened — it only reads `tier`.

| Value | Tier | Bins | Items/bin | AI Scans |
|---|---|---|---|---|
| `0` | Free | 2 | 10 | 5 lifetime |
| `1` | Keeper ($3.99/mo) | 25 | 40 | None |
| `2` | Keeper Pro ($12/mo) | Unlimited | Unlimited | 50/month |
| `3` | BinKeeper Move ($25 one-time) | Unlimited | Unlimited | 150 / 30 days |

**Item capacity add-on:** $0.99/bin for +50 items, one-time, stackable. Available on Free and Keeper tiers.

**All payments happen on binkeeperapp.com via Stripe — never inside the app.** No payment UI, no external links inside the app (App Store compliance).

---

## Firestore Data Model

### `users/{userId}`
```
tier: 0 | 1 | 2 | 3
tierExpiry: timestamp | null
freeAiScansUsed: number          // 0–5, never resets
aiScansUsed: number              // Pro monthly, resets 1st of month
aiScansResetDate: timestamp
moveAiScansUsed: number          // 0–150, resets on new Move purchase
movePurchaseDate: timestamp | null
itemLimitAddOns: {}              // map of binId → addOnCount
stripeCustomerId: string | null
displayName: string
email: string
createdAt: timestamp
```

### `bins/{binId}`
```
name: string
description: string
ownerId: string                  // userId
memberIds: string[]              // for group sharing
photoURL: string                 // Firebase Storage URL
qrCodeValue: string              // = binId
isPublic: boolean                // public shareable QR
season: string | null            // e.g. "christmas", "halloween"
itemLimitAddOns: number          // add-on packs purchased (each = +50 items)
effectiveItemLimit: number       // computed: base + (addOns × 50)
createdAt: timestamp
updatedAt: timestamp
```

### `items/{itemId}`
```
name: string
description: string
binId: string
photoURL: string
status: "in" | "out"
checkedOutBy: string | null
checkedOutAt: timestamp | null
checkedOutLocation: string | null
tags: string[]
createdAt: timestamp
updatedAt: timestamp
```

### `checkoutHistory/{eventId}`
```
itemId: string
userId: string
action: "checkin" | "checkout"
location: string | null
timestamp: timestamp
```

---

## Firebase Cloud Functions

| Function | Trigger | Purpose |
|---|---|---|
| `createCheckoutSession` | HTTPS callable | Creates Stripe Checkout Session |
| `stripeWebhook` | HTTPS POST | Handles Stripe events → updates `tier` in Firestore |
| `resetAiScans` | Scheduled (1st of month) | Resets `aiScansUsed` for Pro users |
| `checkMoveExpiry` | Scheduled (daily) | Downgrades expired Move users to Free |

---

## Key Features

1. **QR bin labeling** — generate, print, scan
2. **AI item identification** — Claude vision API identifies items from photos
3. **Public shareable QR** — read-only bin view for anyone, no account needed
4. **Group/household sharing** — multiple users per bin
5. **Check-in / check-out** — with optional location note
6. **Item search** — full-text across all bins
7. **Seasonal reminders** — holiday/season tags trigger push notifications
8. **Item capacity add-on** — $0.99 per bin for +50 items
9. **BinKeeper Move** — 30-day unlimited mode for moving

---

## App Store Compliance Notes

- **No payment UI inside the app**
- **No links to binkeeperapp.com for purchasing**
- When a user hits a limit, show neutral messages only:
  - ✅ "You've reached your bin limit."
  - ✅ "You've used all 5 of your free AI scans."
  - ❌ "Upgrade at binkeeperapp.com" — never say this

---

## Docs Folder

Full detail on all product decisions lives in `/docs`:

- `binkeeper-pricing-context.md` — all tier details, enforcement rules, add-on, cost rationale
- `binkeeper-payment-context.md` — Stripe integration, Firestore entitlement fields, Cloud Functions spec, IAP notes
- `binkeeper-features-context.md` — full feature list, positioning, viral mechanics, use case angles
- `binkeeper-marketing-plan.md` — channel strategy, content pillars, launch sequence, 90-day targets

**Read the relevant doc before implementing any feature that touches payments, tiers, or entitlements.**

---

*Last updated: April 2026*
