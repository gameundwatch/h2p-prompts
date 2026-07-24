# phase3a — structure / flow analysis [Type A: HTML prototype]

You are now in Phase 3. The orchestrator's First Principle and progression
rules are assumed loaded. Input: `.h2p/phase1-analysis.md` (observation),
`.h2p/phase2-requirements.md` (intent, scope, contracts), and
`.h2p/ubiquitous.md` (term ledger). Also consult
`prompts/references/foundations.md` (foundation catalog) — let its structural
seams and contracts shape the diagrams.

## Responsibility of this Phase

Transcribe the intent and contracts settled in Phase 2 into **four Mermaid
diagrams**. Diagrams are agreement devices that crush perception gaps, and
they become the blueprints for implementation (Phase 6/7).

Substantive agreement was finished in Phase 2, so this Phase is not heavy
debate but **"draw, show, fine-tune"**. Pick up the contradictions and gaps
that only become visible once drawn, and settle details with the user. Do
not add new intent here (if the urge arises, that is a rollback-to-Phase-2
decision).

---

## The four diagrams

### Diagram 1: application structure
Component/module composition and dependencies.
- Represent the "repeated structures" observed in Phase 1(a) as component
  candidates.
- **Make layers explicit**: UI / logic / data access (frontend 3 layers).
  With `backend: mock`, also draw the backend's routing / logic / data
  layers and the `shared` contract both sides depend on.
- Show dependency direction with arrows so the design intent — "UI does not
  know logic or the contract directly", "only the data-access layer depends
  on the contract" — is readable from the diagram.

### Diagram 2: data types
The structure of the data the app handles (entities and relations).
- Diagram the logical contracts defined in Phase 2's contract elevation as
  types.
- With `backend: mock`, this becomes the blueprint of `shared` (the
  canonical contract).
- ★Keep it **strictly separate from Diagram 3 (state)**. Data types
  (persisted/communicated shapes) and transient UI state are different
  things. This separation is the origin of coupling prevention.

### Diagram 3: state structure
The structure and location of frontend-held state.
- Classify the "changing values" observed in Phase 1(c) into local / shared
  / server-derived state.
- Show which component/layer each piece of state belongs to.
- Mind the difference from Diagram 2: Diagram 2 is "contracts exchanged",
  Diagram 3 is "state the screen needs to run". Mixing them breeds
  coupling.

### Diagram 4: flow chart
Primary user-operation flows (matching Phase 2's How).
- Branches and transitions from the user's starting point to goal.
- Reflect the navigation/view switching observed in Phase 1(d).
- Show where communication (exchanges through the contract) occurs in the
  flow.

---

## Procedure

1. Read the inputs and draft the four diagrams in Mermaid. **Element names
   (components, types, state) use identifiers from `.h2p/ubiquitous.md`.**
   If a concept needed by a diagram is not in the ledger, confirm with the
   user and register it first, then draw.
2. Present to the user and confirm per diagram: "does this structure/flow
   match the intent?" Point out contradictions, gaps, and ambiguities made
   visible by drawing; settle the details.
   If the user has no Mermaid preview environment, generate
   `.review/p3-diagrams.html` (a disposable view that renders the Mermaid,
   in `meta.language`) for them. **The canonical form is always the Mermaid
   code in `phase3-structure.md`**; the view's deletion loses nothing.
3. Once agreed, write all four diagrams into `.h2p/phase3-structure.md`.
   Structure and per-diagram Mermaid notation follow
   `prompts/templates/structure.md` (do not change the heading structure;
   for Type A the "Migration plan" section is "N/A").
4. Summarize key structural decisions (layer assignment, state/contract
   separation policy) in `state.md` decisions.

## Do not
- Do not **add** new features or intent here. Diagrams visualize Phase 2's
  agreement; they are not an expansion site. If additions become necessary,
  put a rollback to Phase 2 before the user.
- Do not **mix** data types (Diagram 2) with state (Diagram 3). Mixing them
  calcifies coupling downstream.
- Do not introduce technology-specific expressions (concrete library names,
  file layouts — those are Phase 4+). Diagrams stay logical.
- Do not bring names not in the ledger into diagrams. Naming's canon is the
  ledger (registration first).

## Completion conditions (to mark done)
- The four diagrams are written in `.h2p/phase3-structure.md`.
- Diagram 2 (types) and Diagram 3 (state) are separated.
- With `backend: mock`, Diagram 1 reflects the layer structure and `shared`,
  and Diagram 2 reflects the contracts.
- The user has agreed to all four diagrams.

When the user instructs progression, go through the orchestrator's
consistency check to **Phase 4** (tech stack). The HTML is corrected to the
intent and structure settled here later, during the Phase 6 build, which ports
directly from the original and gates against it.
