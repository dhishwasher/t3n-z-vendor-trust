# Bugs / docs friction notes (for judging)

Collected while building z-vendor-trust against docs.terminal3.io ADK (2026-09-13).

1. **Public listings API strips description** — `GET /api/listings` returns empty `description` for this bounty; full requirements only appear in the listing HTML / JobPosting JSON-LD. Agents/humans must scrape the page, not rely on the list API alone.

2. **Docs WIT versions vs reference repo** — Walkthrough prose cites `host-interfaces@2.2.0` / `host-tenant@1.2.0`, but `Terminal-3/z-tenant-flight` still vendors `2.1.0` / `1.0.0`. This pack follows the **reference repo** pins so `wit-bindgen` links cleanly. Clarify which pin is canonical for newcomers.

3. **Invoke docs snippet duplicates `fetchTrustedManifest` import** and repeats `trustAnchor` fields in the sample `T3nClient` constructor (copy-paste drift in walkthrough/invoke-contract).

4. **System Rust without rustup** — some CI/box images ship `/usr/bin/rustc` without `wasm32-wasip2` std. `rustc --print target-list` lists the target even when the std component is missing; the real fix is `rustup target add wasm32-wasip2`. Docs already say this; worth a one-line “target-list ≠ installed” note.

5. **Contract re-register stale `contract_id`** — register docs correctly warn that map ACLs can point at a stale id after re-register; there is still no “lookup current contract_id by tail” helper. This pack writes `state.json` to mitigate.

6. **API key shown once** — claim page behavior is correct for security but easy to lose during a bounty; a “rotate/reissue sandbox key” affordance would reduce support load (noted as product feedback, not a blocker).

None of these blocked a minimal working pack; all are documentation / DX polish.
