# qset1 Q8 — Family: ₹18K home insurance — pay in one shot or split across cards?

**Stack:** HDFC Regalia Gold + Axis Flipkart + SBI Cashback
**Question:** *"Annual home insurance ₹18K. Pay in one shot or split across cards?"*
**Difficulty:** Medium (insurance D5 cross-validation, 2nd time + cap-aware split)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (Regalia Gold, insurance): typically standard base earn 4 RP/₹150 with potential caps; HDFC has been adding caps; Regalia Gold not as restrictive as Infinia historically but verify
- (Axis Flipkart, insurance): **EXCLUDED** per its broad exclusion list (insurance, jewelry, utilities, education explicitly excluded)
- (SBI Cashback, insurance): **EXCLUDED** April 2026 expansion
- Splitting across multiple cards doesn't help when most cards exclude

## Intent + DecisionContext
- intent_type: `purchase_routing` + `stacking_opportunity` (split across cards)
- amount ₹18K; category=insurance
- No clarifying-Q

## Engine traversal
- Path A — All on Regalia Gold: 18K × 2.67% RP = 480 RP × ₹1/RP SmartBuy = ₹480 or × ₹0.30 = ₹144
- Path B — All on Flipkart: 0 (excluded)
- Path C — All on SBI Cashback: 0 (excluded)
- Path D — Split: only Regalia Gold earns; splitting doesn't help
- Winner: **All on Regalia Gold** for the base earn

## Conversational
- Narration: "**Pay it all on Regalia Gold — splitting doesn't help.**
  - **Axis Flipkart**: insurance is in their exclusion list → ₹0 earn.
  - **SBI Cashback**: insurance was added to their April 2026 expanded exclusion list → ₹0 earn.
  - **Regalia Gold**: insurance still earns at base 4 RP/₹150 → ~480 RP on ₹18K. Modest but not zero.
  - **RP value**: 480 RP = ~₹480 if redeemed via SmartBuy travel; ~₹144 if statement credit.
  - **Verify Regalia Gold insurance status before paying** — HDFC has been tightening caps on insurance category over the past 2 years (e.g., Infinia got a 10K RP/mo cap from July 2025). Regalia Gold may have similar caps; check the latest HDFC T&C or your last statement to see if insurance is still earning normally.
  - **Splitting doesn't unlock more value** since the other 2 cards earn nothing. The whole ₹18K should go on Regalia Gold.
  - **For comparison**: Amazon Pay ICICI (not in your stack) earns 2% on insurance = ₹360. If you ever add that card, you'd have a slightly better insurance route."

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ D5 insurance cross-validation (now 2nd insurance datapoint after Q4.2 — but Q4.2 was for premium-stack, this is mid-tier)
- ✓ "Splitting doesn't help" framing — explicit user-intuition check
- ✓ Stack-upgrade suggestion ("if you add Amazon Pay ICICI...") — E70 pattern

## Refinements engaged + NEW
- Existing: R49, R50, R51, R56, DA1, DA24, E1, E5, E15, **E70** (stack-upgrade), CV1, CV5
- NEW: None

## 80/20 verdict
**SHIP-AS-IS engine answer**; D5 strongly confirms what we already proposed

## Phase-0 sources
- [1] HDFC Regalia Gold T&C (verified earlier)
- [2] Axis Flipkart exclusions (verified earlier)
- [3] SBI Cashback April 2026 exclusions (verified earlier)
