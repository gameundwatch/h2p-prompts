# phase7 — frontend implementation / migration

You are now in Phase 7. The orchestrator's First Principle and progression
rules are assumed loaded. Input differs by type (`state.md`'s `input_type`):
- **Type A**: the `.h2p/source/refactored/` set (Phase 4's working HTML) is
  the reproduction target.
- **Type B**: the root working copy's current code plus Phase 3's target
  structure & migration plan (continuing the foundation begun in Phase 4).
  The object is not reproduction but **executing the remaining steps of the
  agreed migration scope**.
Both reference `.h2p/phase5-stack.md` (settled stack),
`.h2p/phase3-structure.md` (structure / target), `.h2p/phase6-workflow.md`
(discipline), `.h2p/phase2-requirements.md` (contracts),
`.h2p/phase1-analysis.md` (behavior checklist), and `.h2p/ubiquitous.md`
(term ledger).

## Responsibility of this Phase

Stand up the dev environment with the settled stack and implement
`frontend/` (target structure). Because scope includes "up to dev
environment construction", the exit is not "code written" but **"runs on the
dev server"**. This is the frontend-side **fortress of the First
Principle**.

- **Type A**: **faithfully reproduce** the refactored HTML set as
  `frontend/`. For multi-page inputs, reproduce including the page→route
  mapping.
- **Type B**: following Phase 4's foundation, execute the **remaining steps
  of the agreed migration scope**, moving features unchanged into the target
  `frontend/` (3 layers). **Features identical**; only structure shifts
  toward the target. The one-step-one-commit discipline carries over from
  Phase 4. Steps beyond the scope go to the generated CLAUDE.md (h2p's
  exit = completion of the agreed scope).

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

### 1. Scaffold and dev environment (execute directly via bash)
Read the settled stack and scaffold commands in `.h2p/phase5-stack.md` and
**execute them yourself via bash**.
- With `backend: mock`, make the root a monorepo shell and scaffold under
  `frontend/`. With `backend: none`, placement follows scope (per
  `.h2p/phase3-structure.md`'s target structure and `state.md`).
- Initialize with `npm create vite` (template per the selected stack), etc.
- Install dependencies (`npm install`, etc.; package manager per selection).
- Confirm the dev server starts at this point, once.

### 2. Assign to layers (follow Phase 3's diagrams)
Implement the refactored HTML assigned to the frontend's 3 layers.
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

### 3. Port the styles
Following Phase 5's styling strategy, port the HTML's styles. The rendered
result staying identical has top priority. Tokenized values go into that
strategy's variable mechanism; hard-coded ones become per-component.

### 4. Incremental implementation (per Phase 6's plan)
Proceed in the implementation order agreed in Phase 6 (which components
first, etc.). Verify look and behavior on the dev server per unit as you
build up.

### 5. Behavior verification gate (exit) ★
- With `npm install` done, **the dev server starts**.
- The reproduced frontend behaves and looks the same as the refactored HTML
  (Type B: the `origin/` baseline), **checked against the Phase 1 behavior
  checklist**. The agent verifies the machine-checkable range (startup,
  console errors, DOM/state transitions).
- Generate `.review/p7-compare.html` (refactored HTML and the dev server
  side by side, checklist attached — a disposable view, in `meta.language`)
  and **ask the user for final visual/interaction confirmation**. Record
  user-accepted differences in `state.md`'s `approved_deviations`.
- If not matching, fix until it matches; cannot be done before. On pass,
  commit (`h2p: P7 gate passed`) and record the hash in gates.

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
  mock is Phase 8).
- `state.md`: `gates.p7_frontend_behaves` → `passed (commit <hash>)`;
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
  refactored HTML (Type B: the agreed migration scope executed). Naming
  matches the ledger.
- **Behavior verification gate passed** (`p7_frontend_behaves: passed`) with
  the gate commit.
- With `backend: mock`, the `shared/` contract is finalized and implemented.
- The user agreed to the reproduction result.

When the user instructs progression and the gate is passed, go through the
orchestrator's consistency check to Phase 8 (with `backend: none`, Phase 8
is skipped and you go to Phase 9).
