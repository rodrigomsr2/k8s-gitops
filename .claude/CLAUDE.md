# k8s-gitops — Índice para IA

Repositório GitOps centralizado para o ambiente k8s-homelab. Contém os
manifests Kubernetes de todas as aplicações gerenciadas pelo ArgoCD.

> Para visão geral e fluxo de deploy: leia o `README.md`.

---

## Onde encontrar cada tipo de conhecimento

| Tipo | Onde |
|------|------|
| Visão geral, fluxo de deploy e onboarding | `README.md` |
| Decisões arquiteturais (por que X e não Y) | `docs/adr/` |
| Manifests de cada aplicação | `apps/<nome>/` |

---

## Aplicações gerenciadas

| Diretório | Serviço | Namespace |
|-----------|---------|-----------|
| `apps/nexus/nexus-argocd/` | nexus-argocd | `nexus` |

---

## ADRs vigentes

| ADR | Decisão |
|-----|---------|
| `docs/adr/ADR-001-gitops-repository.md` | Repositório GitOps dedicado, separado de infra e código-fonte |

---

## Convenções do projeto

- Cada aplicação tem seu próprio diretório em `apps/<nome>/`
- Cada aplicação roda em seu próprio namespace com o mesmo nome do diretório
- A tag da imagem é atualizada via commit automático pelo CI do repositório de origem
- Semver aplicado: cada tag representa um estado reconciliável do cluster
- Nenhum manifest é aplicado manualmente — tudo passa pelo ArgoCD
