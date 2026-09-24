# qset1 P1.3 — First-time: ₹18,500 college fees payment gateway

**Stack:** HDFC Millennia + Amazon Pay ICICI + RuPay virtual UPI
**Question:** *"I have to pay ₹18,500 college fees on a payment gateway tomorrow — any card worth using here or do most of these exclude rewards?"*
**Difficulty:** Easy (exclusion-category pattern — third D5 datapoint)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (Millennia, education via payment gateway): **EXCLUDED** — payment gateway transactions for education typically classified as excluded category by HDFC; even where MCC accepts, gateway-fee 1% surcharge eats reward
- (Amazon Pay ICICI, education): **EXCLUDED** from cashback per ICICI exclusion list (education explicitly excluded for non-Amazon merchants)
- (RuPay UPI, education): **UPI on credit excluded from rewards on most issuers**; RuPay-credit on UPI category-rate matrix typically excludes education
- **Surcharge:** payment gateways (CRED, Paytm, Mobikwik for school fees) typically levy 1% surcharge from Feb 2026
- **Workaround:** paying directly via school's official website + bank BillPay typically avoids surcharge; some cards (Axis ACE via GPay BillPay = 5%) earn on direct school-website route — but user doesn't have ACE

## Intent + DecisionContext
- intent_type: `purchase_routing` (0.85) + implicit `EXPLAINER` "or do most exclude rewards?" (0.55)
- amount ₹18,500; category=education-payment-gateway
- No clarifying-Q needed (user already framed the expectation correctly)

## Engine traversal
- Query type: purchase_routing
- Hard problems: #6 confidence/freshness (margin guards inapplicable; all zero earn)
- Paths:
  | Path | Math | NetValue |
  |---|---|---|
  | Millennia via payment gateway | 0 (education excluded + 1% surcharge ≈ -₹185) | **-₹185** |
  | Amazon Pay ICICI via gateway | 0 (excluded) | ₹0 |
  | RuPay UPI on credit | 0 (excluded) | ₹0 |
  | **Direct school website + BillPay (any card)** | base rate may apply per-card; surcharge avoided | ~₹0 to small positive |
- Winner: **Pay directly on school website** to avoid surcharge; use Amazon Pay ICICI (no annual fee, no surcharge cost)
- runner_up_loss_reason: "All cards exclude education-via-payment-gateway. Surcharge of 1% via CRED/Paytm/Mobikwik makes that path net-negative."

## Confidence + freshness
- HIGH confidence + fresh (recent industry-wide pattern)

## Conversational
- Narration: "**You're right — most cards exclude education on payment gateways**, and gateways typically charge a 1% surcharge on top, which makes the card route actually a net cost. **Best move: pay directly on your college's official website if they accept direct card payments** (or via your bank's BillPay if listed). You'll skip the surcharge and a few cards still earn small base rewards via direct payment. For these 3 cards, education is excluded on all three regardless of channel — so just optimize for skipping the surcharge."

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ Cross-validates D5 missing-exclusion pattern (3rd datapoint after Q4.2 insurance + Q19 jewelry)
- ✓ Surfaces surcharge as separate cost from earn (engine §3 objective function captures this)
- ⚠ G1 — channel-routing advice (direct website vs gateway) is hand-curated narration; not engine-derived. Could become **E66 channel-aware routing advisory** for excluded categories
- ⛔ None

## Bottlenecks + deeper-dives
- D5 CONFIRMED 3rd time (insurance + jewelry + education) — apply NOW
- D11 (NEW) — Channel-routing for excluded categories (direct site vs gateway). Recommendation engine should suggest channel changes when categorical earn is blocked. **Proposed E67 channel-advisory.**

## Refinements engaged + NEW
- Existing: R49, R50, R51, DA1, DA11 (MCC), E1, E15, CV1
- NEW: **E67** channel-advisory for excluded categories (paired with E65 workaround-discovery from Q19)

## 80/20 verdict
**SHIP-AS-IS engine answer + TIER-1 D5-cluster application now strongly justified (3rd cross-query confirmation)**

## Phase-0 sources
- [1] Education-fee surcharge patterns India — savesage / cardmaven (verified today)
- [2] Amazon Pay ICICI exclusions — ICICI T&C (verified today)
- [3] Millennia exclusion list — HDFC MCC Exclusion PDF (verified today via Q19 retrieval)
