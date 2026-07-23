# stack.md — tech stack artifact (template)

Skeleton for `.h2p/phase4-stack.md`. Phase 6/7 assemble scaffold commands
mechanically from this file; Phase 8 uses it as material for `documents/`.
Do not change section order or headings. Headings are fixed English anchors;
prose is written in **English** (this is an internal `.h2p/` artifact), with
domain terms and identifiers recorded verbatim.

**Recording rejections matters as much as recording adoptions.** It is what
stops future developers (and agents) from re-adding unnecessary
dependencies — the heart of this tool's over-engineering ban.

---

```markdown
# Tech stack (phase4-stack)

## Per-category decisions
Repeat the block below for every category judged (do not omit rejected
categories).

### <category name (e.g. state management)>
- necessity: <against which evidence of this project is this capability
  needed, and where>
- verdict: adopted | rejected
- technology: <name, version policy> ("none" if rejected)
- role in the project: <which layer, responsible for what; 1–2 lines>
- candidates not chosen & why: <candidate> — <reason> / for a rejected
  category, the evidence that the whole category is unnecessary

## Styling strategy
- chosen approach: <plain CSS / CSS Modules / Tailwind / CSS-in-JS>
- rationale: <evidence from the source HTML / working code + Phase 3 diagrams, and the user's choice>

## Backend (only when backend: mock)
- language/framework: <user's choice and reason>
- contract method: <Zod / TypeSpec / OpenAPI, etc. The canon lives in
  shared/, one place only>

## Foundation
- package manager: <npm / pnpm / yarn>
- version control & testing approach: <matched to scope (maintenance horizon)>

## Scaffold commands
Command sequence Phase 6/7 will execute via bash as-is.
- <e.g. npm create vite@latest frontend -- --template react-ts>
- <e.g. cd frontend && npm install>
```
