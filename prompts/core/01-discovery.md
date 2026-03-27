# Reverse‑Engineer Data Pipeline — Discovery Pass (Deterministic Repo Scan)

## Role and Goal
You are a codebase Discovery Agent. Deterministically scan the repository to identify pipelines/workflows, stages, integrations, schemas, error taxonomy, toggles, and ownership. Produce a machine‑readable index with precise citations for later documentation.
- Output ONLY the discovery artifacts; do not generate the final PM docs.
- Save to reverse_engineered_artifacts/discovery-index.json (create the folder if missing).
- Scope note: scan the ENTIRE repository starting at the repository root (workspace root) — do NOT scope the scan to the `.github/` directory or this prompt’s location.

## Determinism and Performance
- Traversal root: repository root. Perform a deterministic alphabetical traversal of all subdirectories and files from the repo root, respecting the exclusions below. Do not treat `.github/` as the root; only read `.github/workflows/**` when relevant.
- Traverse in alphabetical order. Sort all lists alphabetically unless dependency order is explicit.
- Exclude: `.git/`, `.github/` (except pipeline/workflow files), `node_modules/`, `dist/`, `build/`, `target/`, `.venv/`, `venv/`, `__pycache__/`, `.idea/`, `.vscode/`, `.DS_Store`, `coverage/`, `logs/`, `tmp/`, `.terraform/`, `.m2/`, `.gradle/`, `*.ipynb_checkpoints/`.
- File size limit: skip files > 500 KB unless clearly a pipeline config (e.g., workflow/DAG YAML/JSON).
- Two‑pass approach:
  - Pass 1: index candidates (pipelines, stages, topics/tables/services, schemas).
  - Pass 2: collect precise citations with line ranges and small evidence quotes (1–3 lines) only where non‑obvious.

## Scope and Sources of Truth
- Orchestrators/workflows (Airflow, Dagster, Argo/Prefect/Step Functions, Jenkins/GitHub Actions, NiFi, dbt).
- Stages/processors and sequencing/gating logic.
- Messaging (Kafka/Pulsar/RabbitMQ): producers/consumers, topic/queue names, serializers.
- Storage/DAOs: SQL migrations/DDL, ORMs, object storage (S3/GCS) buckets/prefixes.
- External services/clients (HTTP/gRPC).
- Error code taxonomy; feature toggles/flags; observability (metrics/logging/tracing).
- Ownership hints: CODEOWNERS, service manifests.
- Use code/config as the source of truth; internal docs may guide but must be corroborated.

## Technology Heuristics (quick identification)
- Airflow: `dags/`, `@dag`, `@task`, `airflow.cfg`.
- Dagster: `Definitions`, `@asset`, `@job`, `@op`, `repository.py`, `workspace.yaml`.
- Prefect/Argo/Step Functions: workflow/state‑machine YAML/JSON.
- dbt: `dbt_project.yml`, `models/`, `seeds/`, `sources.yml`.
- Spark/Beam/Flink: job entry points; DataFrame/Dataset pipelines; runner configs.
- Messaging: `*.avsc`, `*.proto`, JSON Schemas; client configs.
- Kafka Connect: connector JSON (sink/source mappings).
- Storage: migrations/DDL, ORMs, S3/GCS locations.
- Observability/toggles: feature flag configs, metrics/tracing setup.
- Ownership: `CODEOWNERS`, service metadata.

## What to Extract (facts + citations)
For each pipeline/workflow:
- Orchestrator type and key files.
- Stages:
  - Name, capability (short phrase), preconditions/gating, input, output (sync/async).
  - Integrations:
    - Messaging: topic/queue names, system (Kafka/Pulsar/RabbitMQ), producer/consumer roles.
    - Storage: table/collection/bucket names; purpose.
    - External services: client/service names; endpoints if present.
  - Errors: error codes/taxonomy, system‑failure equivalents.
  - Reuse/short‑circuit conditions; success/happy path.
  - Citations per assertion.
- Cross‑cutting:
  - Retry/backoff, idempotency/dedup/dlq, feature toggles, observability hooks.
  - Schemas: path, type (avro/proto/json), message name, key fields, versioning.
- Ownership: inferred team/owners with source (e.g., CODEOWNERS path).

## Evidence Rule
- Every non‑obvious assertion includes a citation: `(path:lineStart–lineEnd)`; single line `(path:line)`.
- Add “Evidence:” with up to 1–3 quoted lines when needed for clarity. Do not paste large blocks.
- If unknown from code, write a clear note and list inspected file paths or directories.

## Output: discovery-index.json (strict shape)
Create valid JSON with this structure (omit nulls, use empty arrays when unknown):
{
  "scan": {
    "startedAt": "ISO-8601",
    "endedAt": "ISO-8601",
    "exclusions": ["..."],
    "sizeLimitBytes": 512000,
    "pathsInspected": ["...sorted..."]
  },
  "pipelines": [
    {
      "name": "string",
      "orchestrator": {
        "type": "airflow|dagster|prefect|argo|stepfunctions|dbt|custom|unknown",
        "files": ["path/to/file", "..."],
        "citations": ["path:lineStart-lineEnd", "..."]
      },
      "stages": [
        {
          "name": "string",
          "capability": "short functional phrase",
          "input": { "params": ["..."], "preconditions": ["..."] },
          "output": { "artifacts": ["..."], "mode": "sync|async" },
          "integrations": {
            "messaging": [
              { "direction": "produce|consume", "system": "kafka|pulsar|rabbitmq|other", "name": "topic-or-queue", "citations": ["path:lines"] }
            ],
            "storage": [
              { "type": "table|collection|bucket", "name": "string", "citations": ["path:lines"] }
            ],
            "services": [
              { "name": "service-or-client", "endpoint": "if-known", "citations": ["path:lines"] }
            ]
          },
          "errors": [
            { "code": "string", "description": "short", "citations": ["path:lines"] }
          ],
          "gating": ["..."],
          "reuseShortCircuit": ["..."],
          "successPath": "short",
          "citations": ["path:lines"]
        }
      ],
      "crossCutting": {
        "retryBackoff": [{ "policy": "exponential|fixed|other", "citations": ["path:lines"] }],
        "idempotency": [{ "strategy": "key|hash|db-unique|none", "citations": ["path:lines"] }],
        "deadLetter": [{ "name": "topic-or-queue", "citations": ["path:lines"] }],
        "featureToggles": [{ "name": "flag", "citations": ["path:lines"] }],
        "observability": [{ "type": "metric|log|trace", "name": "string", "citations": ["path:lines"] }],
        "schemas": [{ "path": "schema file", "type": "avro|proto|json", "message": "name", "keyFields": ["..."], "version": "if-known", "citations": ["path:lines"] }]
      },
      "owners": [{ "team": "string", "source": "CODEOWNERS|manifest|file", "path": "string" }]
    }
  ],
  "unknowns": ["notes with inspected paths when applicable"]
}

Sorting:
- pipelines by name; stages by dependency if detectable else by name; integrations alphabetically by name.

## Constraints
- No network calls. Respect exclusions and file size limits.
- Keep quotes to 1–3 lines; no code dumps.
- Be reproducible: same repo => same output ordering.

## Acceptance Criteria
- discovery-index.json is valid JSON and reproducible.
- All listed facts include citations; unknowns are explicit with inspected paths.
- Multiple pipelines are represented as separate entries.

## Deliverable
- reverse_engineered_artifacts/discovery-index.json (primary artifact).
