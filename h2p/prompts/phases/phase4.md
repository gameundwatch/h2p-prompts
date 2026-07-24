# phase4 — tech stack selection

You are now in Phase 4. The orchestrator's First Principle and progression
rules are assumed loaded. Input differs by type (`state.md`'s `input_type`):
- **Type A**: the original set `.h2p/source/` plus the Phase 3 diagrams
  (which already encode component boundaries and state flow from intent) and
  `.h2p/phase1-analysis.md` (observations such as CDN loads).
- **Type B**: the root working copy's current stack (existing deps in
  package.json) + `.h2p/phase3-structure.md`'s target structure & migration
  plan + the diagnosed gaps (`.h2p/phase1-analysis.md`, `phase2-requirements`).
Both also reference `.h2p/phase2-requirements.md` (intent, scope, contracts)
and `.h2p/phase3-structure.md` (structure). Also consult
`prompts/references/foundations.md` (foundation catalog) — bind stack choices
to the foundations (TypeScript, validation, config strategy, render strategy,
test tooling) and reject anything no foundation requires (First Principle 2).

## Responsibility of this Phase

From the settled intent and scope, **select the tech stack conservatively,
evidence-based**.

- **Type A**: read the **source HTML through the Phase 3 diagrams** — the
  diagrams carry the intent-fit (component boundaries, state flow, contract
  shapes), so they are the higher-quality evidence for technology judgment;
  the source HTML is the concrete backing.
- **Type B**: a stack already exists. Selection here is not "pick from
  zero" but "**starting from the current stack, choose only the changes
  needed to close the future-intent gaps**". Respect sound existing choices;
  replace/add only what must change (obsolete, duplicated, insufficient for
  the future intent), based on evidence and the two-stage decision.
  Migration cost is part of the judgment (changes hit maintenance directly).

### The two-stage decision (principle for all categories)

Split selection into two stages. Never conflate them.

**Stage 1: category necessity (evidence-based, agent-decided)**
With the source HTML and Phase 3 diagrams as evidence, decide "is this
capability category needed at all?". This is derivable from evidence. If
unnecessary, cut it — that is the agent's responsibility.

**Stage 2: candidate choice (user-decided)**
For categories judged "needed" with **multiple viable candidates**, never
pick unilaterally. **Present candidates and trade-offs (learning cost,
maintainability, scale fit, bundle size, …) neutrally and let the user
choose.** Technology choice binds to the user's skill set and maintenance
responsibility.

Evidence can derive "whether it's needed"; "which one satisfies it" belongs
to the user's preference and fluency. No threshold-based auto-adoption.

### Base stance: conservative; over-engineering forbidden
The role of available cards is, if anything, to encourage the decision **not
to use them**. In Stage 1, cut unnecessary categories aggressively against
scope and evidence. **Adding an unnecessary dependency is a design defect.**

---

## Judgment material per category

### A. Frontend
For each category, keep Stage 1 (evidence of necessity) and Stage 2
(candidate presentation) separate.

- **UI library (React/Vue/Svelte, …)**
  Stage 1: from the number of component candidates and state complexity,
  decide whether a framework is needed or plain TS + Vite suffices. **If
  Phase 1 observed a framework loaded via CDN (React/Vue, …), treat that as
  the strongest evidence for adopting that framework** (it is already in
  use).
  Stage 2: if needed, present candidates and traits (ecosystem, learning
  cost, the user's fluency) and let the user choose.

- **Router**
  Stage 1: does Diagram 4 / Phase 1(d) contain navigation? **A multi-page
  input (several HTML files, one app) is first-class evidence for Router
  necessity.** If none, reject the whole category.
  Stage 2: if needed, present Router candidates matching the chosen UI
  library.

- **State management (built-in vs dedicated library)**
  Stage 1: assess the shared-state scale in Diagram 3. **No mechanical
  thresholds.** Dedicated libraries (Zustand/Jotai/Recoil, …) pay off via
  fine-grained subscriptions (re-render optimization at scale) and moving
  state logic out of the UI; with few shared states and no re-render
  bottleneck, those benefits barely apply and the costs remain (extra
  dependency, scattered state, over-design). In that band, framework
  built-ins (useState/useContext, ref/provide) win. Stage 1 presents this
  trade-off: "built-ins suffice / dedicated is warranted".
  Stage 2: only when dedicated is warranted, present trade-offs (Zustand:
  single store, minimal learning cost / Jotai: atom-level granularity but
  easily scattered / Recoil: stalled development, low recommendation for new
  projects) and let the user choose.

- **UI component set (Radix/MUI, …)**
  Stage 1: does the source HTML clearly contain accessible interactive
  widgets (modal/dropdown/tooltip)? If not, reject.
  Stage 2: if needed, present headless vs styled candidates.

- **Networking (fetch vs library)**
  Stage 1: decide from the `backend` setting and contracts. Does fetch
  suffice?
  Stage 2: if a library is warranted, present candidates (axios, or TanStack
  Query for a data-fetching layer) and let the user choose.

- **Styling strategy**
  Stage 1: use the source HTML's style situation as evidence (tokens
  variablized, or hard-coded?).
  Stage 2: present trade-offs among plain CSS / CSS Modules / Tailwind /
  CSS-in-JS and let the user choose. **Record which and why.**
  (Custom visual-design needs are absorbed into this selection.)

- **Package manager / version control / testing approach**
  Stage 1: decide necessity/strictness from scope (maintenance horizon) —
  minimal for throwaway, test infrastructure for long-term growth.
  Stage 2: multiple candidates (npm/pnpm/yarn, …) → user chooses.

Tie every Stage 1 verdict to "this evidence in the source HTML / Phase 3
diagrams". Adopting a category without evidence is forbidden.

### B. Backend — entirely the user's call (only when `backend: mock`)
Backend language/framework cannot be derived from HTML (the same contract
can be served by Node/Hono or Python/FastAPI). It binds to maintenance,
organizational skill sets, and existing assets. Therefore Stage 1 is skipped
— go straight to **neutral candidate presentation** and let the user choose.

### C. Contract method — tied to the backend language (only when `backend: mock`)
The canonical contract lives in `shared/`. The method follows B's language
choice.
- Frontend and backend both TypeScript → unify types and runtime validation
  in one schema (Zod, …) placed in `shared/` (lightweight, no generated
  intermediates).
- Cross-language (e.g. TS frontend / Python backend) → make TypeSpec /
  OpenAPI the canon and derive both sides' types by generation.
- Multiple viable methods → present trade-offs, user chooses. Whatever the
  method, the invariant holds: **the type canon lives in one place; both
  sides derive from it.**

---

## Procedure
1. Read the source HTML, Phase 2, Phase 3. **Stage 1**: decide each
   category's necessity on evidence (cut the unnecessary here).
2. **Stage 2**: for needed categories with multiple candidates, present
   trade-offs neutrally and let the user choose. With `backend: mock`, also
   put B (backend tech) and C (contract method) before the user.
3. Present the complete selection, **including rejected categories and their
   grounds**, and have the user confirm it is not over-engineered.
4. On agreement, write `.h2p/phase4-stack.md`.

## Writing the artifact
Write `.h2p/phase4-stack.md` following the structure of
`prompts/templates/stack.md` (do not change the heading structure; do not
omit rejected categories).
- Per category: necessity, verdict, adopted technology, **role in the
  project**, candidates not chosen and why.
- Styling strategy; with `backend: mock`, backend tech and contract method;
  foundation (package manager, …).
- **Scaffold commands**: the command sequence Phase 6/7 agents will execute
  via bash as-is.

If the selection settles framework-specific naming idioms (handler naming,
file naming, …), append them to the "Project-specific idioms" section of
`.h2p/ubiquitous.md` Part 2.

`state.md`: summarize in decisions the main technology choices and "what was
left out".

## Do not
- No evidence, no adoption (Stage 1); no equipping for imagined futures.
- Never pick candidates unilaterally in Stage 2. Categories with multiple
  candidates always go to the user. No threshold-based auto-adoption.
- Never choose a method that permits dual contract management (one canon).

## Completion conditions (to mark done)
- Stage 1: every category's necessity decided on evidence, with rejection
  grounds recorded.
- Stage 2: every multi-candidate category settled by user choice.
- With `backend: mock`: backend tech and contract method settled by the
  user.
- Styling strategy selected and recorded.
- Scaffold commands written.
- The user agreed to the stack.

When the user instructs progression, go through the orchestrator's
consistency check to Phase 5. Phase 5 designs the development workflow and
discipline before implementation begins.
