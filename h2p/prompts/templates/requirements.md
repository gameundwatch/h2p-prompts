# requirements.md — requirements artifact (template)

Skeleton for `.h2p/phase2-requirements.md`. The writer differs by input type
(A: phase2a / B: phase2b), but downstream Phases read it without caring about
the type. **The heading structure is shared and immutable.** The Contracts
section is the target of partial updates via "contract amendment" in
Phase 6/7, so its one-contract-per-block format must be kept. Headings are
fixed English anchors; body prose is written in **English** (this is an
internal `.h2p/` artifact), with domain terms and identifiers recorded
verbatim.

---

```markdown
# Requirements (phase2-requirements)

## 5W1H
<Type A: intent retrieved from the HTML and settled through dialogue>
<Type B: current intent read from the code + future intent drawn out in dialogue>
- Why: <reason to exist>
- Who: <intended users / roles>
- What: <core features / the MVP core>
- When/Where: <usage context>
- How: <primary usage flows (map to the behavior checklist)>

## Scope
- backend: <none | mock> — rationale: <how traces were read (incl. CORS /
  key-secrecy judgment)>
- maintenance horizon: <throwaway/short-term | long-term growth>
- migration scope: <Type B: the line between what h2p executes now and what
  is delegated to the generated CLAUDE.md. This line defines h2p's exit.
  Type A: N/A (all Phases run)>

## Gap definition
<Type B: the deltas to close, prioritized; anything that does not serve the
 future intent is marked "excluded">
<Type A: N/A (structure is given fresh)>

## Contracts
Only when backend: mock (or a public API is used). Language-agnostic logical
contracts. One contract = one block. Contract amendments update block by
block and are recorded in decisions.

### <contract name (e.g. list reservations)>
- request: <method, path, parameters (type, required/optional, meaning)>
- response: <structure (type, required/optional, meaning)>
- notes: <what was changed from the frontend's raw demand to make the
  resource representation straightforward>

## Out of scope (backlog)
<deferred items that would not settle, ranges deliberately cut>
```
