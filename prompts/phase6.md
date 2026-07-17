# phase6 — development workflow design

You are now in Phase 6. The orchestrator's First Principle and progression
rules are assumed loaded. Input: `.h2p/phase2-requirements.md` (intent,
scope), `.h2p/phase5-stack.md` (settled stack), and
`.h2p/phase3-structure.md` (structure).

## Responsibility of this Phase

**Fix the development discipline before implementation (Phase 7/8)**: in
what cycle, with what verification, will implementation proceed. Discipline
first, implementation second.

This Phase generates no code or project yet. What gets decided is "how to
proceed". The discipline decided here governs the implementation Phases.

### Scale discipline to scope (most important)
Phase 2's Axis B (maintenance horizon) sets this Phase's weight.
- **Throwaway / short-term**: minimal discipline. Light verification that it
  works; no heavyweight PDCA, CI, or strict review rules (excessive).
- **Long-term growth**: design a sustainable cycle including tests, review,
  and release procedure.
Confirm with the user which way to lean. **Heavy gear on a throwaway is as
wrong as no discipline on long-term growth.** Do not pick a middle on your
own; follow the scope.

---

## What to decide

### 1. Verbalize the development purpose and philosophy
Why, and with what values, this project is developed and maintained.
Translate Phase 2's Why into developer guidance (what to prioritize, what to
sacrifice, what counts as good code). This becomes the core of CLAUDE.md
(Phase 9).

### 2. Visualize the development flow as 5W1H
How development (ongoing work after this tool's migration ends) will run.
- Who: who develops/maintains (individual/team).
- What: the unit of change (per feature / per component, …).
- When: what happens at which timing (implement → verify → integrate).
- Where: which branch strategy, which environments.
- Why: why this flow (consistency with scope and team size).
- How: the concrete procedure.
A Mermaid flowchart is welcome.

### 3. Build the design/implement/test/release cycle
Define the PDCA-like iteration at scope-appropriate weight.
- **Design**: what to check before changing (consistency with Phase 3
  diagrams and contracts).
- **Implement**: discipline that preserves the layer structure (frontend 3
  layers / backend 3 layers); rules that keep contract-mediated decoupling
  intact.
- **Test/verify**: how the Phase 5 testing approach runs; at minimum, how
  each implementation Phase's behavior verification gate is satisfied.
- **Release**: build/deploy policy (only as far as scope demands; a
  throwaway may simplify to "runs locally").

### 4. Agree how Phase 7/8 will proceed
Decide the granularity for the implementation Phases now.
- From which units of Diagram 1 the frontend gets implemented (component
  order, …).
- How each unit's completion is confirmed (mapping to the exit gates).
- With `backend: mock`: the order — contract finalized → frontend
  implementation → backend mock — and the timing of integration checks.

---

## Procedure
1. Read the inputs (intent, scope, stack, structure).
2. Starting from Axis B (maintenance horizon), agree with the user on the
   weight of discipline.
3. Settle items 1–4 through dialogue, checking each stays scope-appropriate.
4. Write `.h2p/phase6-workflow.md`, structured.

## Writing the artifact
Write to `.h2p/phase6-workflow.md`:
- Development purpose and philosophy (material for CLAUDE.md).
- The 5W1H development flow (Mermaid if useful).
- The design/implement/test/release cycle definition (scope-appropriate
  weight).
- How Phase 7/8 will proceed (implementation order, verification timing).

`state.md`: summarize in decisions the discipline-weight policy and key
choices.

## Do not
- No heavy gear disproportionate to scope (CI / heavyweight review for a
  throwaway); conversely, do not drop verification discipline for long-term
  growth.
- Do not generate code or projects here (that is Phase 7/8).
- Do not stack an ideal development regime beyond the First Principle. Stay
  within what exists plus what the scope demands.

## Completion conditions (to mark done)
- The discipline weight follows scope (Axis B).
- Purpose/philosophy, the 5W1H flow, the PDCA cycle, and the Phase 7/8
  approach are written in `.h2p/phase6-workflow.md`.
- The user agreed to the development workflow.

When the user instructs progression, go through the orchestrator's
consistency check to Phase 7. Phase 7 implements the frontend under the
discipline decided here.
