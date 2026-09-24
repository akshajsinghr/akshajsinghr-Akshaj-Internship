# qset1 Q4 — Salaried 30s: ₹3K Amazon gift card to brother

**Stack:** SBI Cashback + ICICI Amazon Pay
**Question:** *"Need to send my brother a ₹3K Amazon gift card for his birthday. Does this even count for rewards?"*
**Difficulty:** Easy (D5 cross-validation — gift cards category, 5th datapoint)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (SBI Cashback, gift card purchase): **EXCLUDED** per April 2026 devaluation — "card/gift/novelty/souvenir shops" explicitly excluded (MCC 5947 etc.)
- (Amazon Pay ICICI, gift card on Amazon): **EXCLUDED** — "Amazon Pay Gift Cards" specifically excluded per Amazon Pay ICICI T&C; gift card purchase doesn't earn cashback (would be a closed-loop wallet load)
- General pattern: gift cards (MCC 5499/5947) universally excluded across Indian cards

## Intent + DecisionContext
- intent_type: `purchase_routing` (0.85) + `EXPLAINER` (0.55)
- amount ₹3,000; category=gift-card-purchase
- No clarifying-Q

## Engine traversal
- Both cards: 0 earn (EXCLUDED)
- Winner: None earning — purchase as utility, no reward optimization possible

## Conversational
- Narration: "**No — gift cards don't earn rewards on either card** (or on most Indian credit cards). SBI Cashback added gift cards to its exclusion list in April 2026. Amazon Pay ICICI doesn't give cashback on Amazon Pay Gift Cards specifically (closed-loop wallet load category). **There's no smart-routing here** — buy the gift card on whichever is most convenient (Amazon Pay ICICI auto-credits to your Amazon balance if there's any small future cashback opportunity, but for this purchase both earn ₹0). **Workaround sometimes discussed in communities**: buying via specific portals (Magicpin, iShop) that route the transaction as a 'shopping' MCC instead of gift card — but these come with their own surcharges + risk of issuer clawback, and aren't worth the hassle for ₹3K."

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ 5th D5 datapoint (insurance, jewelry, college fees, coaching, gift cards) — pattern is overwhelming
- ✓ Workaround-discovery narration (Magicpin/iShop) — partially implements E65 from Q19
- ⚠ G1 — community-workaround narration is hand-curated; not engine-derived. E65 (workaround-discovery) should formalize this.
- ⛔ None

## Refinements engaged + NEW
- Existing: R49, R50, R51, DA1, DA11, E1, E15, CV1
- NEW: E65 (from Q19) further validated — workaround-discovery should be a recommendation engine component

## 80/20 verdict
**SHIP-AS-IS engine answer + STRONGLY URGENT D5 cluster + E65 promote from TIER-2 to TIER-1** (workaround-discovery has now appeared in 3 queries: Q19 jewelry, P1.3 college fees, Q4 gift cards)

## Phase-0 sources
- [1] SBI Cashback April 2026 exclusions (verified today)
- [2] Amazon Pay ICICI gift card exclusion (verified today via Q19 retrieval)
- [3] Card Maven Amazon voucher double-dip guide (community workaround context)
