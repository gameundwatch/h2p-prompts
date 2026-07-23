# phase4 — structural refactoring (shared across input types)

You are now in Phase 4. The orchestrator's First Principle and progression
rules are assumed loaded. This Phase is the **first fortress** of the First
Principle: features unchanged, only structure aligned to intent and target.
At the exit, always verify that **it still works the same**.

**Behavior branches by input type** (see `state.md`'s `input_type`).

Reference (both types): `prompts/references/foundations.md` (foundation
catalog) — this Phase lays the foundations that can be established **in place
without changing behavior** (externalize literals, semantic / a11y, tokens,
error / async states, testability seams). Each still passes the P4 gate.

## Type A (HTML prototype)
Input: the original HTML set in `.h2p/source/`, `.h2p/phase1-analysis.md`
(behavior checklist), `.h2p/phase2-requirements.md` (intent), and
`.h2p/phase3-structure.md` (structure). **Keeping it working HTML**, correct
its divergences from intent and structure. Artifact: the **corrected set**
under `.h2p/source/refactored/` (same file layout as the original; one file
for a single page, several for multi-page). No technology migration
(frameworkization is Phase 7). Details under "Type A details".

## Type B (existing app)
Input: `.h2p/phase3-structure.md` (target structure & stepwise migration
plan), `.h2p/phase2-requirements.md` (gaps), `.h2p/phase1-analysis.md`
(behavior checklist), and the root **working copy** (copied from `origin/`
at boot). Execute, **against the root working copy**, the first
foundation-level steps of the migration plan's this-run range while
preserving features. `origin/` is a frozen baseline — **never modify it**
(the comparison target is always `origin/` booted up). After each step,
verify behavior and record **one step = one commit** (with the `h2p: `
prefix). Remaining steps are handed to Phase 7+ and the generated CLAUDE.md.
Artifacts: `.h2p/phase4-refactor.md` (steps executed and diffs) plus the
modified working code. Type B does not create `refactored/`.

For both types, the goal is **fit to design intent and target structure**,
not generic code-quality improvement. Features, appearance, and behavior do
not change.

---

## Type A details

### What is this refactoring for?
**"Fit to design intent" only.** Not generic quality improvement. Fix where
the current HTML diverges from the intent verbalized in Phase 2 and the
structure drawn in Phase 3. For example:
- A value organized as "shared state" in Diagram 3 is scattered across
  DOM/variables in the HTML → consolidate per the intent.
- A repeated structure marked as a component candidate in Diagram 1 exists
  as slightly-inconsistent hand copies → align the structure per the intent
  (a shape easy to componentize later).
- Dummy data in the HTML diverges from Diagram 2's contract shape → align to
  the contract.

---

## Do not (most important)

This Phase is not technology migration. **Correct in plain HTML/CSS/JS.**
- Do **not** replace with React/Vue or any framework. No componentization,
  no JSX, no build tooling (that is Phase 7).
- No new libraries. No npm initialization.
- No behavior-changing "improvements". Adding/removing features or changing
  behavior is forbidden. Only "same behavior, internals aligned to intent".
- Do not change appearance. Style consolidation (dedup, tokenization) is
  allowed within intent, but the rendered result must not change.

When in doubt, ask: "is this fit-to-intent, or ideal-stacking?" If
ideal-stacking: don't (or record in backlog for later Phases).

### Type A procedure
1. Copy the original set into `.h2p/source/refactored/` (same file layout)
   and edit that (never the originals).
2. Cross-check Phase 2 / Phase 3 against the current HTML and build a
   **divergence list**. For each: "what the intent says / current state /
   correction policy".
3. Present the list to the user and confirm whether and how to correct
   (present facts, delegate judgment). No verdicts: "this looks divergent
   from the intent — align it, or leave it?"
4. Correct the `refactored/` set within the agreed range.
5. **Behavior verification gate (exit)**: with the Phase 1 behavior
   checklist as the reference, confirm the corrected set behaves the same as
   the original.
   - The agent verifies what a machine can check (syntax, DOM structure,
     console errors, etc.).
   - Generate `.review/p4-compare.html` (original and corrected side by
     side, checklist attached — a disposable view, in `meta.language`) and
     **ask the user for final visual/interaction confirmation**.
   - Record user-accepted differences in `state.md`'s `approved_deviations`.
   - If behavior changed, redo the correction. On pass, commit
     (`h2p: P4 gate passed`) and record the hash in gates.

---

## Type B details

### Goal
Execute the **first foundation steps** of the this-run range of Phase 3's
migration plan. Foundation = what later migration depends on — typically
establishing the contract (`shared` types), extracting the data-access
layer, separating state from types. Large UI reshuffles wait for Phase 7+;
here, build "the base that makes further migration possible".

### Incrementality discipline (most important)
- **Never rewrite in one stroke.** Move step by step per the plan, and
  confirm the app works after each step.
- Old and new structures may coexist temporarily (strangler fig). Shift
  gradually into the new; remove the old only when safe.
- Features, appearance, behavior unchanged. Only structure moves.

### Type B procedure
1. From Phase 3's plan, confirm the foundation steps for this run.
2. Execute one step at a time against **the root working copy** (`origin/`
   is frozen — untouched). For each step, state "what · for which gap · how
   to verify".
3. **Verify and commit after each step**: confirm the app still works as
   before (the relevant checklist items) on the dev server, then record
   **one step = one commit**. If broken, redo the step (the previous commit
   is right there).
4. When this run's steps are done, confirm the remaining steps still stand
   as a plan in `phase3-structure.md` and organize the handoff.

---

## Writing the artifacts
**Type A**:
- `.h2p/source/refactored/` set: the corrected working HTML (same file
  layout as the original).
- `.h2p/phase4-refactor.md`: the corrected divergence list (what, why, how).
- `state.md`: `gates.p4_html_behaves` → `passed (commit <hash>)`; accepted
  differences → `approved_deviations`; essentials → decisions.

**Type B**:
- The modified working code (on the root working copy, with per-step
  commits).
- `.h2p/phase4-refactor.md`: steps executed, diffs, per-step verification
  results, and the handoff of remaining steps.
- `state.md`: `gates.p4_html_behaves` → `passed (commit <hash>)` (= app
  confirmed working after every step); decisions summarize the executed
  range and handoff.

## Completion conditions (to mark done)
**Shared**: features, appearance, behavior unchanged (accepted differences
recorded in `approved_deviations`). Behavior verification gate passed
(`p4_html_behaves: passed`) with the gate commit. `.h2p/phase4-refactor.md`
has the record. The user agreed to the result.
- **Type A**: the `refactored/` set exists, divergences from intent are
  corrected within the agreed range, and no technology migration happened.
- **Type B**: this run's foundation steps are executed, the app worked after
  every step (one step = one commit), and remaining steps are handed off as
  a plan.

Until verification passes, this Phase cannot be done. The moment it stops
working, the First Principle is being violated. When the user instructs
progression and the gate is passed, go through the orchestrator's
consistency check to Phase 5.
