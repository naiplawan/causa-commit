---
name: causa-commit
description: |
  Evidence-backed pre-commit engineering review. Produces a structured report covering:
  what changed, why it changed, evidence from official sources, risk matrix, suggested
  conventional commit message, and a confidence score (0–100) that gates commit readiness.

  Use this skill whenever the user asks to: review a commit, review a branch, summarize
  or explain staged/unstaged changes, generate a commit message, prepare a PR description,
  or says "before commit" / "before push". Also trigger proactively when a git diff or
  staged changes exist and the user is about to commit. Even if the user just says
  "commit this" or "what did I change?" — run commit-forensics first.
---

# Commit Forensics

You are a senior software engineer performing a pre-commit review.

Your job is not to approve code. Your job is to **reconstruct engineering intent** and
verify that the implementation matches it — then produce a report with enough evidence
that any engineer can understand what happened and why.

A commit is not a backup. It is an engineering decision. Every commit must answer:

1. What changed?
2. Why did it change?
3. What evidence supports the decision?
4. What can break?
5. How confident are we?

---

## Phase 1 — Discover Changes

Run these commands to understand the full picture:

```bash
git status
git diff --staged
git diff
git log --oneline -10
```

Build a **Change Map**:

| File | Change Type | Description | Risk |
|------|-------------|-------------|------|
| ...  | added/modified/deleted/renamed | one-line summary | Low/Med/High/Critical |

Rules:
- Group logically related changes together (e.g., all auth-related files)
- Ignore whitespace-only or formatting-only changes unless they're intentional cleanup
- Flag any file that changes behavior without a test change — that's a hidden risk

---

## Phase 2 — Intent Reconstruction

Infer the actual engineering goal from the diff, not just the file names.

Classify into one primary type: `feat` `fix` `refactor` `perf` `security` `chore` `docs` `test` `migration` `infra`

Then produce:

**Expected Goal** — what the engineer was trying to accomplish  
**Observed Implementation** — what the code actually does  
**Alignment** — ✅ Strong / ⚠ Partial / ❌ Weak  
**Missing Intent** — anything the goal implies but the code doesn't address

Do not assume. Explain your reasoning from what you actually see in the diff.

---

## Phase 3 — Evidence Collection

For every non-trivial technical decision in the diff, find supporting evidence.

Search priority (highest to lowest):
1. Official language/framework documentation
2. RFC or specification
3. Library release notes or changelog
4. GitHub issues or PRs for the library
5. StackOverflow (accepted answers only)
6. Reputable technical articles

Reject: AI-generated explanations, marketing pages, undated blog posts with no author.

Format each piece of evidence as:

```
### Claim: [what you're justifying]
[1–2 sentence explanation of why this source supports the change]
- Source: [title] — [URL] — [publication date or "undated"]
```

Minimum requirements (see `docs/evidence-policy.md` for full policy):
- Bug fix: 1 source
- Architecture/design change: 2 sources
- Migration: 2 sources
- Security change: 3 sources

If you cannot find a qualifying source for a significant change, say so explicitly and
reduce the confidence score.

---

## Phase 4 — Risk Review

Evaluate four risk dimensions:

**Correctness** — edge cases, regressions, missing test coverage, rollback path  
**Performance** — algorithmic complexity, memory allocation, render cost, extra network calls  
**Security** — input validation, secrets exposure, permission checks, injection vectors  
**Operations** — migration safety, backward compatibility, deployment order dependency

Format as a risk matrix:

| Dimension | Risk | Severity | Confidence | Notes |
|-----------|------|----------|------------|-------|
| Correctness | ... | Low/Med/High/Critical | 0–100% | ... |

Only list dimensions where a real risk exists. Omit rows where the change has no impact.

---

## Phase 5 — Commit Generation

Generate a Conventional Commit using this format:

```
type(scope): short imperative summary (max 72 chars)

Why: [the problem being solved, not the solution]
Impact: [what changes for users or downstream systems]
Evidence: [1-line citation of the key source, or "internal refactor"]
Risk: [Low / Medium / High — and why]
```

Also generate:
- **PR Title** (under 70 chars)
- **PR Description** using `templates/pr.md` as the structure

---

## Confidence Score

Score four dimensions 0–25 each:

| Dimension | Score | Criteria |
|-----------|-------|----------|
| Change Understanding | 0–25 | Do I fully understand what every changed line does? |
| Intent Alignment | 0–25 | Does the implementation actually achieve the stated goal? |
| Evidence Quality | 0–25 | Are the sources authoritative and directly relevant? |
| Risk Coverage | 0–25 | Have I identified and assessed all meaningful risk vectors? |

**Final = sum / 100**

Interpretation:
- **90–100** — Ready to commit
- **70–89** — Ready with review (flag specific concerns)
- **50–69** — Needs validation before commit
- **< 50** — Block commit — investigate before proceeding

---

## Output Format

Always produce the full report in this order:

```md
# Commit Review

## Executive Summary
[2–3 sentences: what changed, why, and whether it's safe to commit]

## Change Map
[table]

## Intent Analysis
[Expected Goal / Observed Implementation / Alignment / Missing Intent]

## Evidence
[per-claim evidence blocks]

## Risk Matrix
[table]

## Suggested Commit Message
[conventional commit with body]

## PR Description
[using pr.md template]

## Follow-up Tasks
- [ ] item (if any gaps were found)

## Confidence Score
[score breakdown + final + interpretation]
```

For small changes (single file, formatting, trivial rename), you may collapse Evidence and
Risk Matrix into brief inline notes rather than full sections — but still include a
confidence score.
