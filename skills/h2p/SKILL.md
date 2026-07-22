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

# h2p — bootstrap

This skill is a thin launcher. It materializes the h2p prompt set into the
current project once, then hands off to the orchestrator, which owns all
migration logic (git init, input detection, state, phases). Do not reimplement
any of that here.

## On invocation

1. **If `prompts/` does not already exist in the current working directory**,
   copy this skill's bundled `prompts/` directory into it:

   ```bash
   [ -d prompts ] || cp -R "<this skill's base directory>/prompts" ./prompts
   ```

   Use the base directory reported for this skill at invocation as the copy
   source. If `prompts/` already exists (an in-progress or prior migration),
   **do not copy or overwrite it** — the in-project prompts are frozen for
   this migration and win.

2. **Read `prompts/orchestrator.md` and follow it.** The orchestrator detects
   whether this is a first boot (no `.h2p/state.md`) or a resume (state
   present) and proceeds accordingly. Re-invoking `/h2p` mid-migration simply
   resumes.

That is all this skill does. Everything else lives in the prompts.
