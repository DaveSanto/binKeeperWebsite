# BinKeeper — Session Notes & Decisions Log
*May 2, 2026*

---

## Key Decisions Made This Session

### 1. BinKeeper Move price raised from $25 → $30

**Rationale:** The referral partner program requires a $5 finder's fee per conversion. At $25 the net margin after fees, Stripe, and Anthropic API costs was ~$17.97. At $30 the net is ~$22.81. The $5 price increase is unlikely to affect conversion given the context (someone mid-move, referred by a trusted service provider).

**Documents to update:**
- [x] binkeeper-pricing-context.md
- [x] binkeeper-payment-context.md
- [ ] BinKeeper-Terms-of-Service.docx — Section 4 (BinKeeper Move)
- [ ] BinKeeper-Refund-Policy.docx — explicitly references "$25 one-time"
- [ ] binkeeper.us pricing page
- [ ] binkeeperapp.com pricing page
- [ ] Stripe product price (update before going live)

**Refund Policy change needed:**
Find: *"BinKeeper Move is a one-time purchase that gives you 30 days of full access and 150 AI scans."*
Update the price reference from $25 to $30 wherever it appears.

**Terms of Service change needed:**
Section 4, BinKeeper Move subsection — no explicit price listed, but review for any $25 references.

---

### 2. Referral Partner Program established

BinKeeper will offer referral partnerships to moving companies and storage facilities.

**Structure:**
- Each partner gets a unique Stripe coupon code tied to their business name
- Customers who use the code receive a discount at checkout
- Partners earn a **$5 finder's fee** per converted customer
- Finder's fees paid out manually at launch; automate if volume justifies it
- Partners must demonstrate at least one conversion before fees are paid

**Legal note:** Referral fees are legal in Massachusetts. Use the term "referral fee" not "commission." Issue a 1099 to any individual paid more than $600 in a calendar year (less relevant for business entities).

**Outreach strategy:**
- Target small-to-mid local moving companies and regional storage facilities
- Avoid national chains — they can't make decisions quickly
- A/B test two message versions:
  - **Version A:** Founding partner / social shoutout angle
  - **Version B:** Concrete $5 finder's fee angle
- Track response rates per version; double down on winner

---

### 3. Go-to-market strategy pivoted to direct outreach

**Approach:** Rather than broad social media marketing, focus first on getting 10 strangers to use the app. Channels in priority order:

1. **Instagram DMs to home organization influencers** (5k–50k follower range)
2. **Direct outreach to moving companies and storage facilities** via Instagram and email
3. **Reddit** — participate in existing threads rather than promotional posts; modmail approach having mixed results
4. **Nextdoor** — Little Free Library and tool-share use cases

**Budget:** Hold the $50–200/month budget until one channel shows signal. Don't spend on paid ads until you know which use case resonates with strangers.

**Success metric for Phase 1:** 10 strangers sign up and create more than one bin. Behavior (return visits, multiple bins created) matters more than stated feedback.

---

### 4. Legal and infrastructure priorities deprioritized

Per advisor guidance: *"Find out if anybody wants this before you invest in all that."*

- Insurance: deferred
- Legal document polish: deferred (existing docs are solid enough)
- Stripe live mode: should be enabled before referral partner outreach begins, since the pitch requires a real discount code

**Known legal doc issues to fix before Stripe goes live:**
- Privacy Policy inconsistency: `privacy@binkeeperapp.com` (Section 6) vs. `admin@binkeeperapp.com` (Section 9) — both appear on the live binkeeper.us privacy page
- One copy of Privacy Policy footer still references "EvenKeel LLC" — must be "Santtro LLC" everywhere
- Public Bins not yet referenced in Privacy Policy (anonymous location data from scanners)

---

### 5. BinKeeper is not an "AI wrapper"

Articulated response to the common investor/user skepticism:

- AI is one feature, not the product. Remove it and BinKeeper still works.
- The core innovation is the physical-to-digital bridge via QR codes.
- The data moat is the inventory itself — users with 30+ bins cataloged don't leave.
- AI Scanning removes real tedium (cataloging a full box), not a fake problem.
- Test: if Anthropic shut down their API tomorrow, BinKeeper would lose one feature. An AI wrapper would cease to exist.

---

## Outreach Messages Drafted This Session

### Instagram influencer (home organization)
> Hi [Name]! I love what you've built here — your content is exactly the kind of thing that inspired me to finally get my own storage chaos under control.
>
> I'm Dave, and I'm a solo developer who built an app called BinKeeper. The idea is simple: photograph what's in a storage bin, attach a QR code label to the outside, and you can instantly search for any item across all your bins. No more mystery boxes.
>
> I'm not a big company — just someone who built this because I genuinely needed it. I'm looking for a handful of real people who love organization to try it and share it with a few friends if they like it. No pressure, no script.
>
> Would you be open to giving it a try? I'll set you up with full access, no strings attached.
>
> Dave — binkeeper.us

### Moving companies (Version A — Founding partner)
> Hi [Name]! I'm Dave, a solo developer who built an app called BinKeeper — it lets movers photograph box contents, attach a QR code label, and search for any item instantly. No more opening 15 boxes looking for one thing.
>
> I have a version called BinKeeper Move built specifically for this. I'd love to set you up as a founding launch partner — your own branded discount code for customers, and a shoutout on our social channels.
>
> Interested? — Dave / binkeeper.us

### Moving companies (Version B — Finder's fee)
> Hi [Name]! I'm Dave, a solo developer who built an app called BinKeeper — it lets movers photograph box contents, attach a QR code label, and search for any item instantly. No more opening 15 boxes looking for one thing.
>
> I have a version called BinKeeper Move built specifically for this. For every customer who purchases using your referral code, I'll send you $5. Your customers get a discount, you get a finder's fee — no cost, no commitment.
>
> Interested? — Dave / binkeeper.us

### Storage facilities
> Hi [Name]! I'm Dave, a solo developer who built an app called BinKeeper — it lets people photograph what goes into each bin, attach a QR code label, and find anything instantly. Perfect for anyone who's ever forgotten what's in their storage unit.
>
> I'd love to set you up with a custom discount code to share with your customers — they get a deal on the app, and you're offering them something useful from day one.
>
> Interested? — Dave / binkeeper.us

---

*This document should be saved to the BinKeeper project folder alongside the other context .md files.*
