# BinKeeper — Legal Documents & Website Integration
*Handoff context for website development*

---

## Overview

BinKeeper has three legal documents that must be accessible on the website before any paid plan is offered. All three are written in plain language and should be published as static pages on binkeeperapp.com. They are operated under **Santtro LLC** as the legal entity behind BinKeeper until a dedicated BinKeeper entity is established.

---

## Documents & URLs

| Document | URL | File |
|---|---|---|
| Privacy Policy | binkeeperapp.com/privacy | BinKeeper-Privacy-Policy.docx |
| Terms of Service | binkeeperapp.com/terms | BinKeeper-Terms-of-Service.docx |
| Refund Policy | binkeeperapp.com/refunds | BinKeeper-Refund-Policy.docx |

---

## Where to Link Each Document

### Website footer
All three documents must be linked in the footer on every page of binkeeperapp.com. Suggested footer link labels:
- Privacy Policy
- Terms of Service
- Refund Policy

### Stripe checkout
Stripe requires a refund policy URL during account setup and displays it on the checkout page. Use: `https://binkeeperapp.com/refunds`

Stripe also allows (and recommends) linking to your privacy policy and terms during checkout configuration. Use:
- `https://binkeeperapp.com/privacy`
- `https://binkeeperapp.com/terms`

### App Store listing
The App Store requires a privacy policy URL in the app listing. Use: `https://binkeeperapp.com/privacy`

The terms of service URL is optional in the App Store listing but recommended. Use: `https://binkeeperapp.com/terms`

### Account creation flow (website)
When a user creates an account or completes a purchase on binkeeperapp.com, include a checkbox or inline notice such as:

> "By creating an account, you agree to our [Terms of Service] and [Privacy Policy]."

Links should open in a new tab. This creates a clear record that the user was presented with and agreed to the terms.

### Pricing page
Link to the Refund Policy from the pricing page, near the plan purchase buttons. Suggested placement: below the CTA buttons as a small text line — "Questions about billing? See our Refund Policy."

---

## Contact Email

All three legal documents direct users to: **admin@binkeeperapp.com**

This is the single contact point for privacy questions, support, refund requests, and legal inquiries. As the business grows, consider setting up forwarding addresses (e.g. support@, privacy@) that route to the admin inbox — but the published documents only need updating when a permanent address is established.

---

## Legal Entity

All three documents identify **Santtro LLC** as the operating entity behind BinKeeper. This is a temporary arrangement until a dedicated BinKeeper LLC is formed. When that happens, all three documents will need to be updated and republished, and the entity name will need to be updated in:

- All three legal pages on binkeeperapp.com
- Stripe account details
- Apple App Store developer account
- Domain registration records
- Firebase billing account

---

## Page Design Notes

- Legal pages should use the standard site header and footer for consistency
- Body text should be clean and readable — 16px minimum, good line height
- Section headings (H1, H2) should follow the document structure
- No sidebar, no ads, no distractions — these pages need to feel trustworthy
- Include the effective date prominently near the top of each page
- Include a "last updated" note at the bottom of each page

---

## Keeping Documents Current

These documents will need to be reviewed and potentially updated when:

- The operating legal entity changes from Santtro LLC to a BinKeeper-specific LLC
- New features are added that change what data is collected or how it is used (e.g. new third-party integrations)
- Pricing or plan structure changes materially
- A new contact email address is established

When any document is updated, notify existing users by email before the changes take effect if the changes affect data collection, data use, or payment terms. Minor corrections (typos, formatting) do not require notification.

---

*Last updated: April 2026*
*See also: BinKeeper Features document, BinKeeper Pricing & Tier System document, BinKeeper Payment & Entitlement document*
