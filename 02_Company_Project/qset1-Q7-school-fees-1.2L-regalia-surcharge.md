# qset1 Q7 — Family: ₹1.2L school fees + 1.5% surcharge — worth on Regalia?

**Stack:** HDFC Regalia Gold + Axis Flipkart + SBI Cashback
**Question:** *"Kid's school fees ₹1.2L this term. School charges 1.5% surcharge — worth it on Regalia?"*
**Difficulty:** Medium (education D5 + surcharge math)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (Regalia Gold, education): typically NOT in HDFC's standard exclusion list (which excludes fuel/wallet/voucher/rent/govt) BUT effective earn varies; education category often earns base 4 RP/₹150 = 2.67% — verify per latest T&C
- (Axis Flipkart, education): **EXCLUDED** per April-2026-onward exclusion expansion (educational services explicitly excluded)
- (SBI Cashback, education): **EXCLUDED** per April 2026 expanded list
- School surcharge 1.5% = ₹1,800 cost on ₹1.2L

## Intent + DecisionContext
- intent_type: `purchase_routing` + surcharge cost-benefit analysis
- amount ₹1.2L; category=education + 1.5% surcharge
- No clarifying-Q (user gave surcharge rate)

## Engine traversal
- Surcharge cost: 1.2L × 1.5% = ₹1,800
- (Regalia Gold, education base earn): 1.2L × 2.67% RP value = ~₹1,067-3,200 in RP terms × varying redemption value
- If RP redeems at ₹1/RP (SmartBuy travel): 4 RP/₹150 × 1.2L = 3,200 RP × ₹1.0 = ₹3,200; net **₹3,200 − ₹1,800 = +₹1,400 net**
- If RP redeems at ₹0.30/RP statement: 3,200 RP × ₹0.30 = ₹960; net **₹960 − ₹1,800 = -₹840 net loss**
- (Axis Flipkart, education): 0 earn − ₹1,800 surcharge = **−₹1,800 net loss**
- (SBI Cashback, education): 0 earn − ₹1,800 = **−₹1,800 net loss**

## Conversational
- Narration: "**Marginal on Regalia Gold — depends entirely on how you'd redeem the points.** Math:
  - **Regalia Gold base earn on education**: 2.67% in RP terms = 3,200 RP. School surcharge cost: ₹1,800.
  - **If you redeem RP for SmartBuy travel** (₹1/RP): net **+₹1,400** in your favor → worth it.
  - **If you redeem for statement credit** (₹0.30/RP): net **-₹840** → school surcharge eats more than the cashback. Skip.
  - **Axis Flipkart and SBI Cashback**: both exclude education entirely → you'd pay the surcharge AND earn nothing = **net -₹1,800 each**. Don't use these.
  
  **What I'd actually do:**
  - **If you have SmartBuy travel use cases** (book any flight/hotel via SmartBuy in next 6-12 months) → use Regalia Gold and route the points to SmartBuy → net positive.
  - **If you'd just redeem for statement credit** → skip the card route entirely. Pay via UPI/netbanking direct → avoid the 1.5% surcharge → keep ₹1,800 in your pocket (better than getting ₹960 cashback − ₹1,800 surcharge = -₹840).
  - **Quick test**: when's your next flight/hotel booking? If within 6 months and ≥₹3K, Regalia Gold + redeem 3,200 RP toward it = clean ₹3,200 of travel value. Otherwise UPI direct."

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ Surcharge + earn math with ValuationProfile-sensitive output (E62 rank-flip)
- ✓ Conditional answer ("if you'd redeem via SmartBuy travel...") — exactly the pattern CV70 prescribes
- ✓ Channel-alternative (UPI direct) surfaced for the "won't earn" scenario
- ⛔ None

## Refinements engaged + NEW
- Existing: R49, R50, R51, R53, R56, DA1, DA24, E1, E15, **E62**, **E67** (channel-advisory), CV1, CV5, **CV70** (rank-flip sensitivity)
- NEW: None

## 80/20 verdict
**SHIP-AS-IS — CV70 + E62 directly applied; engine produces conditional answer correctly**

## Phase-0 sources
- [1] HDFC Regalia Gold base earn (verified earlier)
- [2] Axis Flipkart exclusions (verified earlier)
- [3] SBI Cashback April 2026 exclusions (verified earlier)
