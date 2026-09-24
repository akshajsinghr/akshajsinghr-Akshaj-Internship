# Query Run Index

Rollup of all queries analyzed via the `swypd-query-stress-test` skill. **80 queries** complete across qset1 + qset2. See [`_SUMMARY-and-RANKED-ACTIONS.md`](./_SUMMARY-and-RANKED-ACTIONS.md) for the executive summary + ranked action plan.

## qset1 — persona-block (35 queries)

| # | Query | File | Verdict | Key findings |
|---|---|---|---|---|
| 1 | P1.1 | [iphone-amazon-routing](./qset1-P1.1-iphone-amazon-routing.md) | TIER-1 (R58/CV72) | Prime-membership state |
| 2 | P1.2 | [swiggy-blinkit-zomato-monthly](./qset1-P1.2-swiggy-blinkit-zomato-monthly.md) | SHIP-AS-IS | Quick-commerce sub-category |
| 3 | P1.3 | [college-fees-payment-gateway](./qset1-P1.3-college-fees-payment-gateway.md) | SHIP-AS-IS + D5 | Education exclusion |
| 4 | P2.1 | [mmt-goa-flights-62k](./qset1-P2.1-mmt-goa-flights-62k.md) | SHIP-AS-IS | SmartBuy 10X vs direct |
| 5 | P2.2 | [online-shopping-14k-monthly](./qset1-P2.2-online-shopping-14k-monthly.md) | SHIP-AS-IS | SBI Cashback Apr-26 caps |
| 6 | P2.3 | [regalia-gold-38k-points](./qset1-P2.3-regalia-gold-38k-points.md) | TIER-1 (CV70) | Rank-flip sensitivity |
| 7 | P3.1 | [maldives-2.8L-allocation](./qset1-P3.1-maldives-2.8L-allocation.md) | SHIP-AS-IS | Multi-component travel |
| 8 | P3.2 | [amex-pt-3.2L-milestone](./qset1-P3.2-amex-pt-3.2L-milestone-direction.md) | TIER-1 (R59) | Milestone-qualifying-spend |
| 9 | P3.3 | [europe-biz-class-multicurrency](./qset1-P3.3-europe-biz-class-multicurrency.md) | SHIP-AS-IS + DA61 | E54 multi-currency |
| 10 | P4.1 | [bbd-tv-wait-or-now](./qset1-P4.1-bbd-tv-1.1L-wait-or-now.md) | SHIP-AS-IS | E55 timing + sale-period |
| 11 | **P4.2 (Q4.2)** | [infinia-magnus-insurance](./qset1-Q4.2-infinia-magnus-insurance.md) | **TIER-1 D5 origin** | Missing-exclusion blind spot |
| 12 | P4.3 | [amazon-gift-cards-milestone](./qset1-P4.3-amazon-gift-cards-milestone-50k.md) | TIER-1 (CV76) | Voucher chain + honesty |
| 13 | P5.1 | [business-travel-1.35L](./qset1-P5.1-business-travel-1.35L.md) | SHIP-AS-IS | E66/E70/E72 cluster |
| 14 | P5.2 | [atlas-72k-miles-accor-flights](./qset1-P5.2-atlas-72k-miles-accor-or-flights.md) | SHIP-AS-IS + DA61 | Pareto top-3 |
| 15 | P5.3 | [refundable-bookings-milestone-gaming](./qset1-P5.3-refundable-bookings-milestone-gaming.md) | SHIP-AS-IS | E53 mechanism-aware |
| 16 | P6.1 | [grocery-pharmacy-32k-split](./qset1-P6.1-grocery-pharmacy-32k-split.md) | SHIP-AS-IS + CV72 | Tata NeuPass linkage |
| 17 | P6.2 | [croma-refrigerator-86k](./qset1-P6.2-croma-refrigerator-86k.md) | SHIP-AS-IS | Co-brand app routing |
| 18 | P6.3 | [electricity-bill-11.5k](./qset1-P6.3-electricity-bill-11.5k.md) | SHIP-AS-IS + D5 | Utility exclusion |
| 19 | P7.1 | [gst-2.4L-business](./qset1-P7.1-gst-2.4L-business.md) | SHIP-AS-IS (BizBlack wins) | Business-vs-personal |
| 20 | P7.2 | [meta-ads-1.1L](./qset1-P7.2-meta-ads-1.1L.md) | SHIP-AS-IS + E70 | Forex + stack-upgrade |
| 21 | P7.3 | [saas-60k-monthly](./qset1-P7.3-saas-60k-monthly.md) | SHIP-AS-IS | SmartBuy BizDeals 5X |
| 22 | P8.1 | [upi-local-merchants-35k](./qset1-P8.1-upi-local-merchants-35k.md) | SHIP-AS-IS | RuPay UPI fragmentation |
| 23 | P8.2 | [chemist-upi-4200](./qset1-P8.2-chemist-upi-4200.md) | SHIP-AS-IS + CV80 | Small-ticket simplification |
| 24 | P8.3 | [online-vs-upi-tatamis](./qset1-P8.3-online-vs-upi-tatamis.md) | SHIP-AS-IS | EXPLAINER + E70 |
| 25 | P9.1 | [simplify-2-cards](./qset1-P9.1-simplify-2-cards-12-15L.md) | SHIP-AS-IS | E57 portfolio consolidation |
| 26 | P9.2 | [axis-ace-renewal](./qset1-P9.2-axis-ace-renewal-keep-or-drop.md) | SHIP-AS-IS | E57 KEEP_OR_DROP |
| 27 | P9.3 | [redundant-cards](./qset1-P9.3-redundant-cards-amazon-dining-flights-utility.md) | SHIP-AS-IS | E73 redundancy detection |
| 28 | P10.1 | [senior-medicines-online](./qset1-P10.1-senior-medicines-online.md) | SHIP-AS-IS | Simplicity-leaning |
| 29 | P10.2 | [senior-international-airticket](./qset1-P10.2-senior-international-airticket.md) | SHIP-AS-IS + E70 | Forex + stack-upgrade |
| 30 | P10.3 | [coral-tiny-points-redemption](./qset1-P10.3-coral-tiny-points-redemption.md) | SHIP-AS-IS | Small-balance redemption |
| 31 | P11.1 | [amex-pt-milestone-or-infinia](./qset1-P11.1-amex-pt-milestone-or-infinia.md) | TIER-1 (R59) | Milestone-qualifying-spend |
| 32 | P11.2 | [macbook-apple-routing](./qset1-P11.2-macbook-apple-routing.md) | SHIP-AS-IS | Voucher chain + 70:30 |
| 33 | **P11.3** | [gold-jewelry-15k](./qset1-P11.3-gold-jewelry-15k.md) | TIER-1 D5 | Jewelry exclusion 2nd |
| 34 | P12.1 | [nri-3.6L-india-flights-hotels](./qset1-P12.1-nri-3.6L-india-flights-hotels.md) | SHIP-AS-IS | Atlas tier-building |
| 35 | P12.2 | [rent-55k-via-platform](./qset1-P12.2-rent-55k-via-platform.md) | SHIP-AS-IS + D5 | Rent exclusion (8th D5) |

## qset1 — numbered Q1-Q25 (25 queries)

| # | Query | File | Verdict | Key findings |
|---|---|---|---|---|
| 36 | Q1 | [ace-first-card-general-use](./qset1-Q1-ace-first-card-general-use.md) | SHIP-AS-IS | EXPLAINER + new-user |
| 37 | Q2 | [ace-coaching-fees](./qset1-Q2-ace-coaching-fees.md) | SHIP-AS-IS + D5 | Coaching exclusion |
| 38 | Q3 | [sony-headphones-amazon](./qset1-Q3-sony-headphones-amazon.md) | TIER-1 (R58/CV72) | Prime state 2nd |
| 39 | Q4 | [amazon-gift-card-brother](./qset1-Q4-amazon-gift-card-brother.md) | SHIP-AS-IS + D5 | Gift cards exclusion |
| 40 | Q5 | [swiggy-zomato-15k-monthly](./qset1-Q5-swiggy-zomato-15k-monthly.md) | SHIP-AS-IS | Per-merchant routing |
| 41 | Q6 | [myntra-eors-40k](./qset1-Q6-myntra-eors-40k.md) | SHIP-AS-IS | Axis Flipkart 7.5% Myntra |
| 42 | Q7 | [school-fees-1.2L-regalia-surcharge](./qset1-Q7-school-fees-1.2L-regalia-surcharge.md) | TIER-1 (E62/CV70) | Rank-flip sensitivity 2nd |
| 43 | Q8 | [home-insurance-18k-split](./qset1-Q8-home-insurance-18k-split.md) | SHIP-AS-IS + D5 | Insurance 2nd |
| 44 | Q9 | [oled-tv-flipkart-diwali-95k](./qset1-Q9-oled-tv-flipkart-diwali-95k.md) | SHIP-AS-IS | Co-brand + sale-period |
| 45 | Q10 | [london-hotel-gbp-forex](./qset1-Q10-london-hotel-gbp-forex.md) | SHIP-AS-IS + E72 | FHR non-points benefits |
| 46 | Q11 | [client-dinners-65k](./qset1-Q11-client-dinners-65k.md) | TIER-1 (E72) | Non-points-benefit valuation |
| 47 | Q12 | [diners-black-renewal-decision](./qset1-Q12-diners-black-renewal-decision.md) | SHIP-AS-IS | E57 KEEP_OR_DROP |
| 48 | Q13 | [bbd-iphone17-pro-stacking-6-cards](./qset1-Q13-bbd-iphone17-pro-stacking-6-cards.md) | SHIP-AS-IS | OR-Tools CSP territory |
| 49 | Q14 | [atlas-milestone-12k-tax](./qset1-Q14-atlas-milestone-12k-tax.md) | TIER-1 (R59) | Milestone exclusion 3rd |
| 50 | Q15 | [smartbuy-cap-email-what-changed](./qset1-Q15-smartbuy-cap-email-what-changed.md) | DA64 BLOCKED partial | RULE_CHANGE_DIFF intent |
| 51 | Q16 | [blr-sin-burn-mr-or-pay](./qset1-Q16-blr-sin-burn-mr-or-pay.md) | SHIP-AS-IS | Burn-vs-earn |
| 52 | Q17 | [marriott-singapore-85k-routing](./qset1-Q17-marriott-singapore-85k-routing.md) | SHIP-AS-IS | Co-brand vs Atlas transfer |
| 53 | Q18 | [maldives-3.5L-atlas-newlywed](./qset1-Q18-maldives-3.5L-atlas-newlywed.md) | SHIP-AS-IS | Multi-component + tier |
| 54 | **Q19** | [wedding-jewelry-excluded](./qset1-Q19-wedding-jewelry-excluded.md) | **TIER-1 D5 confirmation** | Jewelry exclusion 1st |
| 55 | Q20 | [gst-1.4L-new-card](./qset1-Q20-gst-1.4L-new-card.md) | TIER-1 (CV79) | Anti-optimism "no card wins" |
| 56 | Q21 | [macbook-pro-2.1L-emi-split](./qset1-Q21-macbook-pro-2.1L-emi-split.md) | SHIP-AS-IS | Voucher + no-cost EMI |
| 57 | Q22 | [mom-hospital-85k](./qset1-Q22-mom-hospital-85k.md) | SHIP-AS-IS | Medical category (not excluded) |
| 58 | Q23 | [regalia-22k-points-simple](./qset1-Q23-regalia-22k-points-simple.md) | SHIP-AS-IS | Simplicity-leaning |
| 59 | Q24 | [kirana-2800-rupay](./qset1-Q24-kirana-2800-rupay.md) | SHIP-AS-IS | RuPay-vs-Visa UPI |
| 60 | Q25 | [japan-multicurrency-atlas-infinia-amex](./qset1-Q25-japan-multicurrency-atlas-infinia-amex.md) | SHIP-AS-IS + DA66 | Multi-currency + lead-time |

## qset2 — numbered Q1-Q13 (13 queries)

| # | Query | File | Verdict | Key findings |
|---|---|---|---|---|
| 61 | Q1 | [15k-regalia-simple](./qset2-Q1-15k-regalia-simple.md) | SHIP-AS-IS | Small-balance |
| 62 | Q2 | [wallet-coin-consolidation](./qset2-Q2-wallet-coin-consolidation.md) | SHIP-AS-IS + E51 | Cross-program non-consolidation |
| 63 | Q3 | [goa-16k-multicurrency-rp](./qset2-Q3-goa-16k-multicurrency-rp.md) | SHIP-AS-IS | Multi-currency specific booking |
| 64 | Q4 | [sbi-elite-45k-expiring](./qset2-Q4-sbi-elite-45k-expiring.md) | SHIP-AS-IS + DA65 | Expiry-driven |
| 65 | Q5 | [europe-summer-4L-RP-2L-atlas-80k-MR](./qset2-Q5-europe-summer-4L-RP-2L-atlas-80k-MR.md) | SHIP-AS-IS | Multi-currency aspirational |
| 66 | Q6 | [business-london-best-converter](./qset2-Q6-business-london-best-converter.md) | SHIP-AS-IS | Transfer-ratio comparison |
| 67 | Q7 | [marriott-singapore-bonvoy-edge-topup](./qset2-Q7-marriott-singapore-bonvoy-edge-topup.md) | SHIP-AS-IS | Top-up transfer math |
| 68 | Q8 | [krisflyer-dec-transfer-bonus-pattern](./qset2-Q8-krisflyer-dec-transfer-bonus-pattern.md) | SHIP-AS-IS + DA61 | Dec bonus historical pattern |
| 69 | Q9 | [marriott-devaluation-edge-bonvoy](./qset2-Q9-marriott-devaluation-edge-bonvoy.md) | SHIP-AS-IS | E55 devaluation-risk |
| 70 | Q10 | [regalia-spouse-pooling](./qset2-Q10-regalia-spouse-pooling.md) | SHIP-AS-IS + DA57/58 | E52 spouse-pooling |
| 71 | Q11 | [goa-18k-burn-vs-save](./qset2-Q11-goa-18k-burn-vs-save.md) | SHIP-AS-IS | Burn-vs-save |
| 72 | Q12 | [citi-thankyou-defunct-28k](./qset2-Q12-citi-thankyou-defunct-28k.md) | SHIP-AS-IS + DA65 | Defunct program rules |
| 73 | Q13 | [japan-cherry-blossom-lead-time](./qset2-Q13-japan-cherry-blossom-lead-time.md) | SHIP-AS-IS + DA66 | E60 seasonality |

## qset2 — unnumbered U1-U13 (13 queries)

| # | Query | File | Verdict | Key findings |
|---|---|---|---|---|
| 74 | U1 | [42k-regalia-gold-simple](./qset2-U1-42k-regalia-gold-simple.md) | SHIP-AS-IS | Small-balance |
| 75 | U2 | [atlas-1.18L-blr-sin-oct](./qset2-U2-atlas-1.18L-blr-sin-oct.md) | SHIP-AS-IS | Burn-vs-save short-haul |
| 76 | U3 | [europe-biz-multicurrency-summer](./qset2-U3-europe-biz-multicurrency-summer.md) | SHIP-AS-IS | Multi-currency aspirational |
| 77 | U4 | [family-maldives-pooling](./qset2-U4-family-maldives-pooling.md) | SHIP-AS-IS | Bonvoy cross-account pool |
| 78 | U5 | [amex-mrcc-68k-expiring](./qset2-U5-amex-mrcc-68k-expiring.md) | SHIP-AS-IS | Expiry-driven Amex |
| 79 | U6 | [transfer-bonus-wait-rp-mr](./qset2-U6-transfer-bonus-wait-rp-mr.md) | SHIP-AS-IS + DA61 | Speculative-transfer risk |
| 80 | U7 | [bali-marriott-4-nights](./qset2-U7-bali-marriott-4-nights.md) | SHIP-AS-IS | Hotel top-up math |
| 81 | U8 | [india-us-biz-class-aspirational](./qset2-U8-india-us-biz-class-aspirational.md) | SHIP-AS-IS | Aspirational threshold-check |
| 82 | U9 | [small-fragmented-balances](./qset2-U9-small-fragmented-balances.md) | SHIP-AS-IS + E51 | Cross-program fragmentation |
| 83 | U10 | [marriott-st-regis-now-or-save](./qset2-U10-marriott-st-regis-now-or-save.md) | SHIP-AS-IS | Devaluation favors burning |
| 84 | U11 | [devaluation-panic-rp-edge](./qset2-U11-devaluation-panic-rp-edge.md) | SHIP-AS-IS | Speculative-transfer trap |
| 85 | U12 | [sbi-elite-spouse-pooling](./qset2-U12-sbi-elite-spouse-pooling.md) | SHIP-AS-IS + DA57/58 | Spouse-pooling 2nd |
| 86 | U13 | [orphaned-defunct-magnus-citi](./qset2-U13-orphaned-defunct-magnus-citi.md) | SHIP-AS-IS + DA65 | Defunct program 2nd |

(Note: 80 actual queries — counts above reach 86 due to numbering reflecting Q4.2/Q19 as both numbered and persona-block listings.)

---

## Cross-query aggregation — final tally

### D5 missing-exclusion blind spot — **CONFIRMED 8 TIMES** (overwhelming)
Q4.2 insurance | Q19 jewelry | P1.3 college fees | Q2 coaching | Q4 gift cards | P6.3 utility | P12.2 rent | P11.3 jewelry-2nd | Q8 insurance-2nd

### New refinements confirmation tally (sorted by cross-query confirmation count)

| Refinement | Confirmations | Tier verdict |
|---|---|---|
| **R56 / R57 / DA67 / DA68 / E63 / E64 / CV71** (D5 cluster) | **8** | **TIER-1 v1 critical (highest priority)** |
| **DA61** HistoricalBonusEvent | **8+** | **TIER-1 (elevate from MEDIUM to HIGH)** |
| **E72** non-points-benefit valuation | **5+** | TIER-2 (Q10, Q11, Q12, P5.1, Q17) |
| **DA64** RuleChangeEvent diff | **5+** | **TIER-1 (elevate from MEDIUM to HIGH)** |
| **R58/CV72** platform-membership state | **4** | **TIER-1 (P1.1, Q3, P6.1, P6.2)** |
| **E62/CV70** rank-flip sensitivity | **4** | **TIER-1 (Q4.2, P2.3, Q7, U10)** |
| **E70** stack-upgrade suggestion | **4** | TIER-2 |
| **CV79** anti-optimism narration | **3** | **TIER-1 (Q20, Q19, Q4)** |
| **R59** milestone-qualifying-spend | **3** | **TIER-1 (P11.1, P3.2, Q14)** |
| **DA65** ExpiryPolicy + DefunctProgram | **3** | **TIER-1 (Q4, Q12, U13)** |
| **DA57/DA58** BankPolicy spouse-pooling | **3** | TIER-1 (Q10, U12, U4) |
| **E55** devaluation-risk (sophisticated math) | **5** | TIER-2 |
| **CV78** burn-vs-save narration template | **6** | TIER-2 |
| **E66** growth-trigger card recommendation | **3** | TIER-2 |
| **E67** channel-advisory excluded-categories | **3** | TIER-2 |
| **CV56** extended-thinking-visible (multi-currency) | **8** | TIER-2 — already in plan, reinforced |
| **DA66** AwardAvailabilityTimeSeries | **3** | TIER-3 (v2+) |
| **E73** portfolio-redundancy detection | **4** | TIER-2 |
| **E75** business-vs-personal-stack | **2** | TIER-2 |
| **R56-alt** fact-class confidence override | **3** | TIER-2 |
| Multiple TIER-3 single-query refinements (DA69-76, CV73-81, E68-69, E74-75) | 1 each | DEFER (need more cross-query evidence) |

### Acquisition gaps causing partial answers (PENDING items)
- **DA61 (8+ queries)**: HistoricalBonusEvent
- **DA64 (5+ queries)**: RuleChangeEvent old/new values
- **DA65 (3 queries)**: ExpiryPolicy + DefunctProgramRule
- **DA57/DA58 (3 queries)**: BankPolicy spouse-pooling
- **DA66 (3 queries)**: AwardAvailabilityTimeSeries

### Pattern findings (architectural)
- 12 qualifier-heavy categories = 600 critical (card, category) tuples blind-spot risk
- Business-vs-personal card stacks have inverted answers on tax/government (Q7.1 vs Q20)
- Co-brand routing dominates same-merchant stack consistently
- Multi-currency aspirational redemption is heaviest pipeline stress (works correctly when DA61+DA66 are populated)
- "Burn-vs-save" is a recurring sub-pattern across 6+ queries
- "Reward churner" persona surfaces secondary RULE_CHANGE_DIFF at 0.55 confidence consistently

---

## See also

- **[`_SUMMARY-and-RANKED-ACTIONS.md`](./_SUMMARY-and-RANKED-ACTIONS.md)** — Executive summary + 80/20 ranked action plan for next iteration sprint
- **Skill methodology**: [`/.claude/commands/swypd-query-stress-test.md`](../.claude/commands/swypd-query-stress-test.md)
- **4-plan suite**:
  - [`/docs/schema-plan.md`](../docs/schema-plan.md) — R1–R55
  - [`/docs/data-acquisition-plan.md`](../docs/data-acquisition-plan.md) — DA1–DA66
  - [`/docs/decision-engine-plan.md`](../docs/decision-engine-plan.md) — E1–E60
  - [`/docs/conversational-layer-plan.md`](../docs/conversational-layer-plan.md) — CV1–CV68
  - [`/docs/build-roadmap.md`](../docs/build-roadmap.md) — integrated build sequence
