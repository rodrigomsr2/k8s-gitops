# ADR-001 — Repositório GitOps dedicado

**Status:** Aceito  
**Data:** 2026-04-22

## Contexto

O milestone `v0.5.0` do `k8s-homelab` introduz GitOps com ArgoCD. Uma
decisão fundamental é onde os manifests Kubernetes das aplicações devem
viver — no repositório da aplicação, no repositório de infraestrutura, ou
num repositório dedicado.

## Decisão

Repositório dedicado (`k8s-gitops`), separado tanto do repositório de
código-fonte das aplicações quanto do repositório de infraestrutura
(`k8s-homelab`).

Cada aplicação gerenciada pelo ArgoCD tem seu próprio diretório em
`apps/<nome>/`, com manifests Kubernetes puros. O ArgoCD monitora este
repositório e reconcilia o cluster a cada commit na branch `main`.

## Consequências aceitas

- Um terceiro repositório para manter. Aceitável dado que o escopo é
  claro e o overhead é baixo.
- O workflow de CI de cada aplicação precisa de permissão de escrita neste
  repositório para atualizar a tag da imagem após o build.
- Novos colaboradores precisam entender o fluxo de três repositórios antes
  de fazer um deploy.

## Alternativas rejeitadas

**Manifests no repositório da aplicação (ex: `nexus`).** Rejeitada porque
acopla o ciclo de release da aplicação aos manifests Kubernetes. Um commit
de feature e um commit de deploy ficam no mesmo histórico, dificultando
auditoria. O repositório de código-fonte não deveria ter conhecimento de
Kubernetes.

**Manifests em `k8s/apps/` dentro do `k8s-homelab`.** Rejeitada porque
mistura infraestrutura com configuração de aplicação. O `k8s-homelab`
gerencia o cluster em si — não as aplicações que rodam nele. Ciclos de
release distintos justificam repositórios distintos.
