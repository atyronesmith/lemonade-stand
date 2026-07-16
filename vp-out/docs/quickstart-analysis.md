# lemonade-stand

LLM guardrails demo with TrustyAI orchestration. Guardrailed chat assistant that only discusses lemons, with HAP detection, prompt injection detection, and language filtering.

- **Version:** 0.1.0
- **Source:** ``

## Architecture

## Required OpenShift Operators

The following operators are automatically installed by the Validated Pattern:

| Operator | Subscription | Channel | Source |
|----------|-------------|---------|--------|
| Node Feature Discovery | nfd | stable | redhat-operators |
| OpenShift Service Mesh | servicemeshoperator | stable | redhat-operators |
| Red Hat OpenShift AI | rhods-operator | fast | redhat-operators |
| NVIDIA GPU Operator | gpu-operator-certified | v24.9 | certified-operators |
| OpenShift Serverless | serverless-operator | stable | redhat-operators |

## Secrets Configuration

The following secrets were detected and should be configured before deployment:

| Secret | Values Path | Action |
|--------|-------------|--------|
| `llm-vllm-api-key` | `lemonade-stand` | Set via Vault or values |
| `model-storage-access-key` | `lemonade-stand/minio` | Set via Vault or values |
| `model-storage-secret-key` | `lemonade-stand/minio` | Set via Vault or values |

## Framework Architecture

This pattern uses the **multisource configuration** approach. Infrastructure Helm charts (clustergroup, vault, external-secrets) are pulled dynamically from the upstream Validated Patterns registry rather than stored locally. This means:

- No fork of multicloud-gitops required
- Upstream bug fixes are received by bumping `clusterGroupChartVersion`
- No `common/` git subtree needed (modern patterns use Ansible collections in the utility container)

The `pattern.sh` script runs all make targets inside a podman-based utility container (`quay.io/validatedpatterns/utility-container`) which includes the `rhvp.cluster_utils` Ansible collection and all required tooling.

> **Note:** The multisource feature is not yet documented on validatedpatterns.io but is used by all current production patterns (multicloud-gitops, rag-llm-gitops) and documented in the [common repo README](https://github.com/validatedpatterns/common).

## Pattern Configuration

- **Pattern name:** lemonade-stand
- **Application name:** lemonade-stand
- **Namespace:** lemonade-stand
- **Chart strategy:** remote
- **Vault enabled:** True

## Deployment

```bash
git init && git add -A && git commit -m "Initial pattern"
oc login <cluster>
./pattern.sh make install
```
