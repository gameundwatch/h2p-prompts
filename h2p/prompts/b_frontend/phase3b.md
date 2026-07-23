# phase3b — redesign & migration plan [Type B: existing app]

You are now in Phase 3 (Type B). The orchestrator's First Principle and
progression rules are assumed loaded. Input: `.h2p/phase1-analysis.md`
(inventory & diagnosis), `.h2p/phase2-requirements.md` (current intent,
future intent, gaps, scope), `.h2p/ubiquitous.md` (term ledger), and the
`origin/` originals. Also consult `prompts/references/foundations.md`
(foundation catalog) — let its structural seams and contracts shape the
redesign and migration plan.

## Responsibility of this Phase

Type A drew Mermaid diagrams to "give structure". Type B already has
structure. So build two things here:

1. **Target structure (redesign)** — define, as Mermaid diagrams, the
   "structure as it should be" that closes Phase 2's gaps and supports the
   future intent.
2. **Stepwise migration plan** — the path from current to target,
   **incremental and feature-preserving**. This is Type B's most important
   step.

The core of the First Principle concentrates here: features unchanged,
structure moved into a form that withstands future growth. **Never rewrite
in one stroke.** Each step keeps "does it still work the same?" true
(strangler fig: keep the old structure while gradually shifting into the
new, removing the old only when safe).

---

## 1. Target structure (redesign)
Based on Phase 2's gaps and future intent, draw the structure as it should
be — the same four diagrams as Type A's Phase 3, but conscious of the
**delta from the current state**.
- **Application structure**: the target layer composition (frontend 3
  layers = UI / logic / data access; with `backend: mock`, the backend 3
  layers and the `shared` contract). How currently-mixed layers get
  separated.
- **Data types**: the canonical contract. Where the types currently
  scattered across components get consolidated.
- **State structure**: how state locations get organized. Separation of
  types (contract) and state.
- **Flow chart**: primary usage flows (features identical to current; only
  structure changes).

**The target structure must stay reachable from the current state.** It is
an ideal form, but a realistic one reachable step by step. Do not draw
unreachable ideals (First Principle 2).

## 2. Stepwise migration plan ★most important
Decompose the path from current to target into a **sequence of small
steps**, each satisfying:
- **Self-contained; the app still works after each step** (features never
  break).
- Small change unit per step (reviewable, verifiable).
- Dependency-ordered (foundation first: contract establishment → data-access
  extraction → UI cleanup, etc.).

Attach to each step: "what · why (which gap it closes) · how to verify".

### Drawing the exit line (scope)
Following the migration scope decided in Phase 2, separate **steps executed
in this run** from **steps left as plan and delegated to the generated
CLAUDE.md**. h2p's exit is "completion of the range agreed here"; steps
beyond it are handed to normal development governed by the generated
CLAUDE.md. Full completion of all steps is never forced. Agree with the user
on how far this run goes.

---

## Procedure
1. Read the inputs (diagnosis, gaps, future intent, ledger) and origin.
2. Draft the four target-structure diagrams and present them. **Element
   names use identifiers from `.h2p/ubiquitous.md`** (unregistered concepts
   get registered first). Confirm the deltas serve the future intent and are
   reachable from the current state.
   If the user has no Mermaid preview environment, generate
   `.h2p/review/p3-diagrams.html` (a disposable HTML view rendering the
   Mermaid). The canonical form is always the Mermaid code in
   `phase3-structure.md`.
3. Build the migration plan as a step sequence with dependency order and
   per-step verification. Draw the line between this run's range and the
   delegated range with the user.
4. Write the target diagrams and migration plan to
   `.h2p/phase3-structure.md`.

## Writing the artifact
Write `.h2p/phase3-structure.md` (same filename as Type A) following the
structure and per-diagram Mermaid notation of
`prompts/templates/structure.md`:
- the four target diagrams (deltas from current explicitly noted);
- the stepwise migration plan (one step = one block; dependency order;
  verification per step);
- the scope line: executed in this run / delegated to CLAUDE.md.

`state.md`: summarize in decisions the target-structure essentials and the
step range of the migration plan.

## Do not
- Do not plan a one-stroke rewrite. Keep the migration in small steps where
  the app works after each.
- Do not draw a target structure unreachable from the current state.
- Do not include structural changes that do not serve the future intent
  (over-engineering).
- Do not mix feature additions/changes into the plan. Only structure moves.

## Completion conditions (to mark done)
- The four target diagrams are drawn with deltas and reachability.
- The migration plan stands as a sequence of small, always-working steps.
- The line between this run's range and the delegated range is drawn.
- The user has agreed to the target structure and the migration plan.

When the user instructs progression, go through the orchestrator's
consistency check to Phase 4. From here on (Phases 4–9) the flow is shared
between types; Phase 4 begins executing the first foundation steps of the
migration plan while preserving features.
