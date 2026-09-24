---
description: Run an exhaustive end-to-end pipeline stress-test on a single qset1/qset2 query. Traces the query through all 4 layers of the swypd suite (schema → acquisition → decision engine → conversational), produces a standardized 11-section analysis identifying works/gaps/blockers/bottlenecks/deeper-dive points + 80/20 verdicts. Writes output to /Users/ankurv/swypd/query_run/<qsetN>-<Q#>-<descriptor>.md and updates the _index.md rollup. Argument: query_id (e.g., "qset1.Q4.2" or "qset2.Q8").
---

# swypd-query-stress-test

Run exhaustive end-to-end pipeline stress-test on a single qset query and write the result as a per-query Markdown file in `/Users/ankurv/swypd/query_run/`.

**Argument:** `query_id` — one of:
- `qset1.Q1` … `qset1.Q25` (the numbered questions in `/Users/ankurv/swypd/qset1.md`)
- `qset1.P<persona#>.<question#>` (persona-block questions, e.g., `qset1.P4.2`)
- `qset2.Q1` … `qset2.Q13`
- `qset2.U<#>` (qset2 unnumbered persona questions)

## Workflow

### Phase 0 — Data-availability audit (MANDATORY; do this FIRST)

**THE SINGLE MOST IMPORTANT RULE OF THIS SKILL:** never assert a fact value from memory, general knowledge, or inference. **Proven across 80 query runs: when this rule is violated (Q4.2 first draft), materially wrong recommendations result. When followed (Q19 + 78 subsequent), zero hallucinations.**

Every numeric or categorical fact (earn rate, cap, exclusion, transfer ratio, redemption rate, MCC mapping) must trace to either:
- **PREFERRED**: graph CategoryEarnAssertion (R56) lookup with explicit state (EARNS_AT / EXCLUDED / CAPPED / UNKNOWN) — once DA68 coverage matrix is populated
- An issuer T&C / aggregator / community URL retrieved during the analysis with cited URL + retrieval date
- An explicit `UNKNOWN — Phase-0 blocker` flag (NEVER silent default to "earn applies")

**Schema reference**: R56 CategoryEarnAssertion is the canonical entity; per schema §11.5 negative-fact framework, UNKNOWN is tracked, never silent.

For each `(card, merchant_category)` tuple in the user's declared portfolio × queried category:

| State | What to do |
|---|---|
| **EARNS_AT** | Retrieve rate + multiplier + caps + source citation |
| **EXCLUDED** | Note as zero-earn with source citation |
| **CAPPED** | Retrieve rate + cap + headroom |
| **UNKNOWN** | HARD STOP. Flag tuple as Phase-0 blocker. Mark analysis "data-incomplete" and proceed with partial-result warning, OR pause pending data acquisition |

**Particularly check the 12 qualifier-heavy categories** where missing-exclusion is a known blind-spot (see /Users/ankurv/swypd/docs/decision-engine-plan.md and qset finding from Q4.2):
- Insurance (MCC 6300/5960)
- Rent (MCC 6513)
- Fuel (MCC 5541)
- Wallet load (MCC 6540)
- Government payments (MCC 9311/9399)
- Utility (MCC 4900 series)
- Education (MCC 8211/8220/8244/8299)
- Gift card purchases
- Quasi-cash / gold / jewelry (MCC 5944/7995)
- BBPS
- UPI-on-RuPay
- EMI conversions

If the analysis runs WITHOUT a populated graph (current state at v0), the analyst MUST retrieve facts on-demand from issuer T&C pages, aggregators (CardInsider/CardExpert/PaisaBazaar), or community sources (Reddit r/CreditCardsIndia, TechnoFino, LFAL) — and cite them inline. Memory-asserted facts are NOT permitted.

### Phase 0b — MANDATORY per-(card, merchant) keyword search (CV103)

**THE SECOND MOST IMPORTANT RULE**: for ANY purchase_routing query mentioning a specific merchant (Jockey, Apple, Tanishq, Reliance Digital, Marriott, etc.), the analyst MUST run per-(card, merchant) keyword search BEFORE category-level retrieval.

**Why**: a real-user query ("best card for Jockey on Infinia + Magnus Burgundy + HSBC TravelOne") revealed the methodology was anchoring on category (apparel) and missing the merchant-specific Portal-Specific Merchant Accelerator (PSMA): SmartBuy Jockey 10X = 33.3% effective, vs Myntra 5X = 16.7%. **2× understatement.** PSMA is a third fact class beyond category-earn (R56) and co-brand (R29), documented in schema §11.7 (R62 MerchantAccelerator), acquisition §25 (DA91 PSMA catalog feed), engine §31 (E81 PSMA pre-check), and conversational §27 (CV103 mandatory keyword search).

**For each card in user's portfolio AND each merchant in query, run these keyword searches**:

| Card family | Required searches |
|---|---|
| **Any HDFC card** (Infinia, Diners Black, Diners Privilege, Regalia/Gold, Millennia, MoneyBack+, Marriott Bonvoy, IHCL HDFC Bonvoy, Tata Neu Infinity/Plus, Pixel Play, BizBlack, BizPower) | `"<merchant> HDFC SmartBuy"`, `"<merchant> Gyftr voucher"`, `"<merchant> SmartBuy 10X"`, `"<merchant> HDFC offer 2026"` |
| **Any ICICI card** (Amazon Pay ICICI, Coral, Sapphiro, Rubyx, Emeralde Private Metal, MakeMyTrip ICICI) | `"<merchant> ICICI iShop"`, `"<merchant> ICICI offer 2026"` |
| **Any Axis card** (Atlas, Magnus Burgundy, Flipkart Axis, Ace, Select, Vistara, Airtel) | `"<merchant> Axis EDGE marketplace"`, `"<merchant> Axis Grab Deals"`, `"<merchant> Axis offer 2026"` |
| **Any Amex card** (Plat Charge, Plat Reserve, Plat Travel, MRCC, SmartEarn, Gold Charge) | `"<merchant> Amex Offers"`, `"<merchant> Amex Travel 2026"` |
| **Any SBI card** (Cashback SBI, Elite, Prime, BPCL Octane, Vistara SBI) | `"<merchant> SBI Card offer"` |
| **Tata Neu Infinity HDFC (if in stack)** | `"<merchant> Tata Neu app NeuPass"` |
| **HSBC TravelOne / Premier** | `"<merchant> HSBC offer 2026"` |
| **IDFC FIRST cards (Mayura, Wealth, WOW)** | `"<merchant> IDFC FIRST offer"` |
| **Universal (run for any merchant + any card)** | `"<merchant> gift card discount"`, `"<merchant> cashback portal"`, `"<merchant> CashKaro Magicpin"`, `"<merchant> Zingoy"` |

**Promotion rule**: when a PSMA is found, it WINS over category-level fallback. Engine resolution order: PSMA → co-brand → category-fallback (per engine §31.2).

**Output requirement**: in §4 (Acquisition layer) section of every per-query file, surface PSMA findings explicitly with:
- Portal name (SmartBuy / Gyftr / iShop / etc.)
- Card-set covered
- Multiplier (e.g., 10X)
- Voucher discount (if applicable)
- Validity window
- Cap-headroom implications
- Source URL

**When PSMA understates math**: if PSMA gives 10X but category-fallback gave 5X (as in Jockey case), narrator MUST surface BOTH:
- Best route via PSMA: ~33% effective
- What would have been recommended without PSMA: ~16.7% category-fallback
- Lift from PSMA: 2×

### Phase 1 — Locate query

Read `/Users/ankurv/swypd/qset1.md` and `/Users/ankurv/swypd/qset2.md` to find the question by `query_id`. Extract:
- Persona one-liner
- Stack (cards held)
- Any declared state in the persona header
- Verbatim question text

### Phase 2 — Produce the 11-section analysis

Use this template **exactly**. Every section must be filled. Any section that returns "blocked / unknown / gap" auto-becomes a deeper-dive point.

```markdown
# Q<id> — <persona one-liner>

**Query file:** `query_run/<filename>.md`
**Run date:** <YYYY-MM-DD>
**Skill version:** v1

## 1. Query header
- **Persona:** <one-line>
- **Stack:** <cards>
- **Declared state:** <what user told us; "none" if cold-start>
- **Verbatim question:** *<exact text>*
- **Difficulty rating:** Easy / Medium / Heavy (per heuristic: 1 card = Easy; 2-3 cards single-category = Medium; 4+ cards OR multi-currency OR multi-component trip OR future-dated = Heavy)

## 2. Intent classification + DecisionContext

| Field | Value | Confidence | Notes |
|---|---|---|---|
| intent_type (primary) | <one of 10 per conv §24.1> | 0.X | <reasoning> |
| intent_type (secondary) | <if any> | 0.X | <implicit intent> |
| amount | <Decimal> | high/med/low | |
| merchant_category | <MCC + label> | | |
| merchant | <name or unspecified> | | |
| target_date | <date or relative> | | |
| portfolio | <list of card_ids> | declared at onboarding | |
| household_scope | <None or Dict> | | |
| expiry_window_days | <None or int> | | |
| extended_thinking_required | true/false | | |

**Clarifying-Q (if any):** <verbatim of what conversational layer would ask, or "none — query is specific enough">

**user_state_assumptions[]:** <list>

## 3. Schema layer — entities & edges traversed

### Nodes
| Layer | Node type | Instance(s) | Properties read |
|---|---|---|---|

### Edges
| Edge type | From → To | Properties read |
|---|---|---|

**Required refinements engaged:** <R# list>
**Schema-blocker check:** ✓ / ⛔ <explanation>

## 4. Acquisition layer — sources, cadence, freshness

| Fact | Source tier | DA refinement | Cadence | Freshness halflife | Status (ACQUIRED / acquisition-blocked / schema-blocked / UNKNOWN-Phase-0-blocker) | Source citation (URL / verified date) |
|---|---|---|---|---|---|---|

**Acquisition-blocker check:** ✓ / ⚠ / ⛔ <explanation>
**Phase-0 UNKNOWN tuples:** <list, or "none">

## 5. Decision engine traversal

- **Query type dispatch:** <one of 6, engine §4>
- **Hard problems engaged:** <#1–#7 list>
- **Path enumeration strategy:** <Beam Search / A* / OR-Tools CSP / direct lookup>
- **Number of candidate paths:** <N>

### Per-path math
| # | Path | Gross earn | Cap clamp | Effective earn | Currency value (₹/pt) | NetValue | Effective % |
|---|---|---|---|---|---|---|---|

**Pareto ranking + winner + runner_up_loss_reason:** <explanation>

## 6. Confidence + freshness scoring + conflict resolution

| Path | Confidence | Freshness | Margin guard applied? |
|---|---|---|---|

**Conflicts detected:** <list with resolution method>
**ConflictDisclosure[] entries:** <count + brief>

## 7. Conversational layer — intent parsing + streaming UX

### Streaming timeline
```
[t=0]      USER: <question>
[t=Xms]    BOT preamble: <text>
[t=Yms]    BOT clarifying-Q (if any): <text>
[USER turn] USER: <response>
[t'=0]     <new turn>
[t'=Xms]   <next milestone>
...
[t'=Zs]    <streaming complete>
```

### Citation rendering
<inline citations with verified dates + tier markers>

### DecisionResult fields surfaced
- winning_path
- runner_up + runner_up_loss_reason
- user_state_assumptions[]
- inline_caveats[]
- conflict_disclosures[]

## 8. End-to-end pipeline timeline

- **Turn 1 (clarifying-Q):** <total user-perceived latency + layer breakdown>
- **Turn 2 (final answer):** <total + breakdown>
- **Latency p50/p95/p99:** <estimates>
- **Budget check:** ✓ within conv §8 budget / ⚠ borderline / ⛔ exceeds

## 9. ✓ Works / ⚠ Gaps / ⛔ Blockers

### ✓ Works
1. <item>

### ⚠ Gaps
1. **G<n>** — <gap> — Severity: <LOW/MED/HIGH>

### ⛔ Blockers
1. <item, or "none">

## 10. Bottlenecks

### Latency bottlenecks
- **B<n>:** <description>

### Quality bottlenecks
- **B<n>:** <description>

### UX bottlenecks
- **B<n>:** <description>

## 11. Deeper-dive points + required refinements

### Deeper-dive points
- **D<n>** — <question/concern> — 80/20 verdict: <SHIP-AS-IS / TIER-1 / TIER-2 / DEFER / ACQUISITION-CRITICAL-BLOCK>

### Existing refinements engaged
- R<#>, DA<#>, E<#>, CV<#> with brief notes

### NEW refinements surfaced
- **R<#> / DA<#> / E<#> / CV<#>** — <name + brief>

### Overall 80/20 verdict
- **SHIP-AS-IS** / **TIER-1 LAYER-IN** / **TIER-2 LAYER-IN** / **ACQUISITION-CRITICAL-BLOCK** with rationale

## Appendix — Phase-0 data-retrieval log
<list every fact retrieved with source URL + retrieval date + tier classification>
```

### Phase 3 — Write output file

File path: `/Users/ankurv/swypd/query_run/<filename>.md`

Filename convention: `<qset>-<question_id>-<3-5-word-descriptor>.md`
- `qset1-Q4.2-infinia-magnus-insurance.md`
- `qset1-Q19-wedding-jewelry-excluded.md`
- `qset2-Q8-krisflyer-transfer-bonus-timing.md`

### Phase 4 — Update _index.md rollup

Update `/Users/ankurv/swypd/query_run/_index.md` (create if absent) by appending a row:

| Query | File | Date | Difficulty | Primary intent | Verdict | Key deeper-dives |
|---|---|---|---|---|---|---|
| Q4.2 | qset1-Q4.2-…md | 2026-05-24 | Medium-Heavy | purchase_routing | TIER-1 LAYER-IN | D1 bundled-Q; D2 DA64; D5 missing-exclusion |

## Heuristics (reference)

- **Difficulty rating:** 1 card → Easy; 2-3 cards single-category → Medium; 4+ cards OR multi-currency OR multi-component trip OR future-dated → Heavy
- **Clarifying-Q dispatch:** ≥2 caps in play AND user didn't declare → ask cap state; user mentions milestone OR ≥1 milestone reachable in stack → ask milestone state; if both → bundled-with-escape (CV69 proposed)
- **Hard-problem engagement:** caps in play → #1; SmartBuy in path → #2; ≥3 active offers → #3; ≥1 currency without declared valuation → #4; future-dated → #5; mixed confidence/freshness → #6; voucher chain → #7
- **80/20 verdicts:**
  - **SHIP-AS-IS** = pipeline correct as-built; no refinement needed
  - **TIER-1 LAYER-IN** = small refinement worth adding to v1
  - **TIER-2 LAYER-IN** = nice-to-have for v2
  - **DEFER** = wait for cross-query evidence
  - **ACQUISITION-CRITICAL-BLOCK** = pipeline cannot answer correctly without specific DA add

## Canonical source-class matrix

Use `/Users/ankurv/swypd/docs/decision-engine-plan.md` §28 as the canonical lookup for "what tier is this fact from + which DA refinement powers it + freshness halflife." Don't re-derive; cite §28 directly.

## Cross-query aggregation

After each run, the analyst (or a subsequent skill invocation) should periodically review `_index.md` and the per-query files to identify:
- Per-deeper-dive-point: how many queries surfaced it? (Sort by frequency)
- Per-new-refinement: how many queries surfaced it? (Frequency = 80/20 promotion signal)
- Per-blocker: which queries are blocked by the same acquisition gap? (Cluster by DA refinement)
- Per-bottleneck: which queries share the same latency/quality/UX bottleneck? (Cluster by class)

Aggregated findings feed back into revisions of the 4 plans (schema, acquisition, engine, conversational) + build-roadmap.

### Cross-query promotion thresholds (proven during 80-query stress test)

- **1 confirmation** → tracked in `_index.md` under "single-query proposals"; remain TIER-3 until more evidence accumulates
- **2 confirmations** → promote to TIER-2 candidate; consider for post-v1 roadmap
- **3+ confirmations** → promote to **TIER-1 v1-critical**; apply in next sync sprint
- **5+ confirmations** → architecture-defining pattern; central to the refinement family (e.g., D5 missing-exclusion at 8 confirmations drove the R56-R60 + DA67/DA68 + E63/E64 + CV71 cluster)

### Aggregation workflow

1. After every per-query analysis, update `_index.md` Cross-Query Aggregation table with new refinement counts
2. Every ~10 queries, scan for refinements that crossed the 3-confirmation threshold → flag for TIER-1 promotion in next iteration
3. After ~20 queries, scan for emergent meta-patterns (e.g., "12 qualifier-heavy categories", "business-vs-personal stack distinction") — these become Design Guidelines, not just refinements
4. After full qset coverage (80 queries), produce `_SUMMARY-and-RANKED-ACTIONS.md` with ranked action plan organized by tier
5. Apply TIER-1 cluster to 4-plan suite in next architecture iteration sprint

### Reference: 80-query stress test outcome

The 80-query analysis produced 14 TIER-1 refinements applied to the 4-plan suite via Sprint 1-3 (per build-roadmap §8). Pattern reference:
- D5 missing-exclusion blind spot (8 confirmations) → R56-R60 + DA67/DA68 + E63/E64 + CV71
- DA61 HistoricalBonusEvent (8+ blocked queries) → elevated to HIGH; Sprint 2
- DA64 RuleChangeEvent diff (5+ blocked) → elevated to HIGH; Sprint 2
- R58/CV72 platform-membership state (4 confirmations) → Sprint 3
- E62/CV70 rank-flip sensitivity (4 confirmations) → Sprint 3
- CV79 anti-optimism narration (3 confirmations) → Sprint 3
- R59 milestone-qualifying-spend (3 confirmations) → Sprint 3

Subsequent stress-test waves should follow the same methodology + threshold-based promotion discipline.

### Reference: 50-query qset3 stress test outcome — LMAE architectural finding

The 50-query qset3 analysis (`/Users/ankurv/swypd/query_run/qset3/`) surfaced a **different pattern: engine-family gap, not refinement gap**. ~70% of qset3 queries are lifecycle/mechanics/meta (application, eligibility, retention, CIBIL, EMI mechanics, lounge access, billing, dispute, insurance, concierge) — out-of-scope for the 2-engine architecture (decision + recommendation).

**Architectural response: 3rd engine — LMAE (Lifecycle/Mechanics Advisory Engine)** added in Sprint 4-5 (build-roadmap §8.4-§8.5). Pattern reference:
- CV82 APPLICATION_ELIGIBILITY (3 confirmations: Q1, Q3, Q4) → Sprint 4 TIER-1
- CV84 CIBIL_ADVISORY (4 confirmations: Q5, Q23, Q25, Q34) → Sprint 4 TIER-1
- CV96 EMI_MECHANICS (3 confirmations: Q28, Q29, Q30) → Sprint 4 TIER-1
- CV88 LOUNGE_FLOW_ADVISORY (1 high-frequency: Q14) → Sprint 4 TIER-1
- DA77 EligibilityCriteria + DA81 CIBIL knowledge + DA84 lounge routing → Sprint 4 TIER-1 data feeds

### LMAE-intent evaluation criteria (DIFFERENT from decision-engine queries)

When evaluating LMAE-intent query analyses (CV82–CV102 cluster), use **different criteria** than decision-engine queries:

| Aspect | Decision-engine queries (qset1+2 pattern) | LMAE queries (qset3 pattern) |
|---|---|---|
| **Correctness criterion** | Numeric (NetValue, Pareto ranking) | Advisory quality (sources cited, no fabrication, structurally correct narrative) |
| **Confidence-band source** | Per-fact tier-of-trust (issuer > community typically) | Per-fact tier-of-trust BUT community elevated to **Tier 1.5** for restriction-class facts (retention scripts, lounge flows, hack patterns) |
| **Output format check** | "Path 1: ₹X; Path 2: ₹Y; ..." (numeric ranking) | "Honest truth: X. Three paths: ... My pick: Y. Action: Z." (narrative template) |
| **Multi-turn** | Single-turn typically | 2-3 turn convergence acceptable; `follow_up_questions[]` in LMAEResult |
| **DPDP** | Standard | Stricter for CIBIL/PII queries; query-time-only with consent |

### Pattern observation across stress tests

Stress-testing surfaces **TWO classes of gaps**:
1. **Refinement gaps** within an engine family (qset1+2 pattern → R56-R60, DA67-DA76, E61-E76, CV69-CV81)
2. **Engine-family gaps** — entirely new engine needed for a different cognitive mode (qset3 pattern → LMAE)

Future stress-tests should probe both:
1. Are existing engines covering the user's transaction routing + optimization needs? (qset1+2)
2. Are existing engines covering the user's lifecycle/mechanics/meta needs? (qset3 → LMAE)
3. Are there OTHER cognitive modes we haven't tested? (potential qset4+ — emerging patterns)

### Reference: PSMA architectural finding (real-user-query, Jockey example)

A single real-user query ("best card for Jockey on Infinia + Magnus Burgundy + HSBC TravelOne") surfaced a third class of architectural gap: **methodology-anchoring gap**. The skill was anchoring Phase-0 retrieval on **category** (apparel) rather than **merchant name** (Jockey), missing the merchant-specific Portal-Specific Merchant Accelerator (10X Jockey on SmartBuy direct = 33.3% effective vs 5X Myntra fallback = 16.7%). 

**Architectural response: Sprint 3.5 mid-cycle injection** — R62 MerchantAccelerator (schema §11.7) + DA91 PSMA catalog (acquisition §25) + E81 PSMA pre-check (engine §31) + CV103 mandatory per-(card, merchant) keyword search (conversational §27 + Phase 0b above).

**Why TIER-1 on single-query evidence**: structural-failure-mode trumps cross-query frequency. The miss applies to EVERY purchase_routing query with a specific merchant, not just Jockey. Same promotion logic as D5 missing-exclusion (qset1+2) and LMAE engine-family (qset3).

**Three classes of architectural gaps surfaced so far**:
1. **Refinement gap within an engine** (D5 missing-exclusion → R56-R60 + DA67/DA68 + E63/E64 + CV71 cluster)
2. **Engine-family gap** (qset3 → LMAE — 3rd engine family for lifecycle/mechanics/meta)
3. **Methodology-anchoring gap** (PSMA — per-(card, merchant) retrieval must be mandatory; not just per-(card, category))

Future stress-tests should probe for ALL three classes of gaps.

## What this skill does NOT do

- Auto-fix the plans (that's a human review step after aggregation)
- Auto-acquire missing facts (Phase-0 UNKNOWN tuples must be either resolved manually before analysis or surfaced explicitly as blockers)
- Run on >1 query at once (one query, one file, one analysis)
- Replace the 4-plan suite — it stress-tests it
