# qset2 U12 — Spouse-pool: Husband + Wife SBI Elite (54K + 61K RP) — pool for international ticket?

**Stack:** Husband SBI Elite + Wife SBI Elite
**Question:** *"Can we combine our SBI points for one international ticket redemption, or does SBI not really let you do that?"*
**Difficulty:** Easy (E52 BankPolicy spouse-pooling)

## Phase-0
- SBI policy on spouse-pooling: separate cardholders cannot pool RP directly
- SBI RP transfer to airlines: limited (no direct KrisFlyer; some Yatra-equivalents)
- 54K + 61K = 115K SBI RP combined if poolable

## Engine traversal (E52)
- Returns deterministic POLICY: SBI does not allow pooling across separate cardholders
- Each account redeems separately

## Conversational
- "**No, SBI doesn't let you pool RP across separate cardholders.** Each Elite balance is tied to its account.
  
  - **What you can do**:
    1. **Add-on cards**: one of you could add the other as an add-on to your Elite primary account. Spends from add-on accrue to primary. **But**: SBI add-on Elite cards are typically issued only after the primary has held the card for 6+ months. And add-on may not have full redemption authority.
    2. **Each redeem separately**: 54K + 61K RP redeemed as separate vouchers, totaling ~₹14-17K of voucher value (at ₹0.25-0.30/RP). Not bad combined, just not 'pooled'.
    3. **SBI Yatra/MakeMyTrip redemption catalog**: each redeems for travel voucher within own account → vouchers can technically be used by either spouse on a single trip booking (vouchers are typically transferable).
    
  - **For an international ticket**: SBI Elite RP at ~₹0.25-0.30 face value × 115K total = ~₹28-35K of voucher-equivalent value. That covers part of an international economy ticket but doesn't get to '1 person biz' territory.
  
  - **Comparison to better-pooling banks**:
    - **HDFC**: same — no pool. Workaround = add-on primary.
    - **Marriott Bonvoy**: allows account-holder-to-account-holder transfers up to ~50K/year (slow but real pool).
    - **No Indian credit card** offers true cross-account pooling for spouses with separate accounts.
  
  - **Practical play**:
    - Each redeems own 54K and 61K SBI Elite RP for Yatra/MakeMyTrip vouchers (~₹14-17K combined)
    - Apply vouchers to a single international ticket booking via Yatra/MMT
    - Effectively 'pooled' at the booking level even though SBI doesn't formally pool
  
  - **DA57/DA58 PENDING**: spouse-pooling BankPolicy acquisition feed would surface per-bank rules — for now, SBI = no pool is well-documented community knowledge."

## 80/20
- Existing: R49, R50, R51, R55, **DA57 + DA58 PENDING**, E1, **E52**, CV1, **CV53** (hard-policy-response)
- **SHIP-AS-IS engine answer; E52 working as designed**
