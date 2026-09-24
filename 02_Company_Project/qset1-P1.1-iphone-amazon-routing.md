# qset1 P1.1 — First-time card holder: ₹42,800 iPhone Amazon BBD-style routing

**Stack:** HDFC Millennia + Amazon Pay ICICI + RuPay virtual UPI
**Question:** *"I'm paying ₹42,800 on Amazon for a new iPhone during sale — should I use Millennia, Amazon Pay ICICI, or split it somehow?"*
**Difficulty:** Easy structurally; Medium pedagogically (first-time user needs simple framing)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (Millennia, Amazon online): **EARNS_AT 5% capped ₹1,000 CashPoints/mo** (Source: HDFC; verified today) — ₹1,000 CashPoints = ₹1,000 redeem value (1:1 statement)
- (Amazon Pay ICICI, Amazon Prime): **EARNS_AT 5% Prime / 3% non-Prime, no cap** (Source: ICICI + cardinsider; verified today)
- (RuPay UPI, Amazon): N/A — Amazon doesn't accept UPI for ₹42K iPhone (gateway uses credit card)
- BBD-style instant discount on iPhone: typically ICICI/Axis/HDFC bank offers; SUPPRESSES points fork applies

## Intent + DecisionContext
- intent_type: `purchase_routing` (0.92); secondary `stacking_opportunity` (0.50) — implicit "or split?"
- amount ₹42,800; merchant=amazon.in; portfolio = 3 cards
- Prime-status unknown → **Clarifying-Q (1, simple):** *"Are you an Amazon Prime member? Affects whether Amazon Pay ICICI earns 5% or 3%."*
- Assume Prime for analysis (most reward-curious users have Prime)

## Engine traversal
- Query type: purchase_routing
- Hard problems: #1 caps (Millennia ₹1,000/mo cap is binding here!); #3 stacking (BBD instant discount fork)
- Paths:
  | Path | Math | NetValue |
  |---|---|---|
  | **Amazon Pay ICICI (Prime, full ₹42.8K)** | 42800 × 5% = ₹2,140 cashback, no cap | **₹2,140** |
  | Millennia full | 42800 × 5% = ₹2,140 BUT capped at ₹1,000/mo CashPoints | ₹1,000 |
  | Split (₹20K Millennia + ₹22.8K Amazon Pay ICICI) | (20000×5%=₹1,000 cap) + (22800×5%=₹1,140) | ₹2,140 |
  | If BBD instant discount active (≥₹3K), discount branch | Up to ₹3,000–4,000 instant — SUPPRESSES points | Variable, often wins |
- Winner: **Amazon Pay ICICI** (clean ₹2,140) unless an instant discount ≥₹2,500 is live → then discount wins via SUPPRESSES
- runner_up_loss_reason: "Millennia 5% capped at ₹1,000/mo. Splitting doesn't unlock more — Amazon Pay ICICI clears the cap by itself"

## Confidence + freshness
- All paths Tier 1.1 issuer + Tier 2 aggregator (verified today). HIGH confidence + fresh.
- ⚠ BBD instant discount status is sale-period-sensitive (DA4) — should be re-checked at purchase time

## Conversational
- t=0 USER → t=180ms preamble → t=400ms clarifying-Q (Prime?) → user "Yes Prime" → t'=600ms engine → t'=1.4s narration begin → t'=2.6s complete
- Narration: "Use **Amazon Pay ICICI** (assuming you're Prime). 5% = ₹2,140 cashback, no cap. Millennia would also give 5% but it caps at ₹1,000/mo so you'd leave ₹1,140 on the table. **Important:** if Amazon shows a bank-offer instant discount of ≥₹2,500, that beats the cashback — go with that and accept zero rewards. Always check sale-page before checking out."
- Citations: [1] ICICI Amazon Pay T&C; [2] HDFC Millennia T&C

## ✓ Works / ⚠ Gaps / ⛔ Blockers
- ✓ Cap-aware routing correctly identifies Millennia limitation
- ✓ SUPPRESSES fork (instant discount vs points) surfaced
- ⚠ G1 — sale-period instant-discount status requires DA4 freshness ≤1hr; engine can only caveat at v0
- ⛔ None

## Bottlenecks + deeper-dives
- B1 — Prime/non-Prime branching adds a clarifying-Q (acceptable since materially changes ₹2,140 → ₹1,284). **TIER-1: surface Prime-status as a one-time onboarding question, persist on user profile.**
- D8 (NEW) — **User-profile attribute "is_Prime_member" (and analogous platform-memberships) as first-class persistable state**, so this Q never needs re-asking. Proposed CV72 + R58 (User entity property).

## Refinements engaged + NEW
- Existing: R49, R50, R51, DA1, DA4, DA10, E1, E5, E6, E15, CV1, CV5
- NEW: **CV72** narrator persists declarable platform-membership state (Amazon Prime, Flipkart Plus, Swiggy One, Zomato Gold/Pro); **R58** User.platform_memberships overlay

## 80/20 verdict
**TIER-1 LAYER-IN** — pipeline answers correctly today with one clarifying-Q. The Prime-membership pattern recurs across many queries (Q3 SBI+Amazon, Q19 implicit, Q5 Swiggy/Zomato — Swiggy One status matters). Adding R58 + CV72 is small effort, high recurring value.

## Phase-0 sources
- [1] HDFC Millennia T&C — paisabazaar.com (verified 2026-05-24)
- [2] Amazon Pay ICICI rates — cardinsider.com (verified 2026-05-24)
- [3] Sale-period instant-discount pattern — DA4 acquisition-plan
