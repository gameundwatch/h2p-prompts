# analysis.md — analysis artifact (template)

Skeleton for `.h2p/phase1-analysis.md`. The writer differs by input type
(A: phase1a observation / B: phase1b inventory & diagnosis), but downstream
Phases (2–8) read it without caring about the type. **The heading structure
is shared and immutable**; type differences are absorbed inside sections
(unused sections stay present, marked "N/A"). Headings are fixed English
anchors; body content is written in the user's language (`meta.language`).

---

```markdown
# Analysis (phase1-analysis)

## Target (analysis_target)
<target files; for multiple HTML files, also the confirmation that they form one app>

## Observation / Inventory
<Type A: (a) DOM structure & UI elements / (b) behavior & events /
 (c) values that change as state / (d) navigation & view switching /
 (e) styling / (f) external dependencies & backend-demand traces
 (record CDN-loaded frameworks concretely: name, version, how they are used)>
<Type B: dependencies & scripts / directory & module layout / build & tooling
 config / size>

## Diagnosis
<Type B only: detected degradation/lack of non-functional requirements
 (layer mixing, tight coupling, state management, dependency health,
 tests/types, duplication) — as facts and risks, with origin locations>
<Type A: N/A (phase1a observes only)>

## Backend-demand traces
<Type A: summary of (f) — what, where, and what it appears to demand>
<Type B: relationship with existing backend / external APIs; contract presence>

## Deferred judgments
<explicit list of things left unknown or ambiguous>

## Behavior Checklist
The sole reference for behavior verification gates (P4/P7/P8). Numbered,
"action → expected result" format. Split into sections per page/screen;
include cross-page navigation as items.

### <page/screen name>
1. [ ] <action> → <expected result>
2. [ ] <action> → <expected result>

### Cross-page navigation
1. [ ] <action> → <destination and state>
```
