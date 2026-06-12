# causa-commit

> Evidence-backed pre-commit engineering review for Claude Code.

**causa** (Latin) — *cause, reason, ground*

A commit is not a backup. It is an engineering decision.
Every commit must answer: what changed, why it changed, what can break, and how confident we are.

---

## What it does

When you invoke `/causa-commit`, Claude runs a structured 5-phase review of your current git changes and produces a full engineering report:

| Phase | Output |
|-------|--------|
| Discover | Change Map — every modified file with type and risk level |
| Intent Reconstruction | Expected goal vs. observed implementation, alignment score |
| Evidence Collection | Authoritative sources justifying each technical decision |
| Risk Review | Risk matrix across Correctness, Performance, Security, Operations |
| Commit Generation | Conventional commit message + PR title + PR description |

The report ends with a **Confidence Score (0–100)** that tells you whether it is safe to commit.

---

## Installation

```bash
npx claude skill add https://github.com/rachaphol/causa-commit
```

---

## Usage

```bash
# From any Claude Code session in a git repo:
/causa-commit

# Or by natural language:
"review before I commit"
"summarize staged changes"
"generate a commit message"
"what did I change?"
```

---

## Confidence Score

| Score | Interpretation |
|-------|----------------|
| 90–100 | Ready to commit |
| 70–89 | Ready with review — specific concerns flagged |
| 50–69 | Needs validation before commit |
| < 50 | Block commit — investigate first |

---

## Output structure

```
# Commit Review

## Executive Summary
## Change Map
## Intent Analysis
## Evidence
## Risk Matrix
## Suggested Commit Message
## PR Description
## Follow-up Tasks
## Confidence Score
```

---

## Evidence policy

Every non-trivial technical decision must be backed by a qualifying source.

**Accepted:** Official docs · RFC/spec · Library release notes · GitHub issues · StackOverflow (accepted answers)

**Rejected:** AI-generated explanations · Marketing pages · Undated/authorless blogs · Personal opinion

Minimum sources required:

| Change type | Min sources |
|-------------|-------------|
| Bug fix | 1 |
| Feature / refactor | 1 |
| Architecture change | 2 |
| Migration | 2 |
| Security change | 3 |

See `docs/evidence-policy.md` for the full policy.

---

## File structure

```
causa-commit/
├── SKILL.md                  — skill definition and workflow
├── README.md                 — this file
├── templates/
│   ├── review.md             — full report template
│   ├── commit.md             — conventional commit guide and examples
│   └── pr.md                 — PR description template with checklist
├── docs/
│   └── evidence-policy.md    — source quality rules by change type
└── evals/
    └── evals.json            — test cases for skill evaluation
```

---

## Conventional commit format

```
type(scope): short imperative summary

Why: [problem being solved]
Impact: [what changes for users or downstream systems]
Evidence: [key source, or "internal refactor"]
Risk: [Low / Medium / High — one sentence]
```

**Types:** `feat` `fix` `refactor` `perf` `security` `chore` `docs` `test` `migration` `infra`

---

## Example report (abbreviated)

From a real review of commit `8ab8f14` in a Go backend:

> **Executive Summary:** The commit fixes a silent pagination bug — `GetAll` was fetching every record from the DB and using `len(logs)` as the total count. Real DB-level `OFFSET`/`LIMIT` is now applied and a separate `COUNT` query returns accurate totals.
>
> **Confidence Score: 89/100 — Ready with review**
> - Interface change breaks any unregistered mock/test double
> - `LIKE '%term%'` with leading wildcard prevents index use on large tables

---

## Optional: pre-commit hook

To gate commits on confidence score, add to `.git/hooks/pre-commit`:

```sh
#!/bin/sh
# Requires jq and the Claude CLI
SCORE=$(claude -p "/causa-commit" | grep -oP '(?<=Total\*\* \| \*\*)\d+')
if [ "$SCORE" -lt 70 ]; then
  echo "causa-commit: confidence $SCORE/100 — review required before committing"
  exit 1
fi
```
