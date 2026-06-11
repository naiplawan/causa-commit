# Commit Message Template

## Format

```
type(scope): short imperative summary (max 72 chars)

Why: [the problem being solved — not the solution]
Impact: [what changes for users or downstream systems]
Evidence: [1-line citation, or "internal refactor / no external dependency"]
Risk: [Low / Medium / High — one sentence on why]
```

## Types

| Type | Use when |
|------|----------|
| `feat` | New capability for users |
| `fix` | Bug correction |
| `refactor` | Code restructure, no behavior change |
| `perf` | Performance improvement |
| `security` | Security hardening |
| `chore` | Tooling, dependencies, build |
| `docs` | Documentation only |
| `test` | Tests only |
| `migration` | Data or schema migration |
| `infra` | Infrastructure / CI / deployment |

## Scope

Use the affected module, component, or domain. Examples:
- `auth`, `forms`, `api`, `db`, `cache`, `ui`, `payments`
- Use `*` if the change cuts across the entire codebase

## Examples

```
fix(auth): prevent session token reuse after logout

Why: logout didn't invalidate server-side token, allowing replay attacks
Impact: active sessions are now invalidated on logout
Evidence: OWASP Session Management Cheat Sheet — https://cheatsheetseries.owasp.org/...
Risk: Medium — users will be logged out of all devices on next deploy
```

```
feat(forms): add autosave draft on blur

Why: users lost work when navigating away from long forms
Impact: drafts are saved to localStorage every time a field loses focus
Evidence: UX research finding #47 — internal Notion doc
Risk: Low — additive change, no existing behavior modified
```

```
refactor(db): extract query builder into separate module

Why: inline SQL strings spread across 12 files made auditing impossible
Impact: no runtime behavior change; queries now centralized in db/queries/
Evidence: internal refactor — no external dependency
Risk: Low — pure move, tests pass before and after
```
