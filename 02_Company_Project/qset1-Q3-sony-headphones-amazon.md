# qset1 Q3 — Salaried 30s: ₹26K Sony WH-1000XM5 on Amazon — Cashback or Amazon Pay?

**Stack:** SBI Cashback + ICICI Amazon Pay
**Question:** *"Sony WH-1000XM5 for ₹26K on Amazon. Cashback or Amazon Pay?"*
**Difficulty:** Easy (clean 2-card comparison)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (SBI Cashback, Amazon online): **EARNS_AT 5% online; capped ₹2,000 cashback/cycle from April 2026** + **₹40K monthly online spend cap** (5% applies up to ₹40K, after that nothing)
- (Amazon Pay ICICI, Amazon Prime): **EARNS_AT 5% Prime / 3% non-Prime, no cap**
- (Amazon Pay ICICI, Amazon non-Prime): 3% no cap
- BBD-style instant discount: variable; should be checked

## Intent + DecisionContext
- intent_type: `purchase_routing` (0.92)
- amount ₹26K; merchant=amazon.in
- **Clarifying-Q (1):** "Are you an Amazon Prime member?" (materially changes Amazon Pay ICICI from 5% to 3%)

## Engine traversal
- Assuming Prime member:
  | Path | Math | NetValue |
  |---|---|---|
  | SBI Cashback | 26000 × 5% = ₹1,300 (within ₹40K monthly cap + ₹2,000 cashback cap) | ₹1,300 |
  | **Amazon Pay ICICI Prime** | 26000 × 5% = ₹1,300 no cap | ₹1,300 |
- TIE at 5%. Tiebreaker: **Amazon Pay ICICI** because (a) no monthly online cap → leaves SBI Cashback's ₹2,000 cap headroom intact for other Amazon/Flipkart/Myntra spends this month; (b) cashback auto-credited to Amazon Pay balance — more useful for an Amazon-shopper

- Non-Prime: Amazon Pay ICICI = ₹780 (3%); SBI = ₹1,300 (5%); SBI wins. **Prime-status materially flips winner.**

## Confidence + freshness
- HIGH confidence + fresh (both verified today)

## Conversational
- Narration: "Use **Amazon Pay ICICI if you're an Amazon Prime member** — 5% on Amazon = ₹1,300, same as SBI Cashback but **doesn't eat your SBI Cashback monthly cap** (₹2,000 cashback / ₹40K spend cap) which you might want for other online shopping this month. Cashback auto-credits to your Amazon Pay balance. **If you're not Prime, use SBI Cashback** — Amazon Pay ICICI drops to 3% for non-Prime, so SBI's 5% wins. Watch for BBD-style instant discounts on Amazon — if a bank-offer instant discount of ≥₹1,300 is live, take it instead (SUPPRESSES points/cashback path)."

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ Cap-headroom optimization (preserve SBI cap for the month)
- ✓ Prime-status branching (D8 from P1.1 surfaces again)
- ✓ Currency-utility consideration (Amazon Pay balance > statement credit for Amazon-shopper)
- ⚠ G1 — Prime-status branching = D8 confirmed 2nd time → strengthens R58/CV72 proposal
- ⛔ None

## Bottlenecks + deeper-dives
- D8 (Prime-membership state) CONFIRMED 2nd time — promote to TIER-1
- D14 (NEW) — Currency-utility framing ("auto-credit to Amazon balance" > "manual statement credit") — narrator could surface this proactively. Already partially in R51 statement_credit_rate but not in narration.

## Refinements engaged + NEW
- Existing: R49, R50, R51, R55, DA1, DA4, E1, E5, E6, CV1
- NEW: **D8 CONFIRMED → R58 + CV72 elevated to TIER-1**

## 80/20 verdict
**SHIP-AS-IS engine answer + TIER-1 LAYER-IN for R58/CV72 (now 2× confirmed)**

## Phase-0 sources
- [1] SBI Cashback April 2026 devaluation — savesage + cardinsider (verified today)
- [2] Amazon Pay ICICI Prime/non-Prime — cardinsider + ICICI T&C (verified today)
