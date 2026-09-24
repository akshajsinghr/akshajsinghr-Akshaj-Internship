# qset1 Q16 — Business traveler: BLR-SIN-BLR ₹95K — burn MR or pay cash + earn?

**Stack:** Amex Plat Reserve + HDFC Infinia + MakeMyTrip ICICI + Marriott Bonvoy Yes
**Question:** *"BLR–SIN–BLR for work next week, ₹95K flight. Burn Amex MR or pay cash and earn?"*
**Difficulty:** Medium-Heavy (burn-vs-earn redemption_optimization)
**Run date:** 2026-05-24

## Phase-0 retrievals
- ₹95K BLR-SIN-BLR is approximately economy fare; could be biz
- **Amex MR → KrisFlyer (Star Alliance)**: typically 1:1; BLR-SIN saver economy round-trip on Singapore Air ~50-60K KF; biz ~120-150K KF
- (Amex Plat Reserve, paying cash): 1 MR/₹50 = 1,900 MR; value ~₹950 conservative or ₹1,900 aspirational
- (Infinia direct flight booking): 5 RP/₹150 × 95K = 3,167 RP × ₹1/RP = ₹3,167 SmartBuy travel
- (MMT ICICI on MMT booking): 3 myCash/₹100 on flights = 2,850 myCash = ₹2,850 (1:1 redeemable on MMT)
- (Marriott Bonvoy Yes — likely co-brand): probably 2-4 Bonvoy/₹150 base; less compelling for non-Marriott

## Intent + DecisionContext
- intent_type: `travel_routing` + `redemption_optimization` burn-vs-earn fork
- amount ₹95K; merchant=direct airline or MMT
- **Clarifying-Q (1):** "Is this economy or business class? And do you have ~50-60K (econ) / 120-150K (biz) Amex MR balance?"

## Engine traversal (burn-vs-earn)
**Burn route (assuming economy + sufficient MR balance):**
- Burn 50-60K MR (worth ~₹25K-30K conservative, up to ₹60K aspirational via KrisFlyer)
- Cost: 50-60K MR + taxes/surcharges (~₹6-10K) on ticket
- vs paying ₹95K cash and earning ~₹1,900-3,167 RP/MR
- **Burn wins clearly for economy**: you save ₹85K of cash for ~₹30K of points value AND avoid taxes on cash route

**Pay-cash-on-Infinia path:**
- 95K cash + 3,167 RP earned (~₹3,167 future redemption)
- Net cost: ₹95K cash − ₹3K future value ≈ ₹92K real cost

**Pay-cash-on-MMT-ICICI path:**
- 95K cash + 2,850 myCash earned (~₹2,850 MMT credit)
- Net cost similar

**Burn-MR-on-KrisFlyer:**
- 50K-60K MR + ~₹6-10K taxes  
- MR value @ ₹1/MR aspirational = ₹50K-60K
- Effective cost: ₹6-10K (taxes only) — saves ₹85K cash

## Conversational
- Narration: "**If economy and you have ~50-60K MR balance: burn the MR.** Here's why:
  - **Burn route**: Amex MR → KrisFlyer at 1:1 → 50-60K KF for BLR-SIN economy round-trip Saver. You pay only the taxes (~₹6-10K) out of pocket. Your MR balance had ~₹1/MR aspirational value, so you're 'spending' ~₹50-60K of MR for a flight that would cost ₹95K cash. Net savings: ~₹85K in cash, minus ~₹50-60K of MR opportunity cost = **~₹30-35K real value captured**.
  - **Pay-cash route on Infinia**: ₹95K out-of-pocket, earn 3,167 RP (₹3K future value). Net cost ~₹92K.
  - **Pay-cash on MMT ICICI**: similar to Infinia (~₹2,850 myCash earned).
  - **Decision math**: burn ≈ ₹35K cash saved net of opportunity cost; pay-cash ≈ ₹3K of earn. **Burn wins by ~10×**.
  
  **EXCEPT if:**
  - **This is business class** (~120-150K KF needed) — check your MR balance.
  - **You're saving MR for a higher-value redemption** like trans-Atlantic biz (where 1 MR = ₹1.50+) — don't burn for BLR-SIN economy in that case.
  - **You're chasing an Infinia milestone** where the ₹95K spend matters for the ₹10L renewal-fee waiver.
  - **Award space isn't available** on the dates you need — sometimes KF Saver inventory is thin BLR-SIN.
  
  **Quick check**: log into KrisFlyer, search BLR-SIN Saver biz/economy for your dates. If available, burn. If not, pay cash on Infinia."

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ Burn-vs-earn computation (E62 rank-flip sensitivity around MR valuation)
- ✓ MR opportunity-cost surfaced (₹1/MR aspirational vs ₹0.50 conservative)
- ✓ Award availability caveat
- ⚠ KrisFlyer award availability is acquisition-side (DA66 AwardAvailabilityTimeSeries — proposed; PENDING)

## Refinements engaged + NEW
- Existing: R10, R49, R50, R51, R53, DA1, DA3, **DA66 PENDING**, E1, E5, **E54** (multi-currency), **E62** (rank-flip), CV1, CV5, **CV70**
- NEW: D30 — **burn-vs-earn as recurring intent** — should be its own intent class or sub-intent of redemption_optimization. **Proposed CV78: burn-vs-earn explicit narration template.**

## 80/20 verdict
**SHIP-AS-IS engine answer; CV78 is TIER-2**

## Phase-0 sources
- [1] Amex MR → KrisFlyer 1:1 (industry standard verified earlier)
- [2] KrisFlyer Star award chart Asia regional (verified earlier)
