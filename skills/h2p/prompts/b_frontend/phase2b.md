# phase2b — current intent, future intent, gap definition [Type B: existing app]

You are now in Phase 2 (Type B). The orchestrator's First Principle and
progression rules are assumed loaded. Input: `.h2p/phase1-analysis.md`
(inventory & diagnosis) and the `origin/` originals.

## Responsibility of this Phase

Type A retro-verbalized "what was this trying to be?". Type B is different:
an existing app has **design intent explicitly embedded in code**. So here:

1. **Read the current design intent** — verbalize, from the code, what is
   built with what intent (from evidence, not speculation).
2. **Draw out the future intent** — settle through dialogue where the user
   wants to scale / what features they want to add. **This is Type B's
   future-oriented step.**
3. **Define the gaps** — make explicit the delta between the current
   structure and the structure needed to support the future intent. These
   gaps become the targets of the later refactoring plan.

Never forget the First Principle: **future intent asks about "features they
want to add", but this tool does not implement those features.** It only
prepares a structure that can withstand their addition. Future intent is
material for deriving "what structural flexibility is needed", not an
implementation order.

---

## Procedure

### 1. Read the current design intent
From Phase 1's inventory/diagnosis and origin, verbalize the current intent.
- What the app does (the feature landscape readable from code).
- Its structural policy (deliberate design, or drift?).
- 5W1H-style organization is fine, but unlike Type A this is **code reading,
  not retro-inference**. Confirm with the user: "is this understanding
  correct?"

### 2. Draw out the future intent ★Type B specific
Ask the user about the outlook.
- What features and scale to grow into: near-term additions, long-term
  direction.
- Changes in users/team (solo → team, user growth, etc.).
- Translate these together into **non-functional demands**. Examples: "add
  more fields" → data-structure extensibility; "add screens" → routing and
  state separation; "team development" → tests, types, clear boundaries.

**The goal is not a feature list.** The goal is extracting "where in the
structure flexibility is needed so those features can be added".

### 3. Define the gaps
Define the delta between the current state (Phase 1 diagnosis) and the
structure that supports the future intent.
- Which non-functional requirements fall short of the future intent.
- Enumerate the gaps to close, prioritized (by impact on the future
  intent). Gaps that do not serve it (idealization irrelevant to the future
  intent) are **not closed** — First Principle 2 (only what serves).

### 4. Establish the ubiquitous-language ledger ★required artifact
Read `prompts/templates/ubiquitous.md` and generate `.h2p/ubiquitous.md`
following its structure. Collect Part 1 (ledger) terms from two sources:
- **Excavated from existing code**: vocabulary embedded in current type/
  variable/module names. If an existing identifier is sound, respect it and
  register as-is (renaming is migration cost).
- **New concepts from the future intent**: concepts the structure must
  support going forward.

Include Part 2 (naming grammar) verbatim from the template. For Type B the
grammar applies **only to new and modified code**; never mass-rename
existing code. Operating rules (append-only; register before use;
renaming = rollback to Phase 2) follow the template.

### 5. Scope decision
As with Type A, settle `backend` (`none` | `mock`). Organize, based on the
diagnosis, how an already-present/consumed backend is handled and whether a
contract exists. Also settle this project's **migration scope**: draw the
line between what h2p executes now and what is delegated to the generated
CLAUDE.md. **This line defines h2p's exit** (exit = completion of the agreed
migration scope; see the orchestrator's jurisdiction boundary). The range is
determined by project size and the user's wishes — the tool never hardcodes
an amount.

---

## Writing the artifacts
Write `.h2p/phase2-requirements.md` (same filename as Type A) following the
structure of `prompts/templates/requirements.md` (do not change the heading
structure). Content: current design intent and future intent (5W1H), gap
definition, scope (`backend`, migration-scope line, maintenance horizon),
logical contracts (when `backend: mock` or external-API contracts exist).

Write `.h2p/ubiquitous.md` (per the template).

`state.md`: `backend` settled, migration scope, decisions summarizing the
key choices (especially future intent and gap priorities).

## Do not
- Do not implement or design features under the pretext of future intent.
  Extract **structural demands** only.
- Do not include idealization that does not serve the future intent in the
  gaps (a source of over-engineering).
- Do not assert current intent by speculation. Ground it in code and confirm
  with the user.
- Do not widen scope on your own. Never exceed the agreed migration scope.

## Completion conditions (to mark done)
- The current design intent is read, verbalized, and confirmed by the user.
- The future intent is drawn out and translated into non-functional demands.
- Gaps are defined with priorities (non-serving ones excluded).
- Scope (`backend`, migration scope, horizon) is settled.
- `.h2p/ubiquitous.md` is established.
- The user has agreed to requirements, gaps, scope, and the ledger.

When the user instructs progression, go through the orchestrator's
consistency check to Phase 3 (phase3b). Phase 3 designs the target structure
and the stepwise migration plan that closes these gaps.
