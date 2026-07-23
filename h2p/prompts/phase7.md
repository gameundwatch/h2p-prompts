# phase7 — backend implementation [only when `backend: mock`]

You are now in Phase 7. The orchestrator's First Principle and progression
rules are assumed loaded. **When `backend: none`, this Phase is skipped**
(the orchestrator sends you to Phase 8).

Input: `shared/` (the canonical contract finalized in Phase 6),
`.h2p/phase4-stack.md` (backend tech, contract method),
`.h2p/phase2-requirements.md` (logical contracts),
`.h2p/phase5-workflow.md` (discipline), `.h2p/phase1-analysis.md` (behavior
checklist), `.h2p/ubiquitous.md` (term ledger), and the running `frontend/`.

## Responsibility of this Phase

Following the contract solidified through Phase 6 (`shared/`), build in
`backend/` **a minimal working server that returns mock responses**. At the
exit, verify frontend and backend run integrated.

### Range line (most important)
- **In range**: a working server returning mock data per the contract, in a
  state the frontend can actually call and integrate with.
- **Out of range**: business-logic implementation; a real database; real
  authentication. The HTML holds no evidence for them, so they are not built
  — building them yields "a non-working ideal" (First Principle).

The mock must **work**, not be a hollow shell: it returns plausible mock
data per the contract, so the frontend can test integration. That is the
exit.

---

## Procedure

### 1. Scaffold the backend (execute directly via bash)
Per the backend selection in `.h2p/phase4-stack.md`, **initialize `backend/`
yourself via bash** and install dependencies. Confirm the server starts.

### 2. Binding to the contract
Reference the `shared/` contract as the **single canon**.
- If the contract method is Zod etc. (TS on both sides), import the `shared`
  schemas in the backend too and validate input/output.
- If TypeSpec/OpenAPI (cross-language), use types/validation generated from
  the canon.
- **Never redefine the contract on the backend side.** Dual management is
  the source of coupling and inconsistency.
- If a contract defect surfaces during implementation, follow Phase 6's
  "Contract amendment" rules (shape fix = approved simultaneous update of
  `phase2-requirements.md` and `shared/`; meaning change = put a Phase 2
  rollback before the user).
- Route and mock-data naming follows `.h2p/ubiquitous.md` — identifiers
  (Part 1) and naming grammar (Part 2).

### 3. Implement in the backend's 3 layers (mirror of the frontend)
- **Routing layer (routes/)**: receives requests, validates input/output per
  the contract. Depends on `shared`. Pairs with the frontend's data-access
  layer across the contract.
- **Logic layer (logic/)**: the processing body. At mock stage, assembles
  contract-conformant mock responses. The place later swapped for the real
  implementation.
- **Data layer (data/)**: the mock-data source. The swap point later
  replaced by real DB access. Consolidate mock data here; the logic layer
  references it.

This 3-layer split keeps the later mock→real swap unit small (the coupling-
prevention logic applies to mock servers too).

### 4. Wire up the frontend
Configure so the frontend's data-access layer can call the backend (dev
proxy / CORS / base URL, per the selected stack's conventions). Parts that
call public APIs directly stay as they are (only the own backend gets
proxied).

### 5. Behavior verification gate (exit) ★
- Start frontend and backend **simultaneously**.
- Confirm the frontend calls the backend's mocks through the contract and
  **works integrated**. Exercise the communication-involving items of the
  Phase 1 behavior checklist (the primary flows of Diagram 4) and confirm
  behavior equivalent to the frontend established in Phase 6. Final
  visual/interaction
  confirmation is the user's; record accepted differences in
  `approved_deviations`.
- If integration fails, fix until it holds; cannot be done before. On pass,
  commit (`h2p: P7 gate passed`) and record the hash in gates.

## Artifacts
- The `backend/` set (routes/ logic/ data/ 3 layers, startup config,
  package.json, …).
- Frontend–backend wiring config (proxy, …).
- `state.md`: `gates.p7_integration_runs` → `passed (commit <hash>)`; key
  implementation/wiring decisions → decisions.

## Do not
- Do not build business logic, a real DB, or real auth (out of range; they
  become non-working ideals).
- Do not redefine the contract in the backend (the canon is `shared`, one
  place).
- Do not flatten the layers into a single mock file (keep 3 layers — they
  are the swap units).
- Do not reduce scaffolding to a written procedure; actually execute and
  run it.
- Never mark complete without passing integration verification.

## Completion conditions (to mark done)
- `backend/` is implemented in 3 layers, bound to the contract (`shared`).
- The mock works; frontend and backend communicate successfully.
- **Behavior verification gate passed** (`p7_integration_runs: passed`).
- The user agreed to the integration result.

When the user instructs progression and the gate is passed, go through the
orchestrator's consistency check to Phase 8. Phase 8 writes up all Phase
agreements, generates the single root CLAUDE.md and the rest, and closes the
process.
