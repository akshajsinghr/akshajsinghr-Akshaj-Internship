# qset2 U13 — Orphaned points: legacy Magnus + Citi migrated + orphaned hotel — any use?

**Stack:** Axis Magnus (legacy) + Citi PremierMiles (migrated); 73K legacy + 39K migrated + 22K orphaned hotel
**Question:** *"Half these programs changed or moved over and I honestly lost track — do I still have any good use here or are these basically stranded balances now?"*
**Difficulty:** Medium (E59 defunct-program + multi-program disambiguation)

## Phase-0 retrievals
- **Axis Magnus (legacy / wound down)**: Axis transitioned Magnus → Magnus Burgundy / Magnus Visa; legacy Magnus points typically migrated to Magnus Burgundy account or to EDGE Reward Points
- **Citi PremierMiles migrated**: Citi exited India retail (2022-23); points likely auto-migrated to Axis EDGE Miles
- **Orphaned hotel points**: depending on which hotel program (Hilton, IHG, etc.), each has its own expiry + redemption rules
- **DA65 DefunctProgramRule PENDING** — engine surfaces general patterns only

## Engine traversal (E59 + E51 cross-program)
- Identify: which program holds each balance currently
- Per-program redemption options
- Don't try to consolidate across programs

## Conversational
- "**The first job is identifying where these balances actually are now.** Without that, I can't give exact redemption guidance. Here's what I'd check:
  
  1. **Legacy Magnus points (73K)**:
     - Were you migrated to Magnus Burgundy or to Atlas? Log into Axis Bank netbanking → Credit Cards → look for any 'Magnus' or 'Atlas' card; check EDGE Reward Points balance.
     - If migrated to Magnus Burgundy: these became EDGE Reward Points; usable on standard Magnus terms (1.2L+/yr Tier 2 transfer cap, partner transfers at 5:2 standard or 5:4 Burgundy).
     - If your Magnus account closed without migration: points may have been forfeited or paid out as statement credit at low rate.
  
  2. **Citi PremierMiles 39K (migrated)**:
     - Citi India exited; most balances migrated to Axis EDGE Miles (if you had a corresponding Axis account opened during migration).
     - Log into Axis → EDGE Miles balance. If 39K Axis EDGE Miles appears, that's your Citi PremierMiles converted.
     - **Active redemption options on Axis EDGE Miles**: transfer to KrisFlyer 5:2, Marriott Bonvoy 2:1, partner programs.
     - If migration was incomplete: contact Axis customer service with proof of original Citi balance.
  
  3. **Orphaned hotel points (22K)** — depends entirely on which hotel program:
     - **Hilton Honors**: ~12 months inactivity → expiry. Check Hilton Honors account.
     - **IHG One Rewards**: ~12 months inactivity → expiry.
     - **World of Hyatt**: ~24 months inactivity → expiry.
     - **Marriott Bonvoy**: ~24 months inactivity → expiry.
     - Each program has different keep-active rules (typically: any activity once a year keeps points alive).
  
  **Best move**:
  1. **Today**: identify which programs currently hold each balance (login to each + match against tax-year statements).
  2. **This week**: redeem any balances at risk of expiry (in ascending order of expiry date).
  3. **Don't try to consolidate** these across programs — that's not how loyalty programs work (per E51).
  
  **For each cluster**:
  - Migrated Axis EDGE Miles (from Citi or Magnus): use as standard Axis EDGE — transfer to KrisFlyer for flights if you have an Asia trip planned.
  - Standalone Magnus legacy points (if not migrated to EDGE): likely terminal; check redemption catalog one last time.
  - Orphaned hotel points: redeem for any night-stay you'd take in next 12-24 months OR convert via partner-mile programs (some hotel programs convert to miles at low rates).
  
  **DA65 caveat**: defunct-program acquisition feed is pending. My analysis reflects general industry patterns + community knowledge of the Citi/Axis transition. **Verify against your specific account migration emails** (you should have received transition communications in 2022-2023)."

## 80/20
- Existing: R49, R50, R51, **DA24** (RuleChangeEvent for migration history), **DA65 PENDING**, E1, **E51** (cross-program), **E59** (expiry-driven), CV1, **CV60** (RULE_CHANGE_DIFF for migration explanation), **CV61** (expiry intent)
- **DA65 PENDING is HIGH-priority** — Q12 + U13 both blocked
- **SHIP-AS-IS engine answer with DA65 PENDING caveat**
