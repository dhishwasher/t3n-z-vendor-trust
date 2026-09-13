# T3N — exact human click path (SSO → DID → Doc → Earn)

**Listing:** https://superteam.fun/earn/listing/t3n-agent-build-challenge/  
**listingId:** `657bf11b-89e4-42ea-bd96-c4339df44f2e`  
**Prize:** 290 USDC · **Deadline:** 2026-09-16 11:59am ET · **HUMAN_ONLY**

Code pack: this folder (`z-vendor-trust`). CoS pushes public GitHub as `dhishwasher`. Paste the live repo URL into the Doc + Earn form when you have it.

---

## 1) SSO (Terminal 3)

1. Open https://go.terminal3.io/adk-community
2. Click **Google SSO** / Sign in with Google (`corymaynard370@gmail.com`)
3. Complete any “claim credits / join ADK community” confirm
4. On the claim page, **copy the API key immediately** (shown once) into a password manager — never chat/commit
5. Leave the page open or bookmark the console you land on

## 2) DID

1. On the same Terminal 3 / ADK surface (or local: `cd client && npm i && export T3N_API_KEY=… && npm run whoami`)
2. Record your **DID** exactly as shown (`did:t3n:…`)
3. Still with the key set, run once (or ask Closer/CoS after key is in env, never paste key in chat):
   - `npm run register`
   - `npm run invoke-health`
   - `VENDOR_DOMAIN=example.com npm run invoke-brief`
   - `npm run host-card` then host-card CLI from README
4. Screenshot: DID / whoami, health JSON, vendor-brief JSON, host-card URL

## 3) Doc (public Google Doc)

1. New **public** Google Doc (Anyone with link can view)
2. Paste in order:
   - Listing URL
   - Public GitHub repo URL (from CoS push)
   - Your DID
   - Host-card URL
   - Screenshots from step 2
   - Short note: handover preference = **pass to Terminal 3 to run**
   - Link or paste `BUGS.md`
3. Copy the Doc share link

## 4) Earn (Superteam Submit)

1. Open https://superteam.fun/earn/listing/t3n-agent-build-challenge/
2. Sign in as Cory (human talent profile)
3. Click **Submit Now**
4. Fill eligibility fields:
   - **Email:** `corymaynard370@gmail.com`
   - **DID:** from step 2
   - **Continue running / pass to us:** prefer **pass to Terminal 3**
   - **Link:** public GitHub repo URL
   - Attach / link the **public Google Doc**
5. Submit · do **not** use agent API submit
6. Optional later: X tag `@terminal3io` (not required for this pack path)
7. Payout later via Superteam claim; prefer Base USDC to `0x435CeB16bC21a8c19E7c84853b84c30CE82A7103` when the rail allows

---

Cookie Chain: **DEAD** (X-gated) — ignore.
