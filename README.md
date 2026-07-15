# lemonade-stand

This repository is an application definition, not just a demo. It contains a single 70-line YAML file — `spec.yaml` — that fully describes a production-grade AI guardrails application. From it, one command generates either a deployable Helm chart or a complete Validated Pattern ready for ArgoCD.

The point is not the lemon chatbot. The point is that five days of partner engineering can be replaced by a spec file and a command.

---

## The commands

```bash
# Clone the repo, install quickpat, then:

# Validated Pattern (GitOps / ArgoCD)
quickpat compose spec.yaml

# Quickstart Helm chart (direct install)
quickpat compose spec.yaml --format qs
```

That's it. Same spec. Two deployment targets. `vp-out/` and `qs-out/` are committed alongside the source so ArgoCD has something to watch and partners have something to `helm install`.

---

## What's here

| Path | Role |
|---|---|
| `spec.yaml` | Source of truth — edit this, never the generated output |
| `charts/` | Hand-written Helm templates for the 5 custom components |
| `vp-out/` | Generated Validated Pattern — ArgoCD target, committed |
| `qs-out/` | Generated Quickstart Helm chart — `helm install` target, committed |

When the spec changes, regenerate with the commands above. Everything in `vp-out/` and `qs-out/` is derived — `spec.yaml` and `charts/` are the only things you maintain.

---

## What lemonade-stand actually is

A TrustyAI guardrails demo — a FastAPI chat assistant restricted to lemon-related topics, with HAP (hate/abuse/profanity) detection, prompt injection detection, and English-only language filtering routed through a GuardrailsOrchestrator. Three InferenceService instances, one MinIO deployment for detector model weights, one GuardrailsOrchestrator with two custom detectors and a language filter.

Under the hood, six building blocks from the shared catalog:

| Block | What it provides |
|---|---|
| `ai-platform-foundation` | OpenShift AI + Serverless + Service Mesh — mandatory for any AI workload on OCP |
| `gpu-compute` | NFD + NVIDIA GPU Operator + ClusterPolicy |
| `model-serving` (×3) | KServe InferenceService + ServingRuntime: vLLM for the LLM, HuggingFace runtime for HAP and prompt injection detectors |
| `object-storage` | MinIO for detector model weights, with RHOAI data connection |
| `guardrails-orchestrator` | TrustyAI GuardrailsOrchestrator routing chat through the detector chain |

Five custom components live in `charts/` because they are genuinely custom — the FastAPI frontend, the Shiny monitoring dashboard, the text chunker, the language detector, and the TrustyAI auxiliary image config. Everything else is configuration applied to shared building blocks.

---

## Why the numbers matter

The lemonade-stand quickstart — roughly 300 lines of handcrafted YAML across nine Kubernetes resource types — is fully described by a 70-line `ApplicationSpec`. Running `quickpat compose` against that spec produces an artifact set that matches Drew Minnear's hand-crafted Validated Pattern at 33 of 33 comparison points. The same spec also produces a self-contained Helm chart that deploys identically without any GitOps infrastructure.

A recent partner engagement took five days and sixteen people on-site to produce one Validated Pattern. That is not a criticism of how the team executed — they moved fast and delivered. It reflects the reality that without shared infrastructure, every conversion starts from scratch. The composition spec eliminates that. A partner defines the spec once; the pipeline handles the Helm charts, the ArgoCD applications, the ExternalSecrets, the operator subscriptions. The VP team reviews the output rather than authoring it from scratch.

Hardware partners scaling to hundreds of units are currently doing QS-to-VP conversions by hand. At that scale, the manual approach does not hold.

---

## The broader context

This repo is the reference implementation for the AI Application Building Blocks proposal — a structured supply chain from composition spec through CI-tested Validated Pattern through partner-certified hardware profile to customer deployment.

- **Compiler**: [quickpat](https://github.com/atyronesmith/quickpat)
