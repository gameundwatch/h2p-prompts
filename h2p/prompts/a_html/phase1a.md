# phase1a — HTML analysis [Type A: HTML prototype]

You are now in Phase 1. The orchestrator's First Principle and progression
rules are assumed loaded; only Phase 1's work procedure is given here.

Reference: `prompts/references/foundations.md` (retrofit-cost foundation
catalog) — while observing, note which readiness elements the input already
has vs. lacks, as **facts** (do not fix here).

## Responsibility of this Phase

**Observe the input HTML** evacuated to `.h2p/source/` (a single page, or a
set of pages forming one app) **and extract facts**. That is all.

The sole product of this Phase is "observed facts" — not judgment, not
evaluation, not improvement proposals. Every later Phase uses this
observation as neutral ground. Opinions mixed into the ground distort
everything above it, so observe and nothing else.

### Do not
- **No evaluation or improvement proposals** ("this design is bad",
  "this should be fixed") — fit-to-intent correction is Phase 6's job,
  technology judgment Phase 4's.
- **No technology references** ("this should be built in React").
- Do not fill gaps with speculation. Write only confirmed facts.
- Leave unknowns and ambiguities honestly as "unknown" / "judgment deferred".

### The only judgment in this Phase
Confirming the **analysis target** when multiple HTML files exist. This is
premise-fixing, not observation, so confirm with the user and record it in
`state.md`'s `analysis_target`. Never pick one silently.

---

## Procedure

### 1. Confirm the analysis target
Check `state.md`'s `source.copied_files`.
- A single HTML file: record it as `analysis_target`.
- Multiple files: observe each file's role (main page / sub page / asset),
  present them to the user, and confirm "do they form one app / which is the
  target?" before finalizing.

### 2. Observe (cover every aspect below, without omission)
Aspects are separated for readability; all of them are enumerations of fact.

**(a) DOM structure and UI elements**
- The major regions/blocks composing the screen.
- **Repeated structures** (same-shaped cards/rows/list items duplicated by
  hand, etc.). These are evidence for later componentization — record their
  count and locations concretely.

**(b) Behavior and events**
- Where event handlers live (onclick attributes, addEventListener) and what
  they cause (as far as observable).
- DOM manipulation (show/hide, add/remove, text rewriting).

**(c) Values that change as state**
- Values that change through user action or time. Where they are held, read,
  and written. Include state buried in global variables, data attributes, or
  the DOM itself.

**(d) Navigation and view switching**
- Tabs, modals, page-like switching, history manipulation — anything where
  "the screen changes". (Router necessity is Phase 4's evidence; here only
  the fact of presence/absence.)

**(e) Styling**
- How styles are held (inline / `<style>` / external CSS).
- Whether design-token-like values (colors, spacing, fonts) are
  **variablized or hard-coded**. (Styling strategy is Phase 4's evidence;
  record facts only.)

**(f) External dependencies and backend-demand traces ★most important**
HTML leaves traces of "the parts the frontend alone cannot complete". Detect
these as facts — they feed Phase 2's scope decision.
- `fetch`/XHR calls, or mocked / commented-out communication.
- **Hard-coded dummy data** (arrays that should come from a server).
- localStorage/sessionStorage used as a **stand-in for persistence**.
- **Forms with no (or dummy) submission target**.
- **UI-only login/auth** (auth screens with no substance).
- External APIs, CDNs, third-party scripts. In particular, **frameworks or
  libraries loaded via CDN (React/Vue/Tailwind, etc.)** are first-class
  evidence for Phase 4's technology selection — always record them
  concretely (what, which version, how used).

For each trace you may observe "what it appears to demand", but never judge
"a backend should be built" (that is Phase 2).

### 3. Build the behavior checklist ★the reference for verification gates
From observations (b)(c)(d), build a **numbered behavior checklist**. Each
item is "action → expected result", at a granularity that can be checked
mechanically.
- For multi-page inputs, split into **sections per page** and include
  cross-page navigation as items.
- This list becomes the sole reference by which the behavior verification
  gates (P6/P7) decide "same behavior". Behavior not listed here will not
  be verified — collect exhaustively.

### 4. Write the artifact
Write the observations to `.h2p/phase1-analysis.md` following the structure
of `prompts/templates/analysis.md` (do not change the heading structure;
mark sections unused by Type A as "N/A"). Each item is an enumeration of
fact with location, content, and the relevant source text. Leave
ambiguities explicitly as deferred judgments. The behavior checklist format
also follows the template.

Update `state.md`: fix `analysis_target`; summarize key observations
(especially backend-demand traces) in decisions in 1–2 lines.

### 5. Present to the user
Convey the essentials briefly. Always touch on the presence/absence of
"(f) backend-demand traces", since it feeds Phase 2's scope decision — but
even here do not say "so a backend is needed". Stay at: "these traces were
observed; how to treat them is decided in the next Phase."

---

## Completion conditions (to mark done)

- `analysis_target` is confirmed.
- Observations (a)–(f) are written in `.h2p/phase1-analysis.md`.
- A **numbered behavior checklist** is written at the end of the artifact
  (per page, including cross-page navigation, for multi-page inputs).
- Presence/absence of backend-demand traces is explicit.
- The user has reviewed the observations.

When the user instructs progression, go through the orchestrator's
consistency check to Phase 2.
