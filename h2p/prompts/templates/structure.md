# structure.md — structure artifact (template)

Skeleton for `.h2p/phase3-structure.md`. The writer differs by input type
(A: phase3a / B: phase3b), but downstream Phases (4–7) read it without caring
about the type. **The heading structure and the per-diagram Mermaid notation
are immutable**; only the diagram content varies per project. All element
names use identifiers from `.h2p/ubiquitous.md` (register new concepts
first). Headings are fixed English anchors; prose is in the user's language
(`meta.language`).

---

```markdown
# Structure (phase3-structure)

## Diagram 1: Application structure
Notation: Mermaid flowchart (graph TD/LR). Show layers (UI / logic / data
access; with backend: mock, also the backend 3 layers and shared) as
subgraphs, with dependency direction as arrows. The diagram must make
readable: "UI does not know the contract directly" and "only the data-access
layer depends on the contract".
<Type B: target structure; list the deltas from the current state below the diagram>

```mermaid
<diagram>
```

## Diagram 2: Data types
Notation: Mermaid classDiagram (or erDiagram). The blueprint of the canonical
contract. **Only shapes of data that are exchanged or persisted belong
here.** Never mix in UI-held transient state (that is Diagram 3).

```mermaid
<diagram>
```

## Diagram 3: State structure
Notation: Mermaid flowchart. Classify frontend-held state into local /
shared / server-derived, showing which component/layer owns each. **Only
state the screen needs to run belongs here.** Never mix in contract types
(that is Diagram 2). This separation is the origin of coupling prevention.

```mermaid
<diagram>
```

## Diagram 4: Flow chart
Notation: Mermaid flowchart. Primary user-operation flows (matching How in
the 5W1H). Mark the points where communication happens (exchanges through
the contract) with node annotations.

```mermaid
<diagram>
```

## Migration plan
<Type B only. Type A: N/A>
One step = one block, ordered by dependency (foundation first). One commit
per completed step (with the h2p: prefix).

### Step <N>: <name>
- what: <the change>
- why: <which gap it closes>
- verify: <how to confirm the app still works after this step>

### Scope line
- executed in this run: <Steps 1–N>
- delegated to the generated CLAUDE.md: <Steps N+1–>
```
