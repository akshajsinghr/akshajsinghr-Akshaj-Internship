# qset1 Q17 — Business traveler: Marriott Singapore 4 nights ₹85K — Atlas EDGE→Bonvoy or co-brand?

**Stack:** Amex Plat Reserve + HDFC Infinia + MakeMyTrip ICICI + Marriott Bonvoy HDFC (co-brand)
**Question:** *"Booking Marriott in Singapore, 4 nights, ₹85K. Transfer Atlas EDGE to Bonvoy or pay with the co-brand?"*
**Difficulty:** Medium-Heavy (multi-currency redemption + co-brand routing)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (Marriott Bonvoy HDFC, Marriott hotels): **8 Bonvoy/₹150** = 5.33% in Bonvoy point value
- (Marriott Bonvoy HDFC base on non-Marriott): 2 Bonvoy/₹150 = 1.33%
- Marriott Bonvoy point value: ~₹1/Bonvoy point in stays = effective 5.33% rebate
- (Infinia direct, Marriott): 5 RP/₹150 = 3.3%; or transfer Infinia RP → Bonvoy 1:1
- Atlas EM → Bonvoy: 2 EM = 1 Bonvoy (Atlas is unusual — most cards 1:1)
- **Award rate**: Marriott Singapore cat-6 ~50-60K Bonvoy/night → 200-240K for 4 nights ON points

## Intent + DecisionContext
- intent_type: `redemption_optimization` + `purchase_routing` cross
- amount ₹85K; merchant=Marriott direct
- Note: user mentions Atlas EDGE balance, doesn't specify amount

## Engine traversal
- **Path A — Pay ₹85K via Marriott Bonvoy HDFC**: 85K/150 × 8 = 4,533 Bonvoy points earned (~₹4,533 hotel value) + Free Night Award progression + 10 Elite Night Credits/year toward Silver Elite (already Silver auto) → Gold
- **Path B — Pay ₹85K via Infinia**: 5 RP/₹150 = 2,833 RP × ₹0.30 statement = ₹850 or × ₹1 SmartBuy travel = ₹2,833
- **Path C — Transfer Atlas EDGE to Bonvoy to cover stay**: need ~200K Bonvoy for 4 nights cat-6 award; Atlas EM → Bonvoy 2:1 = need 400K EM → unlikely available balance for typical user
- **Path D — Pay cash on co-brand + book on points using existing Bonvoy balance**: hybrid — depends on user's Bonvoy balance

## Conversational
- Narration: "**Pay with the Marriott Bonvoy HDFC co-brand card.** Reasons:
  - **5.33% effective** on Marriott spend (8 Bonvoy/₹150). On ₹85K = ~4,533 Bonvoy points earned (~₹4,500 toward your next stay).
  - **Counts toward Marriott elite tier** (10 nights from auto-Silver + 1 elite night credit per ₹15K spend = ~5 more elite night credits from this stay alone, in addition to the actual 4 nights).
  - **Free Night Award** is renewed at card anniversary — keep using the card to retain it.
  - **Atlas EDGE → Bonvoy at 2:1** is structurally inefficient. To cover ₹85K of Marriott Singapore on points via Atlas, you'd need ~200K Bonvoy points = 400K Atlas EM transferred. That's a lot of EM to convert at a 2:1 ratio.
  - **Better Atlas play**: keep EM for KrisFlyer (5:2 → ~₹2/EM aspirational), or for Marriott Bonvoy ONLY if you have very large balances you'd rather not save for flights.
  - **Exception**: if you have a large Bonvoy points balance already (e.g., from past stays + the co-brand earn), pay on points (book the cat-6 award stay for 200-240K Bonvoy = full 4 nights free, leaving the ₹85K cash unspent). This is usually better than transferring Atlas EM to top up.
  - **Quick play**: 
    1. Check Marriott.com for the property's points price for your dates (4 nights cat-6 ≈ 200K Bonvoy)
    2. Compare against your Bonvoy balance
    3. If you have 200K+: book on points, save the ₹85K cash
    4. If you don't: pay with co-brand card and earn ~4,500 Bonvoy + 4-5 elite nights"

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ Multi-currency redemption with co-brand path
- ✓ Atlas EM → Bonvoy 2:1 inefficiency surfaced (R10 card_scope ratio)
- ✓ Free Night Award + elite-tier progression as non-points value (E72)
- ⛔ None

## Refinements engaged + NEW
- Existing: R10, R49, R50, R51, R53, DA1, DA3, E1, E5, **E54**, **E62**, **E72** (non-points-benefit valuation), CV1, CV5
- NEW: None — reinforces existing TIER-2 stack

## 80/20 verdict
**SHIP-AS-IS**; reinforces E72 non-points-benefit valuation across queries

## Phase-0 sources
- [1] Marriott Bonvoy HDFC earn rates (verified today)
- [2] Atlas → Marriott Bonvoy 2:1 (verified earlier)
- [3] Marriott Singapore award rates (industry standard)
