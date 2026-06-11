# Commit Review

## Executive Summary

[2–3 sentences. What changed, why, and is it safe to commit?]

---

## Change Map

| File | Change Type | Description | Risk |
|------|-------------|-------------|------|
| `path/to/file` | modified | [what changed] | Low |

---

## Intent Analysis

**Expected Goal:** [what the engineer was trying to do]

**Observed Implementation:** [what the code actually does]

**Alignment:** ✅ Strong / ⚠ Partial / ❌ Weak

**Missing Intent:** [anything implied but not addressed, or "None"]

---

## Evidence

### Claim: [technical decision being justified]

[1–2 sentence explanation]

- Source: [title] — [URL] — [date]

---

## Risk Matrix

| Dimension | Risk | Severity | Confidence | Notes |
|-----------|------|----------|------------|-------|
| Correctness | ... | Low | 90% | |
| Performance | ... | Low | 90% | |
| Security | ... | Low | 90% | |
| Operations | ... | Low | 90% | |

---

## Suggested Commit Message

```
type(scope): short imperative summary

Why: [problem being solved]
Impact: [what changes for users or downstream]
Evidence: [key source or "internal refactor"]
Risk: Low / Medium / High
```

---

## PR Description

### Problem

[What issue does this solve?]

### Solution

[How does the implementation solve it?]

### Evidence

- [source link]

### Validation

[How was this tested?]

### Risks

[Known risks or "None identified"]

### Rollback

[How to revert if needed]

---

## Follow-up Tasks

- [ ] [item, if any]

---

## Confidence Score

| Dimension | Score | Notes |
|-----------|-------|-------|
| Change Understanding | /25 | |
| Intent Alignment | /25 | |
| Evidence Quality | /25 | |
| Risk Coverage | /25 | |
| **Total** | **/100** | |

**Interpretation:** [Ready / Needs review / Needs validation / Block commit]
