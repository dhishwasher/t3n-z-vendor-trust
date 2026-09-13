# z-vendor-trust — T3N trusted enterprise agent

Minimal, maintainable **Vendor Trust Brief** agent for the Superteam listing
[Try out new docs to build a trusted agent with T3N that we can distribute / host](https://superteam.fun/earn/listing/t3n-agent-build-challenge/)
(`657bf11b-89e4-42ea-bd96-c4339df44f2e`, HUMAN_ONLY).

Built against Terminal 3 ADK docs (Quickstart + Walkthrough): Rust WASM TEE
contract (`wasm32-wasip2`) + TypeScript client (`@terminal3/t3n-sdk@5.2.0`).

## What it does

| Export | Purpose |
|--------|---------|
| `health` | TEE liveness: tenant DID hex, contract id, cluster timestamp, seq — **no HTTP** |
| `vendor-brief` | Authorized public HTTPS GET of `/.well-known/security.txt` + homepage snippet for a vendor domain; returns a structured enterprise brief |

Trust model (matches T3N docs):

- Code runs inside T3N TEE (Intel TDX path)
- Capabilities come only from WIT imports (`tenant-context`, `logging`, `kv-store`, `http`)
- Outbound hosts are **user-authorized** via `member-delegation-update` / `allowed_hosts`
- Optional `user_agent` secret in `z:<tid>:secrets` (control-plane seed); not required for default path
- Agent card can be **hosted on T3N** (`t3n agent host-card`) for distribute/discover

## Layout

```
t3n-agent/
├── README.md                 ← this file
├── SUBMIT.md                 ← human Superteam submit checklist
├── BUGS.md                   ← docs/tooling notes for judging
├── contract/                 ← Rust TEE contract (z-vendor-trust)
│   ├── Cargo.toml
│   ├── wit/world.wit + deps/ ← host interfaces vendored from Terminal-3 reference
│   ├── src/{lib,health,vendor_brief}.rs
│   └── target/.../z_vendor_trust.wasm
└── artifacts/z_vendor_trust.wasm   ← prebuilt copy for publish (162K)
└── client/                   ← Node/TS deploy + invoke scripts
    ├── package.json
    └── src/01-whoami … 05-host-card.ts
```

## Prerequisites

1. **SSO + credits:** https://go.terminal3.io/adk-community — copy API key once (shown once).
2. Node.js ≥ 18, npm.
3. Rust + `wasm32-wasip2` (only if rebuilding WASM):
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   rustup target add wasm32-wasip2
   ```

## How to run (local → testnet)

```bash
# 0) Key (never commit)
export T3N_API_KEY='0x…'   # from claim page

# 1) (Optional) rebuild WASM
cd contract
cargo test
cargo build --target wasm32-wasip2 --release
ls -lh target/wasm32-wasip2/release/z_vendor_trust.wasm
cd ..

# 2) Client
cd client
npm install
npm run whoami            # prints did:t3n:…
npm run register          # uploads WASM, creates secrets map ACL, writes state.json
npm run invoke-health     # TEE health JSON
VENDOR_DOMAIN=example.com npm run invoke-brief
npm run host-card         # writes agent-card.json + prints host command
npx @terminal3/t3n-sdk agent host-card --file agent-card.json --env testnet
```

Self-grant path is used for `vendor-brief` (tenant DID as grantee) so a single
key can demo end-to-end. For a real agent split, claim a second key as
`AGENT_KEY` and grant that DID instead (see ADK Member Delegation).

## How to distribute / host

1. **T3N-hosted agent card** (preferred for this bounty title):
   `npx @terminal3/t3n-sdk agent host-card --file agent-card.json --env testnet`
   Card is served at the node’s `/api/agent-card/<did:t3n:…>` endpoint.
2. **Public GitHub repo** with this pack (source + README + prebuilt wasm optional).
3. **Public Google Doc** linking the repo, screenshots of `whoami` / `health` /
   `vendor-brief` output, and `BUGS.md` notes.
4. Handover: prefer **pass to Terminal 3 to run** (see SUBMIT.md) — short tail
   `vendor-trust`, no paid third-party API required, one env var (`T3N_API_KEY`).

## Maintainability choices

- No Duffel / Stripe / paid SaaS keys required for the happy path
- Domain validator rejects scheme/path/userinfo (reduces SSRF foot-guns)
- Snippets truncated to 400 chars inside the enclave
- Short contract tail `vendor-trust` (avoids downstream canonical-name length traps noted in register docs)
- Native unit tests for domain validation + health stub without WASM

## Listing criteria map

| Requirement | Status in this pack |
|-------------|---------------------|
| SSO + DID + API key | Operator step (claim page) — scripts consume `T3N_API_KEY` |
| Complete Quickstart + Walkthrough | Client follows docs connect → TenantClient → register → invoke → host-card |
| Enterprise useful agent | Vendor trust brief for security.txt / homepage |
| Easy to maintain / hand over | Dependency-light; no paid API; handover notes in SUBMIT.md |
| Public GitHub + Google Doc + screenshots + bugs | Human publish (see SUBMIT.md); BUGS.md included |
| Bonus X tag @terminal3io | Human optional (not done by agent) |

## Eligibility answers (draft)

1. **Email:** operator email (Cory)
2. **DID:** from `npm run whoami` after SSO
3. **Continue running / pass to T3?** Recommend: **pass to Terminal 3 to run** (+ handover via this README)
