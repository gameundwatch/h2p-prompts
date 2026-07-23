# phase1b — inventory & diagnosis [Type B: existing frontend app]

You are now in Phase 1 (Type B). The orchestrator's First Principle and
progression rules are assumed loaded. Input: the original app in `origin/`
(package.json + src + dependencies). **Never touch the originals** — read
and diagnose.

Reference: `prompts/references/foundations.md` (retrofit-cost foundation
catalog) — include the presence/absence of these readiness elements in the
diagnosis.

## Responsibility of this Phase

**Inventory** the existing app (grasp the facts) and **diagnose** it (detect
degraded or missing non-functional requirements). Unlike Type A's pure
observation, this Phase **includes evaluation** — but the object of
evaluation is **non-functional requirements (structural health) only**;
never evaluate the quality of features.

Per the First Principle, this tool does not change features. The diagnosis
exists to see "is the structure ready to grow features?", not to condemn the
features themselves.

---

## Inventory (grasping facts)

Read `origin/` and establish as fact:

- **Dependencies and scripts** (package.json): frameworks, libraries,
  versions; build/test/start scripts; package manager.
- **Directory/module layout** (src etc.): the units files are split into,
  entry points, routing, state-management mechanism.
- **Build/tooling config**: bundler, TS config, lint/format, existing tests.
- **Size**: approximate file count, component count, dependency count
  (material for estimating migration-plan weight).

## Diagnosis (detecting non-functional state) ★

Detect the current structural condition from the viewpoint of "can features
keep being added?". **Ground everything in fact; never presume.** Detection
stays at "here is what this looks like" plus "here is how it could affect
extensibility" — whether to fix it is planned in Phase 3.

Aspects to inspect (only those that apply):
- **Layer mixing**: are presentation, logic, and communication mixed in the
  same place (viewed against the frontend 3 layers: UI / logic / data
  access)?
- **Tight coupling**: does changing one module ripple widely? Do components
  depend directly on API shapes (absence of a contract)?
- **State management**: is state scattered or duplicated? Are types and
  state mixed?
- **Dependency health**: excessive dependencies; obsolete/deprecated/
  abandoned ones; duplicated-purpose libraries (multiple picks for the same
  capability).
- **Tests and typing**: presence and coverage of tests; typing condition.
- **Duplication and tribal knowledge**: copy-paste duplication, implicit
  rules, missing documentation.

Tie every detection to "where in origin, what, and how it looks". Feature
gaps and UI/UX quality are **outside the diagnosis** (outside this tool's
range).

---

## Procedure
1. Read `origin/` and take the inventory (do not modify the originals; all
   subsequent changes happen on the root working copy, and `origin/` remains
   the frozen baseline).
2. Detect the non-functional condition along the diagnosis aspects.
3. **Build the behavior checklist**: run the app and list its main usage
   flows and behaviors as a numbered "action → expected result" checklist.
   Split into sections per screen; include cross-screen navigation. This
   becomes the sole reference by which the behavior verification gates
   (P6/P7) decide "does it still work the same?".
4. Present inventory + diagnosis to the user. Show "possible obstacles to
   growth" as facts and risks (no verdicts; judgment belongs to later
   Phases). If the codebase is large, confirm with the user which
   modules/areas are in scope (`analysis_target`).
5. Write `.h2p/phase1-analysis.md`.

## Writing the artifact
Write `.h2p/phase1-analysis.md` (same filename as Type A, so later Phases
read it type-agnostically) following the structure of
`prompts/templates/analysis.md` (do not change the heading structure).
Content: inventory (dependencies, layout, config, size); diagnosis (detected
non-functional conditions and extensibility risks, with origin locations);
scope (`analysis_target`); deferred judgments; behavior checklist (format
per the template).

`state.md`: fix `analysis_target`; summarize the diagnosis essentials
(especially obstacles to growth) in decisions.

## Do not
- Do not evaluate feature quality or UI/UX (out of range). Diagnosis is
  limited to non-functional requirements.
- Do not decree fixes ("this must be corrected") — remediation is planned in
  Phase 3. Stay at detection and risk presentation.
- Do not **modify** the `origin/` originals (this Phase is read-only).
- Do not fill gaps with speculation. Write only confirmed facts and explicit
  deferrals.

## Completion conditions (to mark done)
- Inventory and diagnosis are written in `.h2p/phase1-analysis.md`.
- Non-functional conditions that could obstruct growth are shown as facts
  and risks.
- A **numbered behavior checklist** is written at the end of the artifact.
- The scope (`analysis_target`) is confirmed.
- The user has reviewed the diagnosis.

When the user instructs progression, go through the orchestrator's
consistency check to Phase 2 (phase2b). Phase 2 reads the current design
intent and draws out the future intent — where the user wants to scale.
