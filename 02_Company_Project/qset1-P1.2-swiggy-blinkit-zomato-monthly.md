# qset1 P1.2 — First-time: ₹9000 Swiggy/Blinkit/Zomato monthly portfolio default

**Stack:** HDFC Millennia + Amazon Pay ICICI + RuPay virtual UPI
**Question:** *"My monthly Swiggy + Blinkit + Zomato is around ₹9,000 — am I even using the right cards for this?"*
**Difficulty:** Medium (recurring + multi-merchant + cap-binding)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (Millennia, Swiggy/Zomato): **EARNS_AT 5% capped ₹1,000 CashPoints/mo combined with Amazon/Flipkart etc.**
- (Millennia, Blinkit): NOT on Millennia 5X list; **EARNS_AT 1% base** (capped ₹1,000/mo total non-accelerated)
- (Amazon Pay ICICI, Swiggy/Zomato/Blinkit): **EARNS_AT 1% base** (no cap; categories outside Amazon ecosystem; precious-metals-exclusion only)
- HDFC MoneyBack+ would give 10X on Swiggy + BigBasket+Blinkit-class — but user doesn't have it

## Intent + DecisionContext
- intent_type: `portfolio_advisory` (0.85) — "right cards for this?"
- amount ₹9,000 recurring; categories=food-delivery (Swiggy/Zomato), quick-commerce (Blinkit)
- No clarifying-Q needed (user volunteered MTD)

## Engine traversal
- Query type: portfolio_advisory + purchase_routing per merchant
- Hard problems: #1 caps (Millennia ₹1,000/mo cap — binding!)
- Per-merchant routing:
  | Merchant | Best card | Monthly value |
  |---|---|---|
  | Swiggy ₹3K | Millennia 5% | ₹150 |
  | Zomato ₹3K | Millennia 5% | ₹150 |
  | Blinkit ₹3K | Amazon Pay ICICI 1% | ₹30 |
- Millennia 5% on Swiggy+Zomato = ₹300; well within ₹1,000/mo accelerated cap
- Total monthly value across stack ≈ ₹330 (3.7% effective on ₹9K)
- runner_up_loss_reason: "Amazon Pay ICICI gives 1% on all; switching all to it = ₹90 vs ₹330"

## Confidence + freshness
- HIGH confidence + fresh (all Tier 1.1 + Tier 2)

## Conversational
- t=0 → t=180ms preamble → t=400ms engine → t=1.2s narration begin → t=2.3s complete
- Narration: "Yes mostly. Use **Millennia on Swiggy + Zomato (5% = ₹300/month)**, and **Amazon Pay ICICI on Blinkit (1% = ₹30/month)** — Blinkit isn't on Millennia's 5X list. You're getting ~₹330/month total, 3.7% effective. If food delivery + quick-commerce grows to ₹15K+/month, consider HDFC MoneyBack+ which gives 10X on Swiggy/BigBasket (Blinkit not listed but BigBasket is close-substitute)."

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ Per-merchant routing within recurring-spend pattern
- ✓ Cap-headroom check (Millennia ₹1,000 cap NOT binding at this volume)
- ⚠ G1 — quick-commerce (Blinkit, Zepto, Instamart) is an emergent category often missing from per-card co-brand lists. **NEW refinement: explicit "quick-commerce" sub-category in MCC mapping (DA11 extension)**
- ⛔ None

## Bottlenecks + deeper-dives
- D9 (NEW) — **Quick-commerce as sub-category** distinct from "grocery" (BigBasket) and "food delivery" (Swiggy meals). Cards treat them differently; community confusion is endemic. **Proposed DA69: quick-commerce MCC sub-classification.**
- D10 (NEW) — When recurring-spend pattern is declared, narrator should surface "consider adding card X if you grow to Y monthly" — proactive recommendation engine pattern not currently formalized. **Proposed E66: growth-trigger card-recommendation hook.**

## Refinements engaged + NEW
- Existing: R49, R50, R51, DA1, DA11, E1, E5, E6, CV1
- NEW: **DA69** quick-commerce sub-category MCC mapping; **E66** growth-trigger recommendation

## 80/20 verdict
**SHIP-AS-IS** for the answer; TIER-2 for refinements (low frequency in v1; revisit after more queries)

## Phase-0 sources
- [1] HDFC Millennia 5X list (verified 2026-05-24)
- [2] Amazon Pay ICICI category rates (verified 2026-05-24)
