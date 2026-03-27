# Seam Analysis Worksheet

Use this worksheet to score migration seams and determine the strangling sequence.

---

## Scoring rubric

| Criteria | Score |
|----------|-------|
| Clear contract/interface exists | +2 |
| Stateless or state can be replicated | +2 |
| Monitoring/observability in place | +1 |
| Feature flag already exists | +2 |
| Shared database writes | −2 |
| Hidden temporal dependencies | −3 |

**Start strangling at seams scoring ≥ 4.**

---

## Seam inventory

| Seam ID | Name | Type | Score | Evidence | Risk | Notes |
|---------|------|------|-------|----------|------|-------|
| SEAM-001 | | API / Event / DB / Flag | | | High/Med/Low | |
| SEAM-002 | | | | | | |
| SEAM-003 | | | | | | |
| SEAM-004 | | | | | | |
| SEAM-005 | | | | | | |

---

## Scoring detail (fill per seam)

### SEAM-XXX: [Name]

| Criteria | Score | Evidence |
|----------|-------|----------|
| Clear contract/interface exists | +2 / 0 | |
| Stateless or replicable state | +2 / 0 | |
| Monitoring/observability in place | +1 / 0 | |
| Feature flag already exists | +2 / 0 | |
| Shared database writes | 0 / −2 | |
| Hidden temporal dependencies | 0 / −3 | |
| **Total** | **___** | |

**Capabilities affected:** CAP-XXX, CAP-YYY

**Migration approach:** [How would you strangle at this seam?]

**Risks:** [What could go wrong?]

---

## Recommended strangling sequence

| Order | Seam | Score | Rationale |
|-------|------|-------|-----------|
| 1 | | | Highest score, lowest risk — builds confidence |
| 2 | | | |
| 3 | | | |

---

## Seams that need to be created

| Area | What's missing | Effort to create |
|------|---------------|-----------------|
| | | Low / Medium / High |
