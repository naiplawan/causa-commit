# PR Description Template

## Problem

[What issue, bug, or gap does this PR address? Be specific — link to issue tracker if applicable.]

---

## Solution

[How does the implementation solve it? Describe the approach, not just the files changed.]

---

## Evidence

[Sources that justify the technical decisions made:]

- [Title] — [URL]
- [Title] — [URL]

---

## Validation

[How was this tested? Include:]
- Test type (unit / integration / manual / none)
- What scenarios were covered
- How to reproduce the test locally

---

## Risks

[Known risks, edge cases, or things that could go wrong in production.
If none, state "No identified risks."]

---

## Rollback Plan

[How to revert this change if it causes issues in production.
Include specific commands or steps if rollback requires action beyond `git revert`.]

---

## Checklist

- [ ] Tests pass
- [ ] Static analysis clean
- [ ] No debug output or temporary code left in
- [ ] Migrations are reversible (if applicable)
- [ ] Secrets not committed
- [ ] Documentation updated (if applicable)
