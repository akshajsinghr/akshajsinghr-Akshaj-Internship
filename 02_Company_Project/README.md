# swypd Documentation

This folder contains two reviewer packages for swypd — a productized credit-card optimization platform for Indian multi-card holders.

## Two packages

| Package | Audience | Scope |
|---|---|---|
| **[`mvp/`](./mvp/)** | Reviewing the **v1 implementation plan** | Transaction routing engine for ~10 cards × ~6 merchant ecosystems + live discovery. 12-week build at 2 FTE. Everything LMAE, recommendation-engine, multi-turn, and Pareto-framing-related is deferred to v2. |
| **[`fullscope/`](./fullscope/)** | Reviewing the **full architectural vision** | 3-engine architecture (decision + recommendation + LMAE) covering transactional + portfolio + lifecycle/mechanics queries. ~22-26 week build covering 92%+ of stress-tested queries at v2 production-ready. |

## Which one to read

- **Building or evaluating v1 implementation?** → Start at [`mvp/README.md`](./mvp/README.md).
- **Evaluating the long-term architectural vision?** → Start at [`fullscope/README.md`](./fullscope/README.md).
- **Want to see what gets cut vs deferred between v1 and v2?** → Read `mvp/decision-log.md` ADR-015 (v1 narrowing decision).

## Relationship between the two packages

The **mvp** package is a strict subset of **fullscope**, narrowed by:
- **Data scope**: top 10 cards (vs ~80), 6 merchant ecosystems (vs unbounded), top 30 PSMAs (vs ~195)
- **Engine scope**: 1 decision engine + EXPLAINER fallback (vs 3 engines including LMAE)
- **Intent scope**: 5 transactional intents (vs 25)
- **Output complexity**: single best route + alternatives (vs Pareto top-3)

The **architecture and schema are identical** between v1 and v2. The mvp package builds the full ontology + selective-bitemporal data layer + 5-class agent taxonomy + steward queue + 3-layer cache from day 1; v1 narrows the data populated and handlers wired up. v2 expansion = add rows + activate dormant handlers, no schema migration.

## Architecture-evolution context

The architecture in both packages emerged from three stress-test iterations producing 338 numbered refinements (R1-R62 schema, DA1-DA91 acquisition, E1-E82 engine, CV1-CV103 conversational). See `stress-test-methodology.md` and `stress-test-index.md` in either package for the methodology + corpus.

The v1 narrowing decision (mvp/) was driven by two independent external reviews of the fullscope package (documented in `mvp/decision-log.md` ADR-015) — both converged on "v1 should be a transaction routing engine; defer LMAE and recommendation engine."

## Doc version

- Reviewer packages last updated: 2026-05-25
- mvp package targets: v1 launch in 12 weeks at 2 FTE
- fullscope package targets: v2 production-ready in ~22-26 weeks
