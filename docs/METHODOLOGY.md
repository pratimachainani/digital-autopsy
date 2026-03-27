# Digital Autopsy — Methodology

## What is a Digital Autopsy?

A systematic examination of legacy code to extract business meaning — not just technical facts.

Three layers of understanding:

| Layer | Question | Who cares |
|-------|----------|-----------|
| **Technical** | What does the system do? | Developers |
| **Business** | What does the system mean? | PMs, BAs |
| **Strategic** | What should the system become? | Architects, leadership |

Most reverse engineering stops at the technical layer. Digital Autopsy produces all three.

## The four phases

### Phase 1: Discovery — Find facts

Scan the codebase and identify every way data enters and exits the system.

**Entry point types to find:**
- Event consumers (Kafka listeners, SQS handlers, webhooks)
- API controllers (REST endpoints, GraphQL resolvers)
- Scheduled jobs (cron, Quartz, batch processors)
- Database triggers (stored procedures, CDC)

**Output:** A structured index of entry points, integrations, schemas, and data flows.

**Key rule:** Cite the code, or don't claim it. Every assertion must reference a specific file and line range.

### Phase 2: Extraction — Find meaning

Translate what you found in Phase 1 into business capabilities.

**The critical distinction — capabilities vs stages:**

A *stage* is implementation — how the code is organized. It might be merged, split, or eliminated in the new system.

A *capability* is a business requirement — it must be preserved or consciously dropped.

| Bad (stage-centric) | Good (capability-centric) |
|---|---|
| "Run BarcodeValidationEnrichment after ContentIngestion" | "Every product must have a unique, scannable barcode before going live" |
| "Execute PriceCalculationStep in the pipeline" | "Apply supplier-specific margins before catalog publish" |

**For each capability, document:**
- What it does (one sentence a PM can validate)
- Why it exists (business driver or regulation)
- Inputs and outputs
- Business rules as policy statements
- Keep / Change / Drop recommendation

### Phase 3: Validation — Confirm truth

Get PM/BA review on the extracted capabilities.

**The Keep / Change / Drop matrix:**

| Capability | Business value | Tech debt | Decision |
|---|---|---|---|
| Barcode validation | High | Low | **Keep** |
| Legacy PDF reports | None | High | **Drop** |
| Manual override flow | Medium | High | **Change** |

Other validation frameworks to consider:
- Wardley Mapping — for strategic positioning
- ATAM (Architecture Tradeoff Analysis Method) — for architecture quality tradeoffs
- Fitness function-driven assessment — for measurable architectural criteria

**Surface decision points, don't hide them.** When the business reason for a behavior is unclear, frame it as a decision for stakeholders with concrete options and an owner.

### Phase 4: Decision — Plan action

Score migration seams and plan the strangling sequence.

**The Strangler Fig pattern:** Grow the new system alongside the old, migrating function by function. The business never experiences downtime.

**Seam types:**

| Seam type | What to look for | Migration potential |
|---|---|---|
| API boundary | REST/gRPC endpoints that can be re-routed | High |
| Event boundary | Kafka topics, queues — add new consumers | High |
| Database boundary | Separate schemas, different DBs | Medium |
| Feature flag | Existing toggles between implementations | High |

**Seam scoring rubric:**

| Criteria | Score |
|---|---|
| Clear contract/interface exists | +2 |
| Stateless or state can be replicated | +2 |
| Monitoring/observability in place | +1 |
| Feature flag already exists | +2 |
| Shared database writes | −2 |
| Hidden temporal dependencies | −3 |

**Rule of thumb:** Start strangling at seams scoring ≥ 4.

## Why structured prompts?

| Aspect | For autopsy reports | For exploration |
|---|---|---|
| Approach | Structured prompts | Autonomous agents |
| Strength | Consistent, auditable output | Discovering unknowns |
| Repeatability | Same prompt → same format | Creative, may vary |
| Human review | Easy — clear input/output | Harder to intervene |

**The hybrid:** Use structured prompts for the formal report. Use agents (Claude Code, OpenCode, Cursor) for ad-hoc exploration when something looks interesting.

## The Surgeon's Oath

1. I will not document what I cannot prove from code.
2. I will translate technical findings into business impact.
3. I will identify migration seams before recommending surgery.
4. I will surface decisions, not make them for stakeholders.
