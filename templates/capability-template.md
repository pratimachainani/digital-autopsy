# Capability Template

Use this template to document one business capability extracted from legacy code.

---

## CAP-XXX: [Capability Name]

**Business purpose:** 2-3 sentences. Why does this capability exist? What business problem does it solve? What would break if it didn't exist?

**Trigger:** What causes this capability to execute?

**Inputs:**

| Input | Business meaning | Required? | Source |
|-------|-----------------|-----------|--------|
| | | Yes/No | |

**Outputs:**

| Output | Business meaning | Destination |
|--------|-----------------|-------------|
| | | |

**Business rules:**

```
RULE CAP-XXX.1: [Rule Name]
WHEN [condition in business terms]
THEN [outcome]
OTHERWISE [alternative outcome]
RATIONALE: [why this rule exists — or "Intent unclear, needs product input"]
```

**Success outcome:** What does "done" look like? Be specific about the end state.

**Failure scenarios:**

| Scenario | System behavior | Severity | Recovery |
|----------|----------------|----------|----------|
| | | Critical/Warning/Info | |

**Modernization assessment:**

| Aspect | Assessment |
|--------|-----------|
| Business rule clarity | Clear / Partially clear / Unclear — needs product input |
| Implementation quality | Clean / Has tech debt / Significant rework needed |
| Recommendation | ✅ Keep as-is / 🔄 Keep rule, change implementation / ❓ Needs product decision / 🗑️ Candidate for removal |
| Rationale | |

> **🔍 Modernization Decision: Keep / Change / Drop?**
>
> **Current behavior:** [describe]
>
> **Question for product team:** [what needs to be decided]

<details>
<summary>📁 Code references (for developer verification)</summary>

| Claim | File | Location |
|-------|------|----------|
| | | |

</details>
