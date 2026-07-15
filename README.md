# lemonade-stand

Application definition for the lemonade-stand guardrails demo — a FastAPI chat
assistant that only discusses lemons, with HAP detection, prompt injection detection,
and language filtering via TrustyAI GuardrailsOrchestrator.

## What's here

| Path | What it is |
|---|---|
| `spec.yaml` | ApplicationSpec — the source of truth for this application |
| `charts/lemonade-stand-app/` | FastAPI chat frontend |
| `charts/chunker-service/` | Text chunking microservice |
| `charts/lingua-detector/` | Language detection (English-only filter) |
| `charts/shiny-dashboard/` | R Shiny monitoring dashboard |
| `charts/guardrails-config/` | TrustyAI auxiliary image references ConfigMap |

## Generating output

Install [quickpat](https://github.com/your-org/quickpat), then:

```bash
# Generate a Validated Pattern
quickpat compose spec.yaml --output ../lemonade-stand-vp

# Generate a Quickstart (coming soon)
quickpat compose spec.yaml --output-qs ../lemonade-stand-qs
```

The generated output is not committed here. `spec.yaml` and `charts/` are
the source of truth — regenerate whenever you need a fresh VP or QS.

## Architecture

Built on 8 composable building blocks:

- **ai-platform-foundation** — OpenShift AI + Serverless + Service Mesh
- **gpu-compute** — NFD + NVIDIA GPU Operator
- **model-serving** × 3 — primary LLM (vLLM), HAP detector, prompt injection detector
- **object-storage** — MinIO for detector model weights
- **guardrails-orchestrator** — TrustyAI GuardrailsOrchestrator

Custom components (this repo):

- **lemonade-stand-app** — FastAPI frontend
- **chunker-service** — sentence chunker for guardrails pipeline
- **lingua-detector** — language filter
- **shiny-dashboard** — metrics dashboard
