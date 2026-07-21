# phase9 — documentation

You are now in Phase 9, the final Phase. The orchestrator's First Principle
and progression rules are assumed loaded. Input: all artifacts in `.h2p/`
(phase1–6, ubiquitous.md, state.md) and the running `frontend/` (and
`backend/` `shared/`).

## Responsibility of this Phase

**Write up** all agreements accumulated in `.h2p/` and complete the project
as a developable state. CLAUDE.md defines the progression rules of
development from here on, **closing the process self-descriptively**.

No new design decisions here. The job is translating what Phases 1–8 settled
into a form developers and future agents can read. Documents are written in
the user's language (`meta.language`); heading anchors and identifiers stay
English.

---

## What to generate

### 1. CLAUDE.md (single, at root) ★
The one governing document for the whole project (frontend / backend /
shared). Never split, even in a monorepo. From now on this CLAUDE.md is the
progression rule agents follow in post-migration development. Include:
- **Project purpose and philosophy** (translate Phase 2's Why and Phase 6's
  development philosophy).
- **Architecture** (3-layer structure, contract-mediated decoupling, diagram
  essentials — Phase 3).
- **Tech stack and rationale** (Phase 5 — especially "what was left out" and
  why, so future developers do not add unnecessary dependencies).
- **Development workflow and discipline** (Phase 6, at scope-appropriate
  weight).
- **Inviolable design principles**: the contract canon lives in one place;
  the UI never holds the contract directly; layers never mix; **naming
  follows the term ledger and naming grammar (`documents/ubiquitous.md`),
  and new concepts get registered first**; etc. Verbalize the rules that
  prevent coupling and vocabulary drift.
- **Startup and development procedure** (dev startup; with `backend: mock`,
  how to start both).
- For Type B, if migration steps remain, state them as the **handoff plan**
  (h2p's exit = the agreed scope completed; the rest proceeds under this
  CLAUDE.md).

### 2. README.md (root)
The human entrance: project overview, setup, startup, directory layout.
Where CLAUDE.md is for agents/discipline, README is the project's public
face. **Generate it fresh** for the migrated project (the h2p tool guide
lives in the plugin, never in the project, so there is nothing to preserve or
strip here).

### 3. documents/ (root)
Keep design decisions traceable. Write up the `.h2p/` agreements
(requirements, structure diagrams, stack rationale, workflow) as design
documents maintainers can consult later. Preserving Phase 3's Mermaid
diagrams here pays off in later development.
**Write `.h2p/ubiquitous.md` up as `documents/ubiquitous.md`** (including
Part 2, the naming grammar). It remains the canon of meaning and naming in
post-migration development (CLAUDE.md points at it).

---

## Procedure

### 1. Write up
Read each `.h2p/` artifact and generate items 1–3 above. Polish working-memo
prose into readable deliverables. Match the coverage and depth to scope
(backend presence, maintenance horizon).

### 2. Final consistency check
- Does CLAUDE.md's description match the actually generated
  frontend/backend/shared structure?
- Does the startup procedure actually work (dev starts as written)?

### 3. Retiring the tool scaffolding
The conversion is a one-time event; do not let the tool's scaffolding squat
in a live project. The `prompts/` set was materialized into this project by
the h2p plugin at the start; retire it now. With the user's consent:
- Fold `prompts/` and `.h2p/` into `.h2p-archive/`.
- **Keep `.h2p-archive/` tracked by git** (do not gitignore it) so the
  migration's decision record stays in history. Ignore only generated
  things like `node_modules`.
- **Record this retirement as the final commit**
  (e.g. `h2p: migration completed, scaffolding archived`).
- **Final disposal of `.h2p-archive/` is the user's call.** It is the record
  of the conversion's decisions, so always ask whether to discard or keep
  (never delete on your own). If they choose disposal, note it remains in
  git history.
- After retirement, the root holds only the clean project (CLAUDE.md /
  README.md / documents/ / frontend/ etc.).

### 4. Report completion
Tell the user the migration is complete, development can start, and the
record remains in `.h2p-archive/`.

## Artifacts
- Single root `CLAUDE.md`, `README.md`, `documents/` (incl. ubiquitous.md).
- Tool scaffolding retired with the final commit (within user consent).
- `state.md`: Phase 9 done; all Phases complete.

## Do not
- No new design decisions or feature additions. Write up, period.
- Do not split CLAUDE.md (single, at root).
- Do not discard `.h2p-archive/` without the user's confirmation.
- Do not gitignore the migration record (`.h2p*`, `prompts/`) — history
  keeps it.
- Do not break the project root while retiring.

## Completion conditions (to mark done)
- CLAUDE.md (single, root), README.md, and documents/ (incl. ubiquitous.md)
  are generated.
- Descriptions match the project reality; the startup procedure actually
  works.
- The tool scaffolding is retired and tidied (with user consent), final
  commit recorded.
- The user agreed the project is complete.

With this, the whole html-to-project flow completes. The working HTML
prototype has migrated, design intent intact, into a project where
development can start immediately. The generated CLAUDE.md guides
development from here.
