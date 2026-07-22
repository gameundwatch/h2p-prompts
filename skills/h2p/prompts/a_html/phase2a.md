# phase2a — requirements (5W1H), scope, contract elevation [Type A: HTML prototype]

You are now in Phase 2. The orchestrator's First Principle and progression
rules are assumed loaded. Input: `.h2p/phase1-analysis.md` (Phase 1's
observed facts).

## Responsibility of this Phase

The heaviest Phase in the whole flow. Four jobs:

1. **Requirements via 5W1H** — work backward from the HTML artifact to put
   into words "what was this trying to be?".
2. **Scope decision** — frontend-only vs. backend-integrated; maintenance
   horizon. This branches all downstream structure, layer thickness, and
   pruning strictness.
3. **Contract elevation** — translate the frontend's raw demands into
   "contracts sound as an API". The single most important step for cutting
   tight coupling off at the source.
4. **Establishing the ubiquitous-language ledger** — fix the "names" of the
   core concepts appearing in the settled intent and contracts in
   `.h2p/ubiquitous.md`. If the contract is the canon of data shapes, the
   ledger is the **canon of meaning and naming**.

If Phase 1 was "facts", Phase 2 is "intent". Settle, through dialogue with
the user, the author's intent behind the observed facts. Intent cannot be
fully inferred — **ask and fill through dialogue**; that is the essence of
this Phase. Do not assert; show observations and ask.

---

## Procedure

### 1. Requirements via 5W1H
Using Phase 1's observations as leads, settle the following through
dialogue. Ask the user about everything and obtain agreement. For what seems
clear from observation, confirm: "observationally it looks like X — is that
right?" For unknowns, ask plainly.

- **Why**: what problem does this app solve? Its reason to exist.
- **Who**: intended users. Multiple roles (general user / admin)?
- **What**: the central features and value. What is the MVP core?
- **When/Where**: usage context. Continuous or momentary; which environment?
- **How**: the primary usage flows (how a user operates to reach the goal).
  Map to Phase 1's (b)(d) observations.

**Start from Why.** Once the reason is fixed, later scope decisions and
over-engineering restraint stop wobbling.

### 2. Scope decision
The frontend is always built. Two axes to settle:

**Axis A: backend presence**
Judge from Phase 1's (f) backend-demand traces; record `none` or `mock` in
`state.md`'s `backend`. Read external-API traces by kind:
- **Traces of self-owned persistence/auth** (dummy data, localStorage
  stand-ins, target-less forms, UI-only auth) → a backend is being demanded.
  Candidate for `mock`.
- **External-API traces** split by CORS and key secrecy:
  - Public APIs callable directly from the browser (maps, weather — CORS
    enabled, key exposure acceptable) → the frontend can call them directly;
    this alone may stay `none`.
  - CORS-forbidden APIs, or APIs whose keys must stay secret → cannot be
    called from the frontend; a backend is needed as proxy/BFF. Candidate
    for `mock`.
- Present these as facts and ask the user: include a backend (`mock`) or not
  (`none`)? Traces are judgment material, not compulsion.

**Axis B: maintenance horizon**
- Ask: throwaway/short-term, or long-term growth?
- The **number** of layers does not change (3 layers is the baseline). The
  horizon adjusts build depth and the strictness of the dev workflow
  (Phase 6).

Once settled, record in decisions with rationale. If `backend: none`, tell
the user Phase 8 will be skipped.

### 3. Contract elevation ★most important step
**Required when `backend: mock`.** (With `backend: none` but direct public-
API use, lightly organize that API's contract for the data-access layer to
reference, but do not create a canonical `shared/`.)

The "raw demands" observed in Phase 1 are shaped by the frontend's internal
convenience (data shaped for display, etc.). Copying that shape onto the API
boundary creates coupling where the backend chases every frontend change.

So **elevate raw demands into contracts sound as an API**:
- Straighten frontend-convenient flattening / screen-specific shapes into
  honest resource representations.
- Define each exchange as a contract — "for this request, this response"
  (types, required/optional, meaning).
- Language-agnostic logical contracts suffice at this point (the concrete
  schema method — Zod / TypeSpec, etc. — is chosen with the tech in
  Phase 5).

**Principle: what is derived backward is the contract, not the
implementation.** The contract is "the honest representation of this
resource", not "the shape the frontend happens to want right now". Shaving
off frontend convenience here prevents all later coupling.

### 4. Establish the ubiquitous-language ledger ★required artifact
Read `prompts/templates/ubiquitous.md` and generate `.h2p/ubiquitous.md`
following its structure.
- **Part 1 (ledger)**: enter the core concepts (entities, operations,
  states, roles) that appeared in the 5W1H and contract elevation. Fixing
  the user-language ⇄ English-identifier correspondence uniquely is the
  heart of it.
- **Part 2 (naming grammar)**: include the template's fixed content verbatim
  (never generate it).
- Operating rules (append-only; register before use; renaming = rollback to
  Phase 2) follow the template.

### 5. Write the artifacts
Write `.h2p/phase2-requirements.md` following the structure of
`prompts/templates/requirements.md` (do not change the heading structure;
mark sections unused by Type A as "N/A").

Update `state.md`: `backend` settled, Phase 8 treatment (normal/skipped),
decisions appended.

---

## Do not
- Do not **assert intent by speculation**. Ask what is unknown. What will
  not settle even when asked stays as "deferred" — never force it.
- Do not **widen scope on your own**. Traces do not compel integration.
  Judge against Why; cut what the user does not need.
- Do not **let frontend convenience into the contract**. Compromise here and
  coupling calcifies.
- Do not propose new features. Phase 2 verbalizes "what the working HTML
  was", not ideals stacked on top (First Principle).

---

## Completion conditions (to mark done)
- The 5W1H is settled through dialogue and written in
  `phase2-requirements.md`.
- `backend` (Axis A) and the maintenance horizon (Axis B) are settled and
  recorded in `state.md`.
- With `backend: mock` (or public-API use), the logical contracts are
  written.
- `.h2p/ubiquitous.md` is established (ledger with user-language ⇄ English
  correspondence, definitions, origins).
- The user has agreed to requirements, scope, contracts, and the ledger.

When the user instructs progression, go through the orchestrator's
consistency check to Phase 3. Phase 3 turns the settled intent and contracts
into Mermaid diagrams.
