# k8s-gitops

GitOps repository for the k8s-homelab environment. Centralizes Kubernetes manifests for all applications managed by ArgoCD. Versioned with semver — each tag represents a stable, reconcilable state of the cluster's desired configuration.

## Aplicações

| Aplicação | Namespace | Diretório | Imagem |
|-----------|-----------|-----------|--------|
| `nexus-argocd` | `nexus` | `apps/nexus/nexus-argocd/` | `ghcr.io/rodrigomsr2/nexus-argocd` |

## Estrutura do repositório

```
k8s-gitops/
├── README.md
├── CHANGELOG.md
├── .claude/
│   ├── CLAUDE.md                  # Índice de navegação para IA
│   └── skills/
│       ├── semver.md
│       └── project-organization.md
├── apps/
│   └── nexus/                     # Namespace nexus — serviços do monorepo nexus
│       └── nexus-argocd/          # Serviço nexus-argocd
│           ├── namespace.yaml
│           ├── deployment.yaml
│           ├── service.yaml
│           └── ingress.yaml
└── docs/
    └── adr/
        └── ADR-001-gitops-repository.md
```

## Como funciona

O ArgoCD, instalado no cluster pelo repositório
[k8s-homelab](https://github.com/rodrigomsr2/k8s-homelab), monitora este
repositório. Qualquer commit na branch `main` é detectado e reconciliado
automaticamente no cluster.

O ciclo completo de deploy:

```
push no repositório nexus (código-fonte)
        ↓
GitHub Actions — build + push da imagem para o GHCR
        ↓
GitHub Actions — commit neste repositório atualizando a tag da imagem
        ↓
ArgoCD detecta o commit e reconcilia o cluster
```

## Adicionando uma nova aplicação

1. Criar o diretório `apps/<nome-da-aplicacao>/`
2. Adicionar os manifests Kubernetes (Deployment, Service, Ingress)
3. Registrar a nova `Application` no ArgoCD (via `k8s-homelab`)
4. Atualizar este README e o CHANGELOG

## Documentação

| Documento | Descrição |
|-----------|-----------|
| `docs/adr/ADR-001-gitops-repository.md` | Decisão de criar um repositório GitOps dedicado |
| `.claude/CLAUDE.md` | Índice de navegação para agentes de IA |
