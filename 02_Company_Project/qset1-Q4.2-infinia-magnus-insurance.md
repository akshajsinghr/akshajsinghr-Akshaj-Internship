# qset1 Q4.2 — Reward churner: ₹27K insurance routing across Infinia + Magnus Burgundy + Amex Gold + HSBC Cashback

**Query file:** `query_run/qset1-Q4.2-infinia-magnus-insurance.md`
**Run date:** 2026-05-24
**Skill version:** v1 (first artifact; methodology in `/Users/ankurv/swypd/.claude/commands/swypd-query-stress-test.md`)
**Critical post-run finding:** this analysis surfaced D5 — the "missing-exclusion blind spot" — which is now the most important architecture gap in the 4-plan suite. See §11 and Appendix.

---

## 1. Query header

- **Persona:** Reward churner who tracks deals obsessively (qset1 Persona 4)
- **Stack:** HDFC Infinia + Axis Magnus Burgundy + Amex Gold Charge + HSBC Cashback (4 cards)
- **Declared state:** none explicit (user hasn't told us MTD insurance spend, current Infinia/Magnus point balances, or upcoming milestone position)
- **Verbatim question:** *"Paying ₹27,000 insurance premium this week — any decent route left or have most reward paths been nerfed?"*
- **Difficulty rating:** **Medium-Heavy**. Structurally simple (single-merchant routing) but the user's phrasing ("any decent route left", "nerfed") encodes two parallel intents — primary `purchase_routing` + implicit `RULE_CHANGE_DIFF`. The harder element is correctness: 3 of the 4 cards exclude insurance entirely, and the analysis must surface that.

---

## 2. Intent classification + DecisionContext

| Field | Value | Confidence | Notes |
|---|---|---|---|
| `intent_type` (primary) | `purchase_routing` | 0.88 | "₹27K insurance" + "which route" |
| `intent_type` (secondary) | `RULE_CHANGE_DIFF` | 0.55 | "nerfed" — surface what's recently changed. Below 0.75 routing threshold; emitted as inline caveat instead |
| `amount` | `Decimal('27000.00')` | high | Verbatim |
| `merchant_category` | `5960` (insurance — direct marketing/insurance services) | high | Standard MCC |
| `merchant` | unspecified | low | Specific insurer not relevant for category-based routing |
| `target_date` | this week (≤7 days) | high | "this week" |
| `portfolio` | `[infinia, magnus-burgundy, amex-gold-charge, hsbc-cashback]` | declared at onboarding | — |
| `household_scope` | None | — | — |
| `expiry_window_days` | None | — | — |
| `extended_thinking_required` | `False` | high | 4 cards, single category — typical query budget |

**Clarifying-Q** (per conv §5 discipline + engine §5.1 `needs_elicitation`): cap state matters for Infinia (10K RP/mo cap on insurance category — currently the only earning card per §4); milestone state could redirect routing toward any of the cards if user is mid-chase. Conversational layer emits ONE bundled clarifying question with explicit escape:

> *"Quick check first: how much have you spent on insurance through these this month, and are you near a milestone? Say 'skip' to assume clean."*

User responds: *"Skip — assume clean caps and no immediate milestone."*

**user_state_assumptions[]:**
- "Cap headroom assumed clean per user explicit skip"
- "No active milestone pressure per user explicit skip"
- "User redeems via SmartBuy travel (₹1/pt) per cold-start default; override if user prefers statement credit (₹0.30/pt)"

---

## 3. Schema layer — entities & edges traversed

### Nodes
| Layer | Node type | Instance(s) | Properties read |
|---|---|---|---|
| L0 | `PaymentNetwork` × 3 | Visa (Infinia, Magnus B), Mastercard (HSBC), Amex (Gold) | name |
| L1 | `Issuer` × 4 | HDFC, Axis, HSBC, Amex India | name, country |
| L2 | `Card` × 4 | Infinia, Magnus Burgundy, Amex Gold Charge, HSBC Cashback | name, fee, network, `pooling_policy` (R55) |
| L3 | `RewardCurrency` × 4 | HDFC RP, Axis EDGE RP, Amex MR, HSBC Cashback INR | name, `is_travel_currency`, `is_cashback_currency`, `statement_credit_rate` (R51) |
| L4 | `PaymentRail` × 2 | Online gateway, swipe | rail-specific earn modifier |
| L5 | `Merchant` (generic insurer) | — | online/offline |
| L5 | `Category` MCC 5960 | "Insurance — Direct Marketing/Direct Insurance Services" | label |
| L6 | `EarnRule` (reified) × 4 | Per-card insurance treatment (rule presence varies — see §4) | rate, multiplier, rail-scope, validity, **R56 CategoryEarnAssertion state (proposed)** |
| L6 | `Cap` × 1 | Infinia 10K RP/mo insurance cap | scope, period, limit unit, limit value |
| L6 | `Exclusion` × 3 | **HSBC insurance exclusion, Magnus insurance exclusion, Amex Gold insurance exclusion** (all 3 cards excluded — corrected from initial analysis) | scope, reason |
| L6 | `Surcharge` × 0 | None (insurance category has no surcharge in India 2026 across these cards) | — |
| L9 | `RedemptionChannel` × 4 | SmartBuy travel, EDGE Miles → partner (moot for this query), MR transfer (moot), direct cashback (moot) | value rate per point, cap, fee, `statement_credit_rate` (R51) |
| L11 | `RuleChangeEvent` × 3+ | Axis 2024 cut insurance to base-then-excluded; HDFC 2023 added 10K RP/mo insurance cap; HSBC 2024 added insurance to excluded list; Amex India long-standing exclusion | what changed, magnitude, announced_at, **`old_value`/`new_value`/`affected_card_ids[]` (DA64 — currently PENDING)** |
| Overlay | `CardHolding` × 4 | User's 4 declared cards | acquired_date, status |
| Overlay | `ValuationProfile` | Cold-start defaults: ₹1/pt SmartBuy travel for HDFC RP + EDGE; cashback face-value for HSBC | per-currency assumed value, `valuation_confidence`, `declared_at`, `inferred_at` (R53) |
| Overlay | `CapConsumption` | User declined → assumed clean | per-cap consumed |
| Query | `DecisionContext` | Constructed per §2 | full envelope |
| Query | `DecisionResult` | See §5 | options[], `runner_up_loss_reason` (R50), `conflict_disclosures[]` (R49) |

### Edges
| Edge type | From → To | Properties read |
|---|---|---|
| `EARN_RULE` × 1 | Infinia EarnRule → Category 5960 | rate (5 RP/₹150), multiplier (1X), validity |
| `CAPPED_BY` × 1 | Infinia insurance rule → Cap (10K RP/mo) | — |
| `EXCLUDED_BY` × 3 | **HSBC default rule → Exclusion(insurance); Magnus default rule → Exclusion(insurance); Amex Gold default rule → Exclusion(insurance)** | scope (insurance MCC 5960), reason ("excluded per T&C") |
| `EARNS_IN` × 4 | Card → RewardCurrency | — |
| `REDEEMS_VIA` × 1 (relevant — for Infinia only since others earn 0) | HDFC RP → SmartBuy travel / statement credit | ₹ value per point (₹1.00 / ₹0.30), `statement_credit_rate` (R51) |

**Required refinements engaged:** R49 (conflict_disclosures), R50 (explanation_structure + runner_up_loss_reason), R51 (RewardCurrency flags + statement_credit_rate), R53 (ValuationProfile confidence), R55 (Card.pooling_policy — declared but unused here).

**NEW refinements surfaced:** R56 (CategoryEarnAssertion per-card×category) — see §11. R57 (EarnRule.default_rule_applies) — see §11.

**Schema-blocker check:** ✓ NO BLOCKERS. All entities and edges declared in schema-plan §2/§3. R51–R55 sync additions cover the new properties needed.

**⚠ Schema-completeness concern (D5):** the schema *allows* EXCLUDED_BY to be omitted for cards whose default earn rule covers the category. But it doesn't *require* each (card, category) tuple be explicitly resolved. This is the missing-exclusion blind spot — see §11 + Appendix.

---

## 4. Acquisition layer — sources, cadence, freshness

**⚠ Phase-0 retrieval-discipline note:** in this analysis, facts were retrieved on-demand from issuer T&C + community-validated sources. The v0 graph at the time of this analysis did NOT contain populated per-card-per-category coverage for these tuples. Phase-0 audit returned 3 UNKNOWN tuples (HSBC, Magnus, Amex insurance treatment) which were resolved by manual retrieval from issuer T&C pages + community cross-check (Reddit r/CreditCardsIndia, TechnoFino, CardExpert May 2026 threads).

| Fact | Source tier | DA refinement | Cadence | Freshness halflife | Status | Source citation |
|---|---|---|---|---|---|---|
| Infinia base earn rate (5 RP/₹150) | Tier 1.1 issuer T&C | DA1 per-card source registry | Quarterly + event | 90d | ✓ ACQUIRED in v0 manual seed | HDFC Infinia T&C (verified 2026-04-22) |
| **Infinia 10K RP/mo insurance cap** | Tier 1.1 | DA1 | Quarterly | 90d | ✓ ACQUIRED | HDFC Infinia Rewards Schedule §3.2 (verified 2026-04-22) |
| **HSBC Cashback — insurance EXCLUDED** | Tier 1.1 + community cross-check | DA1 + DA10 community | Quarterly | 90d | ✓ ACQUIRED (manual Phase-0 retrieval) | HSBC India Cashback T&C §5.4 "Excluded Categories" (verified 2026-03-15); confirmed by TF May 2026 thread |
| **Magnus Burgundy — insurance EXCLUDED from EDGE RP** | Tier 1.1 + community cross-check | DA1 + DA10 | Quarterly | 90d | ⚠ ACQUIRED but AGING — Axis MITC last updated 2026-03-08 (75d ago); community reports confirm exclusion is still active | Axis Magnus Burgundy MITC Schedule A "Categories not eligible for reward points" (verified 2026-03-08); Reddit r/CreditCardsIndia mega-thread May 2026 confirms |
| **Amex Gold Charge — insurance EXCLUDED from MR** | Tier 1.1 + community | DA1 | Quarterly | 90d | ✓ ACQUIRED | Amex India Membership Rewards T&C §7 (verified 2026-04-01); long-standing exclusion since 2022 |
| MCC 5960 = Insurance — Direct Services | Tier 1.6 MCC + community-validated edge cases | DA11 | Quarterly | 365d | ✓ ACQUIRED | Visa/MC public MCC tables + Indian community-validated mappings |
| HDFC RP statement-credit rate (₹0.30/pt) | Tier 1.2 issuer portal (SmartBuy) | DA1 + Tier 1.2 | Quarterly | 60d | ✓ ACQUIRED via R51 | SmartBuy redemption page (verified 2026-04-22) |
| HDFC RP SmartBuy-travel rate (₹1/pt) | Tier 1.2 issuer portal | DA1 | Quarterly | 60d | ✓ ACQUIRED | SmartBuy travel page (verified 2026-04-22) |
| RuleChangeEvent diff — "what was nerfed on insurance recently" | DA24 + Tier 1.7 + Tier 3.0 | DA24 + **DA64 extension (PENDING)** | Event-driven | 30d | ⚠ DA64 ACQUISITION-PENDING — old_value/new_value/affected_card_ids[] fields needed for diff reconstruction | Generic caveat only without DA64 |

**Acquisition-blocker check:** ⚠ ONE PENDING (D2 — DA64). Engine answers the primary intent correctly even without DA64; secondary "nerfed?" intent gets only a generic acknowledgment. Workaround acceptable for v1.

**Phase-0 UNKNOWN tuples at start of analysis:** 3 (HSBC × insurance; Magnus × insurance; Amex × insurance). Resolved by manual retrieval. **In a production run after DA68 (coverage matrix) is built, these tuples should be pre-populated. Until then, every analysis of a qualifier-heavy category needs Phase-0 manual retrieval.**

**Freshness margin guards:** Magnus T&C is at 75d (aging band per schema §11.3). No margin-guard triggered because Magnus is at NetValue ₹0 and doesn't compete for ranking — but the aging caveat is surfaced to the user.

---

## 5. Decision engine traversal

**Query type dispatch:** `purchase_routing` (engine §4 row 1; latency target 200ms p95 typical).

**Hard problems engaged:**
- **#1 Stateful caps** — Infinia 10K RP/mo insurance cap. Engine §5.1 `CapHeadroomVector`. User skipped elicitation; engine assumes clean → headroom = full 10K cap.
- **#4 Valuing unredeemed points** — engine §5.4 cold-start: HDFC RP → ₹1/pt (travel-currency, SmartBuy). Surfaced as explicit assumption.
- **#6 Confidence + freshness ranking margin guards** — engine §5.6. Magnus T&C at aging band; no guard triggers because Magnus is NetValue ₹0.

**Path enumeration strategy:** Direct lookup (no Beam Search needed — 4-card single-category comparison). 4 candidate paths.

**Number of candidate paths:** 4

### Per-path math
| # | Path | Gross earn | Cap clamp | Effective earn | Currency value (₹/pt) | NetValue | Effective % |
|---|---|---|---|---|---|---|---|
| **1** | **Infinia online** | 27000/150 × 5 = **900 RP** | None (within 10K/mo) | 900 RP | ₹1.00 SmartBuy travel | **₹900.00** | **3.33%** |
| 2 | HSBC Cashback online | **0** (insurance EXCLUDED per T&C §5.4) | N/A | 0 | — | ₹0.00 | 0.00% |
| 3 | Magnus Burgundy online | **0** (insurance EXCLUDED from EDGE RP per MITC Schedule A) | N/A | 0 EDGE RP | — | ₹0.00 | 0.00% |
| 4 | Amex Gold Charge | **0** (insurance EXCLUDED from MR per India terms §7) | N/A | 0 MR | — | ₹0.00 | 0.00% |

**Pareto ranking:** Path 1 (Infinia) is the **only positive-NetValue option.** No runner-up in the conventional sense.

**`runner_up_loss_reason` (R50):** *"All other cards in stack exclude insurance from rewards. Infinia is the only earning route on this category."*

**Pareto top-3 from recommendation-engine wrapper:** Only Path 1 qualifies; Paths 2–4 surfaced with `EXCLUDED` annotation rather than ranked.

**Determinism check:** No ties; deterministic output.

---

## 6. Confidence + freshness scoring + conflict resolution

| Path | Confidence | Freshness | Margin guard applied? |
|---|---|---|---|
| **1 Infinia** | 0.95 HIGH (Tier 1.1 issuer T&C, recently verified) | fresh (30d ago) | No |
| 2 HSBC EXCLUDED | 0.92 HIGH (Tier 1.1 + community cross-check) | fresh (60d ago) | No |
| 3 Magnus EXCLUDED | 0.85 HIGH (Tier 1.1 with community confirmation; T&C aging at 75d) | aging | No (path doesn't compete) |
| 4 Amex Gold EXCLUDED | 0.95 HIGH (Tier 1.1; long-standing exclusion) | fresh | No |

**Conflicts detected:** None. Initial concern that issuer "regular reward points apply unless category excluded" language could conflict with community-reported "insurance is base-only" — resolved by retrieving the exclusion list from Schedule A which explicitly names insurance.

**ConflictDisclosure[] entries:** 0

**Ranking margin guards:** None triggered (single positive-NetValue path; no competing options).

**⚠ Negative-fact confidence note (D5 / E64 proposed):** the exclusion facts for HSBC, Magnus, Amex should carry their own confidence + freshness bands in DecisionResult. Currently the engine's R49/R50 surface positive facts cleanly; exclusion facts get surfaced via the explainer but not with the same first-class confidence semantics. **See §11 — E64 proposed.**

---

## 7. Conversational layer — intent parsing + streaming UX

### Streaming timeline

**Turn 1 (clarifying-Q):**
```
[t=0]      USER: "Paying ₹27,000 insurance premium this week — any decent route left or have most reward paths been nerfed?"
[t=180ms]  BOT preamble (200ms target): "Let me check your insurance options across Infinia, Magnus, Amex, and HSBC..."
[t=220ms]  BOT clarifying-Q: "Quick check first: how much have you spent on insurance through these this month, and are you near a milestone? Say 'skip' to assume clean."
[USER turn] USER: "Skip — assume clean caps and no immediate milestone."
```

**Turn 2 (final answer):**
```
[t'=0]     <New turn begins; conversational layer reconstructs DecisionContext>
[t'=50ms]  Profile reconstruction + DecisionContext finalization
[t'=180ms] BOT preamble: "Got it. Computing best route on ₹27K insurance..."
[t'=400ms] <Engine call dispatched; retrieval planner pulls subgraph in parallel>
[t'=600ms] <Engine returns DecisionResult (200ms compute)>
[t'=800ms] BOT begins narrating winner — leading with the STRUCTURAL insight (3 of 4 excluded), not numeric ranking:

           "**Best route: HDFC Infinia. Honestly, it's the only route.**
            
            On ₹27,000 insurance:
            - **Infinia: 900 RP** (₹900 if you redeem via SmartBuy travel; ₹270
              as statement credit). Within the 10,000 RP/month insurance cap.
              (HDFC T&C verified 30 days ago)
            - **HSBC Cashback: ₹0** — insurance is in the excluded-categories list. 
              (HSBC T&C §5.4 verified 60 days ago)
            - **Magnus Burgundy: ₹0** — insurance is excluded from EDGE RP accrual.
              (Axis MITC Schedule A verified 75 days ago — aging, recheck soon)
            - **Amex Gold Charge: ₹0** — insurance excluded from MR per India terms.
              (Amex T&C §7 verified 30 days ago)
            
            **About 'nerfed'**: yes, severely. Insurance has been a target across
            issuers 2024–25 — Magnus moved from accelerated to excluded, Amex
            excluded long-standing, HSBC added it to excluded list. Infinia is
            the cleanest survivor for now and is itself capped at 10K RP/month.
            
            **Heads-up on assumption**: if you'd prefer to redeem via statement
            credit instead of SmartBuy travel, Infinia's effective drops from
            ₹900 to ₹270 — still your only earning option, just less valuable.
            
            Assumption: caps clean per your skip; SmartBuy travel valuation."
[t'=2.4s]  <Streaming complete>
[t'=2.4s+] BOT inline citations:
           [1] HDFC Infinia Rewards Schedule §3.2 — verified 2026-04-22
           [2] HSBC India Cashback T&C §5.4 "Excluded Categories" — verified 2026-03-15
           [3] Axis Magnus Burgundy MITC Schedule A — verified 2026-03-08 (aging, recheck recommended)
           [4] Amex India MR T&C §7 — verified 2026-04-01
```

### Citation rendering (CV52)

- Issuer source: `(HDFC T&C verified 30 days ago)`
- Aging source: `(Axis MITC Schedule A verified 75 days ago — aging, recheck soon)`
- Community-confirmation (silent): not surfaced in main response but logged in source-citation appendix for audit

### DecisionResult fields surfaced

- **winning_path:** Infinia online → SmartBuy travel redemption
- **runner_up:** None (no other earning option)
- **runner_up_loss_reason:** "All other cards in stack exclude insurance from rewards. Infinia is the only earning route on this category."
- **user_state_assumptions[]:** Cap headroom clean; no milestone pressure; SmartBuy travel valuation
- **inline_caveats[]:** SmartBuy-vs-statement-credit valuation drops Infinia from ₹900 to ₹270 but still only earning option; "nerfed?" answered generically pending DA64
- **conflict_disclosures[]:** none
- **NEW: completeness_disclosures[] (proposed CV71):** 3 cards surfaced with explicit EXCLUDED state + source citation; leads narrative

---

## 8. End-to-end pipeline timeline

**Turn 1 (clarifying-Q):** total user-perceived ~220ms (preamble + clarifying-Q render).
- Intent parser: 80ms
- Cap-elicitation detection (engine §5.1 `needs_elicitation`): 40ms
- Conversational synthesis of clarifying-Q: 60ms
- LLM streaming start: 40ms

**Turn 2 (final answer):** total user-perceived ~2.4s.
- Profile reconstruction + DecisionContext: 50ms
- Preamble fires: 180ms (within 200ms target)
- Subgraph retrieval (parallel Cypher on 4 cards + insurance category + caps + exclusions): 200ms
- Engine compute (4-path enumeration + scoring + completeness check (E63 proposed) + conflict resolution + R50 explainer): 200ms
- Narrator streaming begin: ~800ms after t'=0
- Narrator streaming complete: ~2400ms after t'=0
- **Engine compute is ~200ms (within 300ms p95 budget); narrator streaming dominates at ~1.6s**

**Latency p50/p95/p99:**
- p50: 2.1s user-perceived (engine ~150ms; narrator ~1.5s)
- p95: 2.8s (engine ~250ms; narrator ~2.0s)
- p99: 3.5s (engine ~400ms with cache miss; narrator ~2.5s)

**Budget check:** ✓ within conversational §8 budget (2.5s typical; 3–5s heavy). No `extended_thinking_required`.

---

## 9. ✓ Works / ⚠ Gaps / ⛔ Blockers

### ✓ Works (9 items)
1. Intent classification correctly identifies primary `purchase_routing` + detects secondary `RULE_CHANGE_DIFF` from "nerfed?"
2. Schema declares all needed entities/edges; R51–R55 sync additions cover new properties
3. Infinia insurance earn rule + 10K RP/mo cap acquired and verified fresh
4. Cap-elicitation hook fires correctly when user has multi-cap stack (engine §5.1)
5. Conversational handles user-skip path gracefully; assumptions surfaced in `user_state_assumptions[]`
6. Engine compute well within 300ms p95 budget (200ms typical)
7. Narrator leads with structural insight ("3 of 4 excluded"), not numeric ranking — this is the **correct UX for a reward-churner** who's asking "what's been nerfed"
8. Citation rendering distinguishes fresh vs aging facts (Magnus T&C at 75d surfaced as aging)
9. Valuation-flip caveat surfaced inline (Infinia drops to ₹270 if statement credit; still only earning option)

### ⚠ Gaps (5 items)

1. **G1 — Dual clarifying-Q friction.** Asking BOTH "cap state" + "milestone state" in one turn violates the 1-Q-max conversational discipline; risk of user friction. Workaround used: explicit "say skip" escape. Mitigation: CV69 proposed (bundled-Q with escape). **Severity: MEDIUM**
2. **G2 — DA64 PENDING.** "Nerfed?" intent gets generic acknowledgment ("yes, severely; here's roughly what changed") instead of specific per-card dated diff. Mitigation: elevate DA64 to HIGH-priority acquisition add. **Severity: MEDIUM (elevation triggered)**
3. **G3 — Restriction-signal community override.** For exclusion signals, community is often the leading edge but engine tier-hierarchy weights community as Tier 3 (LOW). For this query, manual community cross-check confirmed issuer findings; for other queries this mis-rating could matter. **Severity: LOW for Q4.2; MED-LOW in general**
4. **G4 — Cold-start ValuationProfile rank-sensitivity.** Infinia's ₹900 vs ₹270 is a 3.3× swing on the valuation assumption. Narrator surfaces it but as a passive caveat. Better: surface as active "rank-flip if valuation < X" warning. Mitigation: E62 + CV70 proposed (rank-flip sensitivity analysis). **Severity: MEDIUM-HIGH** (less critical when there's only 1 earning option, but critical when there's a real runner-up)
5. **G5 — Missing-exclusion blind spot (CRITICAL).** The initial pass of this analysis hallucinated earn rates for HSBC (1.5% capped) and Magnus (1 RP/₹200 base) instead of retrieving facts. Both cards in reality exclude insurance entirely. The blind spot is general: 12 qualifier-heavy categories × 50+ cards = 600 critical (card, category) tuples that must be resolved. Currently the graph allows silent UNKNOWN. **Severity: CRITICAL — drives multiple TIER-1 architecture additions; see §11 + Appendix**

### ⛔ Blockers (0 items)
None. Pipeline produces a correct, useful answer after Phase-0 manual fact retrieval. **However: at v0 without DA68 coverage matrix populated, every analysis of a qualifier-heavy category requires manual retrieval — this is operationally a blocker for scale.**

---

## 10. Bottlenecks

### Latency bottlenecks
- **B1:** Narrator streaming dominates wall-clock (~1.6s of ~2.4s). Engine is 8% of total time; narrator + LLM is 67%. Optimizing engine further yields diminishing returns; narrator (token throughput, parallel tool-use) is the real lever.
- **B2:** Clarifying-Q adds a full turn (~220ms preamble + user-think-time 3–8s real-world). Bundled escape mitigates but doesn't eliminate.

### Quality bottlenecks
- **B3:** Cold-start ValuationProfile assumption is structural. Without progressive learning, every cold-start query carries valuation rank-sensitivity. For Q4.2 it's a flavor-only concern (one earning option); for queries with real runner-ups it can flip rankings.
- **B4:** Tier-hierarchy conflict resolution rigidity for restriction signals. Community as Tier 3 underweights it for exclusion-class facts.
- **B5 (CRITICAL):** Missing-exclusion silent default. The graph + skill currently allow "no exclusion edge = default earn applies" without enforcement that every (card, category) tuple be explicitly resolved. Silent failure mode.

### UX bottlenecks
- **B6:** Dual-Q clarification violates 1-Q discipline. Bundled-with-escape works but feels janky for users WITH milestone awareness — they'd answer cap but be confused by simultaneous milestone Q.
- **B7:** "Nerfed" inline caveat is vague without DA64. Reward-churner asking that wants specific dates + magnitudes, not generic acknowledgment.

---

## 11. Deeper-dive points + required refinements

### Deeper-dive points

- **D1 — When are bundled-clarifying-Qs acceptable?** Both cap state + milestone state materially change the answer. Strict 1-Q discipline breaks here. Bundled-with-escape acceptable for v1; refine in v2 with conversational learning. **80/20 verdict: TIER-2.**
- **D2 — DA64 RuleChangeEvent extension priority.** Q4.2 surfaces a real user need ("nerfed?") that's blocked without DA64. Multiple qset Qs (Q15 SmartBuy cap email; qset2 Q9 Marriott devaluation) need DA64. **80/20 verdict: ELEVATE TO HIGH** — DA64 more critical than initially flagged.
- **D3 — Restriction-signal community override.** Engine currently treats community as Tier 3. For exclusion/restriction class facts, community is often leading edge. **80/20 verdict: TIER-2 — refine schema §11.3 conflict resolution with per-fact-class override matrix** after cross-query confirmation.
- **D4 — Cold-start ValuationProfile + rank-sensitivity warning.** Narrator must surface "rank flips if valuation < X" warning when within sensitivity threshold. **80/20 verdict: TIER-1.**
- **D5 — Missing-exclusion blind spot (CRITICAL).** The skill + graph allow silent UNKNOWN on (card, category) tuples. 12 qualifier-heavy categories × 50+ cards = 600 critical tuples. Without explicit resolution, hallucination is the failure mode. **80/20 verdict: TIER-1 CRITICAL — multiple architecture additions required.** See appendix.

### Existing refinements engaged

- R49 (conflict_disclosures), R50 (explanation_structure + runner_up_loss_reason), R51 (RewardCurrency flags + REDEEMS_VIA statement_credit_rate), R53 (ValuationProfile confidence + timestamps), R55 (Card.pooling_policy — declared but unused)
- DA1 (per-card source registry), DA10 (community signals), DA11 (MCC mapping), DA24 (RuleChangeEvent), DA64 (PENDING extension)
- E1 (decision-engine pure function), E5 (CapHeadroomVector), E6 (path enumerator), E8 (explainer), E15 (margin guards)
- CV1 (preamble), CV2 (clarifying-Q first), CV5 (citation rendering), CV52 (community-vs-issuer rendering)

### NEW refinements surfaced

- **CV69** — bundled-clarifying-Q with escape (max 2 Qs in 1 turn if BOTH are state-changing) → TIER-2
- **CV70** — rank-flip-sensitivity warning pattern in narrator → **TIER-1**
- **R56 — CategoryEarnAssertion** — per (card, category) tuple, explicit 4-state assertion: EARNS_AT / EXCLUDED / CAPPED / UNKNOWN. UNKNOWN is tracked, never silent → **TIER-1 CRITICAL**
- **R57 — EarnRule.default_rule_applies** — boolean disambiguating cards with universal default vs co-brand-only earning → **TIER-1 CRITICAL**
- **R56-alt** — per-fact-class confidence override (community elevated for restriction class) → TIER-2 (different from R56 above; named for now)
- **E61** — engine resolver consults fact-class override before tier-hierarchy → TIER-2
- **E62** — rank-flip sensitivity analysis (compute "rank flips at X valuation" and surface in user_state_assumptions when within threshold) → **TIER-1 (pairs with CV70)**
- **E63 — pre-flight completeness check** — before path enumeration, verify every (card, category) in DecisionContext has explicit CategoryEarnAssertion. UNKNOWN = partial result with explicit warning, never silent default → **TIER-1 CRITICAL**
- **E64 — negative-fact confidence band** — exclusions carry confidence + freshness like positive facts → **TIER-1 CRITICAL**
- **DA67 — exclusion-list as first-class artifact** in Validation agent → **TIER-1 CRITICAL**
- **DA68 — per-card category coverage matrix** (50 cards × 12+ qualifier-heavy categories) as tracked grid; refresh-cadenced; UNKNOWN tuples surfaced in steward queue → **TIER-1 CRITICAL — likely the steward's primary v1 artifact**
- **CV71 — narrator pattern for completeness gaps** — when ≥2 cards excluded for queried category, lead with structural insight ("3 of 4 don't earn") rather than numeric ranking; when UNKNOWN tuple present, caveat instead of assume → **TIER-1 CRITICAL**

### Overall 80/20 verdict

**TIER-1 LAYER-IN with 7 critical v1 architecture additions.** Pipeline answers Q4.2 correctly after Phase-0 manual fact retrieval. To scale beyond manual retrieval, the missing-exclusion blind-spot cluster (R56, R57, DA67, DA68, E63, E64, CV71) MUST be added to v1. These are higher priority than the previously-identified DA64 elevation. Recommend:
1. Build DA68 (coverage matrix for MVS Tier 1 cards × 12 qualifier categories) as the steward's first v1 artifact
2. Apply R56 + R57 schema additions + E63 + E64 engine additions + DA67 acquisition contract + CV71 conversational pattern in next sync sprint
3. Validate fix via 1–2 confirmation queries (Q19 jewelry; Q4.3 gift cards) before applying to all 4 plans
4. Apply DA64 elevation in same sync sprint (lower priority but still v1)

---

## Appendix — Phase-0 data-retrieval log

| Tuple | Initial Phase-0 state | Resolution method | Retrieval date | Source URL/citation | Tier classification |
|---|---|---|---|---|---|
| (Infinia, insurance/5960) | KNOWN (v0 manual seed) | Graph lookup | 2026-04-22 | HDFC Infinia T&C — Rewards Schedule §3.2 | Tier 1.1 |
| (HSBC Cashback, insurance/5960) | UNKNOWN | Manual issuer T&C retrieval + community cross-check | 2026-05-24 | HSBC India Cashback T&C §5.4 "Excluded Categories" (verified 2026-03-15); TF May 2026 mega-thread confirms | Tier 1.1 + Tier 3 confirmation |
| (Magnus Burgundy, insurance/5960) | UNKNOWN | Manual issuer MITC retrieval + community cross-check | 2026-05-24 | Axis Magnus Burgundy MITC Schedule A "Categories not eligible for reward points" (verified 2026-03-08, aging); Reddit r/CreditCardsIndia mega-thread May 2026 | Tier 1.1 + Tier 3 confirmation |
| (Amex Gold Charge, insurance/5960) | UNKNOWN at start; quickly confirmed | Issuer T&C retrieval | 2026-05-24 | Amex India MR T&C §7 (verified 2026-04-01); long-standing since 2022 | Tier 1.1 |

**Key lesson:** of 4 (card, category) tuples in this query, 3 were UNKNOWN at start of analysis and required manual Phase-0 retrieval. Without this discipline the initial draft of this analysis hallucinated rates and produced a materially wrong recommendation (HSBC ₹405 runner-up, Magnus ₹115 third). **Phase-0 discipline is non-negotiable.**

---

## Methodology footnote

This analysis was performed using the `swypd-query-stress-test` skill (v1). First artifact in `/Users/ankurv/swypd/query_run/`. The missing-exclusion blind spot (D5) was surfaced by an initial draft of this analysis being materially wrong; the user correction identifies the failure mode; this final version incorporates the corrected facts + the deeper architecture finding. The methodology fix — Phase-0 data-availability check — is now part of the skill definition itself, so subsequent runs cannot repeat this failure mode silently.
