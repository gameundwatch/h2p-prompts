# Foundation catalog — what to provision up front (high retrofit cost)

Reference consulted by Phases 1, 3, 4, 5. This is the canonical list of
structural / non-functional **foundations that are expensive to retrofit**
(large blast radius) and are therefore laid up front as *readiness* — per the
First Principle's anti-YAGNI / MVP stance.

**Readiness, not features.** Every entry provisions the *ground* for future
growth; it never adds the *feature* itself (First Principle 1). Where the line
is subtle, the entry states "provision X / do not add Y". Provision only where
retrofit is costly **and** growth is plausible for *this* app, **sized in
proportion** (headroom, not infinite generality).

**Scope.** h2p is frontend-centric (backend `none` or `mock`). Tags:
`FE` frontend · `BE(mock)` effective only with a mock backend · `cond`
provision only if the input shows a trace · `handoff` out of default scope,
left to the generated CLAUDE.md.

Reading an entry: **blast radius** = what a late retrofit touches.

---

## A. Literal / value externalization (touches every literal)
| Element | Blast radius | Scope |
|---|---|---|
| i18n string externalization (+ locale-aware date/number/currency, plurals, RTL) | every string | FE — provision: externalized, locale-ready strings / do not add: translation catalogs, language-switch UI |
| Design tokens / theming (color, spacing, typography → variables) | every style rule | FE (HTML prototypes hardcode these) |
| Config / env externalization (env strategy, dev/stg/prod, feature-flag seam; twelve-factor) | every hardcoded value | FE / BE(mock) |
| Magic constants → named constants / enums | scattered literals | FE / BE(mock) |

## B. Type & contract rigor (touches every boundary)
| Element | Blast radius | Scope |
|---|---|---|
| **TypeScript from the start** (adopt the type system) | JS→TS migration is the largest retrofit | FE / BE(mock) — top priority |
| Strict domain / API types (one canonical source for shapes) | types ripple everywhere | FE / BE(mock) |
| API boundary conventions (consistent error shape, versioning, pagination, DTO⇄domain split) | breaking changes to consumers | BE(mock) |
| Validation-schema single source (shared by forms and API) | double-maintained, drifting validation | FE / BE(mock) |

## C. Security readiness (touches every entry point)
| Element | Scope |
|---|---|
| Input validation at trust boundaries / output encoding (XSS) | FE / BE — provision: validation, encoding, no hardcoded secrets, typed contracts / do not add: the auth *feature* |
| Authz-check placement, CORS / CSP posture | FE / BE |
| Password/secret hashing done right (bcrypt/scrypt, never home-grown) | cond / handoff — only where auth already exists |

## D. Cross-cutting runtime concerns (woven through every layer)
| Element | Scope |
|---|---|
| Error handling / error-boundary strategy (consistent) | FE / BE |
| Async states (loading / empty / error) handled uniformly | FE |
| Logging / observability seams (correlation id, error-tracking hooks) | FE light / BE heavier |

## E. Structural seams (decide what can change independently)
| Element | Blast radius | Scope |
|---|---|---|
| Layer separation + dependency direction (UI / logic / data; no cycles) — core | untangle coupling | FE / BE |
| State-management architecture (server-state ⇄ client-state split; data-fetch/cache placement) | rewrite data flow | FE |
| Routing / navigation structure (URL design, deep-linkability) | rework navigation | FE |
| Render-strategy readiness (CSR / SSR / SSG): keep components render-agnostic, avoid hard `window`/`document` coupling | major rework to switch later | FE (the choice itself is Phase 5) |
| **Performance seams (code-split boundaries, lazy-load points; Core Web Vitals awareness)** | restructure to split later | FE — medium |
| Testability seams (pure functions, DI, no hidden globals / direct-DOM coupling) | restructure to make testable | FE / BE |
| Module / directory organization (feature-sliced or layered) | mass file moves | FE / BE |
| Naming / ubiquitous language (ledger + naming grammar) — core | rename cascade | FE / BE |

## F. UI integrity (HTML-specific, easily lost)
| Element | Scope |
|---|---|
| Accessibility: semantic HTML / ARIA / keyboard / focus management / contrast | FE — highest here. provision: semantic structure + a11y readiness / do not add: new UI features |
| Responsive / layout strategy (breakpoint design) | FE — retrofitting a fixed layout touches all layout CSS |
| SEO-semantic / meta (structure overlaps a11y; meta tags are cheap) | FE |

## G. Process & knowledge discipline (rationale / quality decays)
| Element | Scope |
|---|---|
| Build discipline from day one (lint / format / typecheck / CI) | violations accumulate if retrofitted late |
| Architecture docs / ADR (decision rationale is lost over time) | FE / BE |

---

## Conditional — provision only if the input shows a trace
- **Real-time (WebSocket / SSE):** retrofitting real-time onto request/response is expensive, but it is feature-driven — provision only when the input implies it.
- **Offline / PWA / service workers:** mostly a feature; preserve if the input already has it, do not add otherwise.

## Out of default scope — hand off to CLAUDE.md
Provision none of these by default (frontend + mock only); they take effect only when a real backend/infra is built, so leave them to post-migration development:
- Persistence schema / migrations, transactions / ACID / concurrency, DB scaling (indexes, replication, sharding, CAP)
- Cache infrastructure (Redis / CDN), message brokers / queues / background jobs, search engines
- Containerization / orchestration (Docker / K8s), web servers (Nginx), scaling (throttling / backpressure / circuit breaker), microservices / service-mesh / serverless topology
- CQRS / Event Sourcing and other advanced backend patterns
- DevOps / deployment / IaC (cloud services, GitHub Actions, Ansible, Terraform, Linux) — h2p ends at "dev server runs + CLAUDE.md"; deployment is post-handoff

## Umbrella names (already covered, just naming them)
- **DDD** = ubiquitous language + bounded contexts + module boundaries (E + naming).
- **TDD** = the testability seams in E.
- **Twelve-factor** = config/env externalization in A.

---

## How each Phase consults this catalog
- **Phase 1 (analysis):** observe, as **facts**, which readiness elements the
  input already has vs. lacks (hardcoded values, non-semantic DOM, injected
  secrets, no types, fixed layout, hidden globals, …). Record them; do not
  fix yet. This does not belong in the behavior checklist (that is about
  behavior); keep it as structural observations.
- **Phase 3 (structure):** let the structural seams (E) and contracts (B)
  shape the target structure and diagrams.
- **Phase 4 (refactoring):** lay the foundations that can be established
  in place without changing behavior — externalize literals (A), semantic /
  a11y (F), tokens (A), error / async states (D), testability seams (E).
  Each still passes the P4 behavior gate.
- **Phase 5 (tech-stack selection):** bind choices to foundations — TypeScript
  (B), validation library (B), config strategy (A), render strategy (E),
  test tooling (E/G). Reject anything a foundation does not require
  (First Principle 2).
