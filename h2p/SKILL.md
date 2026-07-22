---
name: h2p
description: >
  html-to-project (h2p): migrate a WORKING HTML prototype (single page or
  multiple pages forming one app, including CDN-loaded frameworks), or
  restructure an existing frontend app, into a project ready for continued
  development — without changing any features. A user-driven, multi-session,
  9-phase process. Invoke explicitly when the user says "/h2p", "html-to-
  project", "h2p", or clearly asks to "migrate/convert this HTML prototype
  into a project", "scale this HTML up into a frontend/fullstack project", or
  "restructure this frontend app for growth without changing behavior". Do
  NOT use for fixing broken pages, adding features, debugging, or generic
  HTML/CSS help — h2p never changes features.
---

# h2p — launcher

This skill is a thin launcher. The h2p prompt set is read **in place** from
this skill's own directory; nothing is copied into the user's project. All
migration logic (git init, input detection, state, phases) lives in the
orchestrator — do not reimplement any of it here.

## On invocation

1. This skill's base directory is reported to you at invocation; call it
   `<H2P>`. The prompt set lives at `<H2P>/prompts/`. **Read it in place —
   never copy it into the user's project.**

2. **Read `<H2P>/prompts/orchestrator.md` and follow it.** It reads the
   remaining prompts from `<H2P>/prompts/` as needed and keeps all working
   state under `./.h2p/` in the user's project. Re-invoking `/h2p` resumes
   from `.h2p/state.md`.

The user's project only ever accumulates `./.h2p/` (state, artifacts, working
files) and the actual migration output (`origin/`, `frontend/`, etc.). That is
all this skill does; everything else lives in the prompts.
