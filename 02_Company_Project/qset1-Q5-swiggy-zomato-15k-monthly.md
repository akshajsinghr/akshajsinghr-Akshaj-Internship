# qset1 Q5 — Online shopper: ₹15K Swiggy/Zomato monthly — best card

**Stack:** Axis Flipkart + Tata Neu Infinity + Amazon Pay ICICI + SBI Cashback
**Question:** *"₹15K of Swiggy/Zomato a month — which card actually maxes that out?"*
**Difficulty:** Medium (per-merchant routing within food delivery)
**Run date:** 2026-05-24

## Phase-0 retrievals
- (Axis Flipkart, Swiggy/Zomato): 4% at preferred partners (Swiggy included) capped at ₹4K/qtr
- (Tata Neu Infinity, Swiggy/Zomato): 1.5% base; not in Tata 5%; might be in Tata Neu app 5% if Swiggy is on Tata Neu app for delivery (unlikely — Swiggy isn't Tata-brand) — so 1.5%
- (Amazon Pay ICICI, Swiggy/Zomato): 1% non-Amazon
- (SBI Cashback, Swiggy/Zomato): 5% online but Swiggy/Zomato might be reclassified — verify; assume 5% capped ₹2K/cycle binding at ₹40K
- HDFC Millennia would be 5% on Swiggy + Zomato (₹1,000 cap) but not in stack

## Engine traversal
- Path A — Axis Flipkart 4% on ₹15K = ₹600/mo (well within ₹4K/qtr cap)
- Path B — SBI Cashback 5% on ₹15K = ₹750/mo (cap binds with other online spend; depends on whether food-delivery still 5%)
- Path C — Tata Neu Infinity 1.5% = ₹225/mo

## Conversational
- Narration: "**SBI Cashback IF food-delivery is still classified as 5% online (verify against your latest cashback statement) — gives ₹750/month. Otherwise Axis Flipkart's 4% on Swiggy/Zomato preferred-partner = ₹600/month.**
  - **Risk on SBI Cashback**: April 2026 expanded exclusion list added many categories; food-delivery hasn't been explicitly excluded but the 5% on online means cashback caps at ₹2,000/cycle. If your other online spend already exceeds ₹25K/mo, food-delivery cashback gets capped out.
  - **Axis Flipkart 4%** is cleaner — Swiggy/Zomato/Ola/Cult.fit/PVR are explicit preferred partners. ₹4,000/quarter cap = ₹1,333/mo equivalent — at ₹15K/mo food-delivery spend you'll earn ~₹600 (well within cap).
  - **Tata Neu Infinity**: 1.5% on non-Tata = ₹225/mo. Don't use here.
  - **Amazon Pay ICICI**: 1% on non-Amazon = ₹150/mo. Don't use here.
  - **My pick**: Axis Flipkart for reliability (4% with simple cap math), unless SBI Cashback is currently giving you 5% on food-delivery AND your other online spend is moderate (<₹25K/mo).
  - **Stack-upgrade angle**: HDFC Millennia would give 5% on Swiggy/Zomato (capped at ₹1,000/mo CashPoints = ₹1,000 face value). At ₹15K/mo food-delivery you'd hit the cap = ₹750/mo. Comparable to SBI Cashback uncapped scenario."

## 80/20 verdict
**SHIP-AS-IS**
