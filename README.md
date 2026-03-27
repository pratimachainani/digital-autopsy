# Digital Autopsy by Silicon Surgeons

> Reverse engineering for legacy modernization with structured prompts.

Digital Autopsy is an open-source prompt library and methodology for extracting business capabilities from legacy codebases. It produces PM-ready documentation that stakeholders can validate before any modernization work begins.

## The problem

Legacy systems have code. They don't have documentation.

Understanding the code ≠ Understanding the business.

Teams reverse engineer legacy systems and produce technical descriptions that no PM can read, no BA can validate, and no architect can plan migration from. The result: modernization projects start with assumptions instead of facts.

## The approach

Two structured prompts, run sequentially, that translate code into business language — repeatable, auditable, and stakeholder-ready.

### Core prompt chain

| Step | Prompt | What it does | Output |
|------|--------|-------------|--------|
| 1 | [01-discovery](prompts/core/01-discovery.md) | Deterministic repo scan — finds every entry point, integration, schema, and data flow | `discovery-index.json` |
| 2 | [02-extraction](prompts/core/02-extraction.md) | Extracts business capabilities with Keep/Change/Drop recommendations, migration seams, and a validation session guide | `CAPABILITY-INVENTORY.md` |

Run 01 first. Feed the codebase. Review the discovery index. Then run 02 to translate code into business capabilities.

### ML model analysis

Run this independently whenever you need to understand how an ML model is integrated into a codebase.

| Prompt                                            | What it does | Output |
|---------------------------------------------------|-------------|--------|
| [ml-model-analysis](prompts/ml-model-analysis.md) | Deep-dive on one ML model's integration — request/response attributes, invocation points, business rules | `{model}_analysis.md` |


## Quick start

1. Clone this repo
2. Open your legacy codebase in your preferred AI tool (Claude, GPT-4, OpenCode, Cursor, Claude Code)
3. Copy the prompt from `prompts/core/01-discovery.md` and run it against your codebase
4. Review the `discovery-index.json` output — verify the entry points and integrations
5. Run `prompts/core/02-extraction.md` to produce the capability inventory
6. Take the output to your product team for validation

## Tool compatibility

These prompts work with any LLM that supports large context windows and tool use:

- **Claude** (Anthropic) — 200K context, excellent for all prompts
- **GPT-4** (OpenAI) — works well with chunked input
- **Gemini** (Google) — 1M+ context, good for full-repo scans
- **OpenCode / Dayton** — use these prompts as agent system instructions
- **Claude Code** — paste prompts as slash commands
- **Cursor** — use as project-level rules

For large monorepos: the discovery prompt has been tested on full repositories. For codebases exceeding your model's context window, chunk by module boundary.

## Templates

Blank templates for manual use — no AI required:

- [Capability template](templates/capability-template.md) — CAP-XXX format for documenting one business capability
- [Seam analysis worksheet](templates/seam-analysis-worksheet.md) — Score migration seams to decide where to start strangling
- [Decision matrix](templates/decision-matrix.md) — Keep/Change/Drop framework for capability validation

## Methodology

See [docs/METHODOLOGY.md](docs/METHODOLOGY.md) for the full Digital Autopsy framework — the four phases, the capability-vs-stage distinction, the seam scoring rubric, and the Keep/Change/Drop decision matrix. The methodology works independently of the prompts — you can run it manually as a team exercise.

## Roadmap

- Dedicated seam identification prompt (migration seam scoring as a standalone pass)
- Decision point synthesis (aggregate all 🔍 decision points into one stakeholder document)
- Test gap analysis (identify business rules with no test coverage)

## Origin

**Presenter:** Pratima Chainani

## License

MIT
