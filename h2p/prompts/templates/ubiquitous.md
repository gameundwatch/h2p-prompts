# ubiquitous.md — canonical language of the project (template)

`.h2p/ubiquitous.md` is the project's **canonical source of meaning and
naming**. If the contract is the canon of "data shapes", this ledger is the
canon of "names". Diagram element names in Phase 3, code identifiers in
Phase 7/8, and document terminology in Phase 9 all follow this file.

Two parts. **Part 1 (the ledger) is created per project in Phase 2. Part 2
(the naming grammar) is fixed tool content — include it verbatim, never
generate it.** Headings are fixed English anchors; **definitions and prose are
written in English** (this is an internal `.h2p/` artifact), but the ledger's
"Term (user language)" column keeps the domain surface term **verbatim** in
the user's language (e.g. `予約`) — that column is canonical data, not prose,
and is the whole point of the ledger.

---

## Part 1: Term Ledger (created in Phase 2)

| Term (user language) | Definition | Code identifier (English) | Origin |
|----------------------|------------|---------------------------|--------|
| 予約 | The act (and record) of a user securing a seat | Reservation | Phase1(c) observation; contract POST /reservations |

Requirements dialogue happens in the user's language; code and diagram
identifiers are English. **This table fixes the correspondence uniquely** —
it prevents the accident where the same concept becomes `Booking` in a
diagram and `Reservation` in code.

### Operating rules
- **Append-only.** When a downstream Phase needs a new concept, confirm with
  the user and register it before using it (also add one line to `state.md`
  decisions).
- **Renaming an existing term is treated as a rollback to Phase 2.** Silent
  paraphrasing is forbidden.
- Type B: if an existing code identifier is sound, respect it and register
  it as-is (renaming is migration cost).

---

## Part 2: Naming Grammar (fixed — do not edit)

The ledger provides the vocabulary; this grammar provides the construction.
**Dictionary-style naming** — names whose meaning is clear on sight — makes
explanatory comments unnecessary. This tool assumes 0→1 construction, so
there is no requirement to match a pre-existing convention; follow this fixed
dictionary. For Type B, the grammar applies **only to new and modified
code**; never mass-rename existing code.

### Verb dictionary (function names start with a verb used in this sense)
| Verb | Meaning |
|------|---------|
| get | synchronous, side-effect-free retrieval (returns what is at hand) |
| fetch | retrieval over the network |
| load | read from storage / file |
| save | write to persistence |
| create | bring a new entity into existence (an ID is born) |
| build | assemble from parts (pure, no side effects) |
| parse | string/external format → internal structure |
| format | internal structure → display string |
| to<X> | type/format conversion (pure; e.g. toDate, toJson) |
| validate | check and report violations |
| render | reflect onto the screen |
| handle | respond to an event (e.g. handleSubmit) |
| update | modify an existing entity |
| remove | take out of a collection (the entity remains) |
| delete | destroy the entity itself |
| find | look for one match (may not exist) |
| filter | return all matches |
| init | prepare for use / initialize |

### Modifier rules
- **Booleans** start with `is` / `has` / `can` / `should`
  (`isOpen`, `hasError`).
- **Numbers carry their unit in the name** (`timeoutMs`, `widthPx`,
  `delaySeconds`, `maxRetryCount`). Unit-less numeric names are forbidden.
- **Collections** are plural (`users`); keyed lookups are `xxxById`
  (`usersById`).
- **Never break symmetric pairs**: open/close, add/remove, start/stop,
  show/hide, begin/end. Do not swap one side for a different word.
- **Name length scales with scope**: `i` inside a loop is fine; abbreviations
  in module-level / exported names are not (except conventional ones like
  URL, ID).

### Project-specific idioms (the only section extended in Phase 5/6)
Add only conventions that stem from the chosen tech stack.
- <e.g. event handlers are handleXxx; React props are onXxx>
- <e.g. component files are PascalCase.tsx; everything else kebab-case.ts>
