# Evidence Policy

Every technical decision must be backed by a qualifying source.
This policy defines what counts.

---

## Acceptable Sources

| Tier | Type | Examples |
|------|------|---------|
| 1 | Official documentation | MDN, React docs, Rust Book, Python docs |
| 2 | RFC / Specification | IETF RFCs, W3C specs, ECMAScript spec |
| 3 | Framework release notes | GitHub Releases, CHANGELOG.md |
| 4 | Tracked issues / PRs | GitHub Issues, Jira tickets |
| 5 | Community Q&A | StackOverflow (accepted or highly-voted answers) |
| 6 | Technical articles | Blog posts with named author, publication date, and technical depth |

---

## Rejected Sources

- ❌ AI-generated explanations (including Claude itself)
- ❌ Undated or authorless blog posts
- ❌ Marketing copy or landing pages
- ❌ Personal opinion without supporting data
- ❌ "It worked on my machine"

---

## Minimum Evidence by Change Type

| Change Type | Min Sources | Preferred Tier |
|-------------|-------------|----------------|
| Bug fix | 1 | Tier 4 or higher |
| Feature | 1 | Tier 1–3 |
| Refactor | 1 | Tier 1–3 |
| Architecture change | 2 | Tier 1–2 |
| Migration | 2 | Tier 1–3 |
| Security change | 3 | Tier 1–2 mandatory |
| Dependency upgrade | 1 | Tier 3 (release notes) |

---

## When You Can't Find Evidence

If a meaningful change lacks a qualifying source:

1. State this explicitly in the Evidence section
2. Explain what you searched for and where
3. Reduce the Evidence dimension of the Confidence Score proportionally
4. Add a Follow-up Task: "Find authoritative source for [decision]"

Do not fabricate or hallucinate citations.
