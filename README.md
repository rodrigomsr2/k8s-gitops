# k8s-gitops

GitOps repository for the k8s-homelab environment. Centralizes Kubernetes manifests for all applications managed by ArgoCD. Versioned with semver — each tag represents a stable, reconcilable state of the cluster's desired configuration.

## Aplicações

| Aplicação | Namespace | Diretório | Imagem |
|-----------|-----------|-----------|--------|
| `nexus-argocd` | `nexus` | `apps/nexus/nexus-argocd/` | `ghcr.io/rodrigomsr2/nexus-argocd` |
| `monitoring-stack` | `monitoring` | `apps/monitoring/monitoring-stack/` | Prometheus, Grafana, Node Exporter |
| `loki` | `monitoring` | `apps/monitoring/loki/` | Helm wrapper chart com dependência `grafana/loki` |
| `promtail` | `monitoring` | `apps/monitoring/promtail/` | Helm wrapper chart com dependência `grafana/promtail` |

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
│   ├── argocd/
│   │   └── argocd-config/         # Ingress e ApplicationSet auto-managed
│   ├── monitoring/
│   │   ├── monitoring-stack/      # Prometheus, Grafana, Node Exporter
│   │   ├── loki/                  # Helm wrapper chart para grafana/loki
│   │   └── promtail/              # Helm wrapper chart para grafana/promtail
│   └── nexus/
│       └── nexus-argocd/          # Serviço nexus-argocd
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

1. Criar o diretório `apps/<namespace>/<nome-da-aplicacao>/`
2. Adicionar os manifests Kubernetes
3. Fazer commit e push na branch `main`
4. Atualizar este README e o CHANGELOG

O ApplicationSet `homelab` cria uma Application automaticamente para cada
diretório que segue o padrão `apps/*/*`.

Para apps baseadas em charts de terceiros, usar um wrapper chart com
`Chart.yaml`, `Chart.lock` e `values.yaml`. O diretório `charts/` gerado por
`helm dependency build` é local e fica ignorado.

## Documentação

| Documento | Descrição |
|-----------|-----------|
| `docs/adr/ADR-001-gitops-repository.md` | Decisão de criar um repositório GitOps dedicado |
| `.claude/CLAUDE.md` | Índice de navegação para agentes de IA |
