# phase6 — frontend implementation / migration

You are now in Phase 6. The orchestrator's First Principle and progression
rules are assumed loaded. Input differs by type (`state.md`'s `input_type`):
- **Type A**: the original set `.h2p/source/` is the reproduction target,
  **corrected to Phase 2/3 intent as you port** (the fit-to-intent step
  below). The gate is against the original.
- **Type B**: the root working copy's current code plus Phase 3's target
  structure & migration plan. The object is not reproduction but **executing
  the agreed migration scope on the working copy** — foundation steps first
  (establish the contract / `shared`, extract the data-access layer, separate
  state from types), then the rest — all against the settled stack.
Both reference `.h2p/phase4-stack.md` (settled stack),
`.h2p/phase3-structure.md` (structure / target), `.h2p/phase5-workflow.md`
(discipline), `.h2p/phase2-requirements.md` (contracts),
`.h2p/phase1-analysis.md` (behavior checklist), and `.h2p/ubiquitous.md`
(term ledger).

## Responsibility of this Phase

Stand up the dev environment with the settled stack and implement
`frontend/` (target structure). Because scope includes "up to dev
environment construction", the exit is not "code written" but **"runs on the
dev server"**. This is the frontend-side **fortress of the First
Principle**.

- **Type A**: **faithfully reproduce** the original set as `frontend/`,
  corrected to Phase 2/3 intent (see the fit-to-intent step below). For
  multi-page inputs, reproduce including the page→route mapping.
- **Type B**: execute the **agreed migration scope** on the working copy —
  **foundation steps first** (contract / `shared`, data-access extraction,
  state/type separation: the base later steps depend on), then move features
  unchanged into the target structure. **Never rewrite in one stroke**:
  strangler-fig, **one step = one commit**, verify the app still works after
  each. **Features identical**; only structure shifts toward the target.
  Steps beyond the scope go to the generated CLAUDE.md (h2p's exit =
  completion of the agreed scope).

### Do not change features (most important)
Regardless of type, **no changes to features, appearance, or behavior**.
- Type A: ask "was this in the HTML?" If not, do not build it.
- Type B: ask "is this a structural change in the migration plan, or a
  feature change?" If a feature change: don't (→ backlog). Only structure
  moves.
- Anything doubtful goes to backlog, delegated to post-migration development
  (the generated CLAUDE.md).

This fortress guarantees the design intent and features reach the project
undistorted.

---

## Procedure

### 1. Dev environment with the settled stack (execute directly via bash)
Read the settled stack and commands in `.h2p/phase4-stack.md` and **execute
them yourself via bash**.
- **Type A**: scaffold fresh. With `backend: mock`, make the root a monorepo
  shell and scaffold under `frontend/`; with `backend: none`, placement
  follows scope (per `.h2p/phase3-structure.md` and `state.md`). Initialize
  with `npm create vite` (template per the selected stack), etc.
- **Type B**: do **not** re-scaffold — the working copy already runs. Apply
  only the stack changes settled in Phase 4 to the existing project (add/swap
  deps, config), in place.
- Install dependencies (`npm install`, etc.; package manager per selection).
- Confirm the dev server starts at this point, once.

### 2. Fit to intent / foundation first (before the bulk build)
Both types do the fit-to-intent and in-place foundations **here**, before the
bulk implementation — never as a behavior change; the gate is against the
baseline (Type A: the original; Type B: `origin/`).
- **Type A**: build a **divergence list** — where `.h2p/source/` diverges
  from Phase 2 intent and Phase 3 structure (e.g. shared state scattered
  across DOM/variables → consolidate; inconsistent hand-copied structures →
  align for componentization; dummy data off the contract shape → align).
  Present it to the user (facts, delegate judgment: "this looks divergent from
  the intent — align it, or leave it?"), then apply the agreed corrections as
  you port.
- **Type B**: execute the **foundation steps** of the migration plan first
  (contract / `shared`, data-access extraction, state/type separation) —
  strangler-fig, one step = one commit — then the rest below.
- **Both**: lay the in-place foundations (externalize literals, semantic /
  a11y, tokens, error / async states, testability seams —
  `references/foundations.md`). **Behavior, appearance, and interactions must
  not change.** Anything that would change behavior → backlog, not here.

### 3. Assign to layers (follow Phase 3's diagrams)
Implement the (intent-corrected) HTML assigned to the frontend's 3 layers.
- **UI layer (components/)**: presentation. Materialize the component
  candidates observed in Phase 1 and organized in Phase 3. Knows neither the
  contract nor logic directly.
- **Logic layer (logic/ — naming stack-dependent)**: business rules and
  state control. Depends on neither presentation nor API. Implements
  Diagram 3's state structure.
- **Data-access layer (api/)**: communication. The only layer that depends
  on the `shared` contract (or the public API's contract). Consolidate the
  HTML's fetch / dummy-data references here.
- Preserve the Diagram 2 (types) / Diagram 3 (state) separation in the
  implementation. Do not mix.
- **Naming follows `.h2p/ubiquitous.md`**: type, variable, component, and
  endpoint names match Part 1 (ledger) identifiers; identifier construction
  follows Part 2 (naming grammar: verb dictionary, modifier rules). If a
  concept needed by the implementation is not in the ledger, confirm with
  the user and register it first. No silent paraphrasing or ad-hoc
  translation.

### 4. Port the styles
Following Phase 4's styling strategy, port the HTML's styles. The rendered
result staying identical has top priority. Tokenized values go into that
strategy's variable mechanism; hard-coded ones become per-component.

### 5. Incremental implementation (per Phase 5's plan)
Proceed in the implementation order agreed in Phase 5 (which components
first, etc.). Verify look and behavior on the dev server per unit as you
build up.

### 6. Behavior verification gate (exit) ★
- With `npm install` done, **the dev server starts**.
- The reproduced frontend behaves and looks the same as the baseline —
  **Type A: the original `.h2p/source/`; Type B: the `origin/` baseline** —
  **checked against the Phase 1 behavior checklist**. The agent verifies the
  machine-checkable range (startup, console errors, DOM/state transitions).
- Generate `.review/p7-compare.html` (the baseline and the dev server side by
  side, checklist attached — a disposable view, in `meta.language`) and **ask
  the user for final visual/interaction confirmation**. Record user-accepted
  differences in `state.md`'s `approved_deviations`.
- If not matching, fix until it matches; cannot be done before. On pass,
  commit (`h2p: P6 gate passed`) and record the hash in gates.

### Contract amendment (when `backend: mock` / public API in use)
If a contract defect surfaces during implementation, classify the change:
- **Shape fix** (missing field, type granularity, required/optional
  mistake, naming alignment to the ledger — anything where **app behavior
  does not change**): no rollback to Phase 2 needed. Present the diff to the
  user, get approval, then update `.h2p/phase2-requirements.md`'s Contracts
  section and `shared/` **simultaneously**, and add one line to decisions.
  Fixing only one side — splitting the canon — is forbidden.
- **Meaning change** (adding/removing resources, changing an exchange's
  meaning): this is a requirements change and must not masquerade as a shape
  fix. Put a rollback to Phase 2 before the user.

The criterion: "does app behavior change?" When in doubt, treat it as a
meaning change.

## Artifacts
- The `frontend/` set (3-layer src, vite.config, package.json, …).
- With `backend: mock`, finalize and implement `shared/` (the canonical
  contract) at this stage (the data-access layer depends on it; the backend
  mock is Phase 7).
- `state.md`: `gates.p6_frontend_behaves` → `passed (commit <hash>)`;
  accepted differences → `approved_deviations`; key implementation decisions
  → decisions; ideals foregone during reproduction → backlog.

## Do not
- No ideals (no new features, behavior improvements, or visual changes).
  Reproduce, period.
- No layer mixing (no logic or networking inside UI). No contracts directly
  in the UI.
- Do not reduce scaffolding to a written procedure. **Actually execute via
  bash** and produce a running environment.
- Never mark complete without passing verification.

## Completion conditions (to mark done)
- The dev environment is built; the dev server starts.
- The frontend is implemented in 3 layers, faithfully reproducing the
  original corrected to intent (Type B: the agreed migration scope executed).
  Naming matches the ledger.
- **Behavior verification gate passed** (`p6_frontend_behaves: passed`) with
  the gate commit.
- With `backend: mock`, the `shared/` contract is finalized and implemented.
- The user agreed to the reproduction result.

When the user instructs progression and the gate is passed, go through the
orchestrator's consistency check to Phase 7 (with `backend: none`, Phase 7
is skipped and you go to Phase 8).
