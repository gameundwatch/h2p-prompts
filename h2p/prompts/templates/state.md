# state.md — canonical progress record (template)

`.h2p/state.md` is the **sole source of truth** for html-to-project. The
current position, scope, per-Phase completion status, and important decisions
exist permanently nowhere else. The orchestrator always reads it at boot;
each Phase updates it on every agreement. Even if conversational context is
lost (compaction, agent switch, dropped session), work can be restored from
this file alone.

Below is the initial template written at first boot. The agent updates values
while keeping this structure intact. Do not change section order or headings
(they are re-read mechanically). **Headings and keys are fixed English
anchors; body text is written in English (this is an internal `.h2p/`
artifact), with domain terms and identifiers recorded verbatim.** The
`meta.language` value itself records the user's language for dialogue and
user-facing deliverables — it does not make this file the user's language.

---

```markdown
# html-to-project state

## meta
- created: <ISO8601>
- last_updated: <ISO8601>
- current_phase: 1
- language: <user's language, e.g. ja>   # dialogue + user-facing deliverables; .h2p/ artifacts are English
- input_type: undecided   # undecided | A | B (detected and recorded by the orchestrator at boot)
- backend: undecided   # undecided | none | mock (settled in Phase 2; "full" is reserved, out of scope)
                       # The frontend is always built; the only branch is backend presence.

## source
Input evacuation / working-copy status and analysis-target confirmation.
Content differs by input_type.
- **Type A**: list the evacuated input (under .h2p/source/).
  - copied_files:
    - <e.g. source/index.html>
- **Type B**: no evacuation. Record the working-copy status.
  - working_copy: <e.g. copied to root (node_modules/.git excluded),
    dependencies installed, startup verified. .origin/ frozen>
- analysis_target: undecided   # target files and the confirmation that they form one app
- notes: <e.g. multi-page or single, notes at confirmation>

## phases
Status per Phase: pending | in_progress | done | skipped.
"artifact" is the relative path. Conditions for "done" are defined by each
Phase prompt.

| # | name | status | artifact |
|---|------|--------|----------|
| 1 | Analysis (A: observe / B: inventory & diagnose) | in_progress | .h2p/phase1-analysis.md (incl. behavior checklist) |
| 2 | Requirements & contract elevation | pending | .h2p/phase2-requirements.md, .h2p/ubiquitous.md |
| 3 | Structure (A: give / B: redesign & migration plan) | pending | .h2p/phase3-structure.md |
| 4 | Tech stack selection | pending | .h2p/phase4-stack.md |
| 5 | Development workflow design | pending | .h2p/phase5-workflow.md |
| 6 | Frontend implementation / migration | pending | frontend/ |
| 7 | Backend implementation | pending | backend/, shared/ |
| 8 | Documentation | pending | CLAUDE.md, README.md, documents/, ISSUES.md (if backlog non-empty) |

## gates
Behavior-verification gate records, consulted by the orchestrator on
progression. When marking passed, attach the gate commit hash
(e.g. `passed (commit a1b2c3d)`).
- p6_frontend_behaves: not_checked   # not_checked | passed (commit <hash>) | failed
- p7_integration_runs: not_checked   # n/a when backend: none

## approved_deviations
Differences from the original found at verification gates that the **user
explicitly accepted** (append-only; never delete lines). Each entry: Phase,
the difference, and the reason for acceptance, in 1–3 lines. Silent
acceptance violates the First Principle.
- <empty if none>

## decisions
Append-only log of important decisions (chronological, append at the end).
Also serves as the basis for rollback judgments. Each entry: timestamp,
Phase, decision, rationale, in 1–3 lines.
- <ISO8601> P1: <e.g. target fixed to index.html; other.html judged to be an asset store>

## backlog
Deferred work: items out of the current Phase's scope, plus problems and open
questions the First Principle forces us to leave untouched during migration
(behavior must not change) — to be tackled **after handoff, under CLAUDE.md
governance**. This is the internal running log; record items here as they
arise in any Phase. Phase 8 writes it up into the root deliverable `ISSUES.md`
(from `prompts/templates/issues.md`), which survives migration and CLAUDE.md
points at. Migration never resolves these — it only lays the seams (see
`references/foundations.md`) that make them cheap later. Append-only.
- <empty if none>
```

---

## Operating rules

### Consistency between current_phase and the phases table
The value of `current_phase` and the single `in_progress` row in the phases
table must always match. On Phase completion the orchestrator updates three
things at once: (a) that row → `done`, (b) next row → `in_progress`,
(c) `current_phase` advanced.

### Git discipline
h2p is an environment-construction workflow that includes git.
- At first boot, if the working folder is not a git repository, the
  orchestrator runs `git init`.
- **Always commit when a behavior verification gate (P6/P7) passes**
  (message with the `h2p: ` prefix, e.g. `h2p: P6 gate passed`) and record
  the hash in gates.
- **Type B migration steps: one step = one commit.**
- `.h2p/`, `.origin/` (frozen input baseline), and `.archive/` (retired
  working dir, created at Phase 8) are **tracked by git** (never gitignored)
  so the migration's decision record stays in history — there is no `prompts/`
  in the project (the prompt set is read in place from the h2p skill).
  `.archive/` is safe for the user to delete after completion; the choice is
  theirs. Gitignore only the disposable / generated: `.review/` (a one-way
  projection of `.h2p/` with no canonical information — add it to `.gitignore`
  at first boot) and `node_modules`.

### How backend takes effect
The frontend is always built; the only branch is backend presence.
- `backend: none`: mark the Phase 7 row `skipped` and
  `gates.p7_integration_runs` as `n/a`. Do not create `shared/` or
  `backend/`.
- `backend: mock`: run Phase 7 normally, creating `shared/` (canonical
  contract) and `backend/` (mock server).
- `backend` stays `undecided` until Phase 2 settles it. On settling, record
  the rationale in decisions (especially how external-API traces were judged
  regarding CORS / key secrecy).

### Updating gates
Verify at the exit of gated Phases (4, 7, 8) and record the result in gates.
Verification uses the Phase 1 behavior checklist as the reference; final
confirmation of visuals and interaction is performed by the user. Record any
user-accepted differences in `approved_deviations` before marking passed.
On a progression request, the orchestrator confirms the relevant gate is
`passed` before advancing; while `failed`, progression is held.

### Rollback handling
When the user instructs a rollback to an upstream Phase:
1. Set the target Phase's status to `in_progress` and rewind `current_phase`.
2. Rewind all Phases **downstream** of the target to `pending` and the
   affected gates to `not_checked`.
3. Append to decisions: when, to where, and why the rollback happened.
4. Tell the user that downstream artifacts are invalidated (files are not
   deleted; re-execution overwrites them).

### Writing principles
- Always refresh last_updated on every write.
- decisions, backlog, and approved_deviations are **append-only** (never
  delete lines); they are future judgment material.
- Keep this file in a state where a human can fully grasp the current
  position by reading it.
