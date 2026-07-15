# lemonade-stand

LLM guardrails demo with TrustyAI orchestration. Guardrailed chat assistant that only discusses lemons, with HAP detection, prompt injection detection, and language filtering.

## Prerequisites

Install these on your cluster before deploying:

- OpenShift AI operator installed (channel: fast)
- OpenShift Serverless operator installed
- Red Hat OpenShift Service Mesh operator installed
- DataScienceCluster configured with: kserve, dashboard, trustyai, modelmeshserving
- NVIDIA GPU Operator installed (channel: v24.9)
- Node Feature Discovery operator installed
- ClusterPolicy configured (mig_strategy: single)
- GPU node(s) available with nvidia.com/gpu resource

## Install

**Option A — values.yaml secrets** (CI / automated):

```bash
helm install lemonade-stand ./chart \
  --set secrets.vllmApiKey=<your-key> \
  --set secrets.minioAccessKey=admin \
  --set secrets.minioSecretKey=adminpassword \
  -n lemonade-stand --create-namespace
```

**Option B — create secrets out-of-band** (interactive / production):

```bash
oc new-project lemonade-stand
./scripts/create-secrets.sh
helm install lemonade-stand ./chart -n lemonade-stand
```

## Regenerate

This chart is generated from `spec.yaml` by quickpat:

```bash
quickpat compose spec.yaml --format qs
```
