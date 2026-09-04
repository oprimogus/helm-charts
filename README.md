# Generic Application Helm Chart

Chart genérico para implantação de aplicações containerizadas no Kubernetes. Projetado para suportar desde workloads simples até microserviços com persistência, regras de agendamento e autoscaling.

---

## Estrutura do Repositório

```text
.
├── charts
│   └── generic-application
│       ├── Chart.yaml          # Metadados do Chart (versão, nome)
│       ├── values.yaml         # Valores padrão
│       └── templates/          # Manifestos Kubernetes
│           ├── _helpers.tpl    # Helpers de template Go
│           ├── configmap.yaml  # Mapeamento de variáveis de ambiente
│           ├── deployment.yaml # Deployment principal
│           ├── hpa.yaml        # Autoscaling (HPA)
│           ├── ingress.yaml    # Roteamento de Ingress
│           ├── pvc.yaml        # Persistence Volume Claims
│           └── service.yaml    # Exposição de portas
├── tests/                      # Casos de teste locais
│   └── minecraft.yaml
├── index.yaml                  # Índice do repositório Helm Pages
└── README.md
```

## values.yaml

| Parâmetro | Tipo | Padrão | Descrição |
| :--- | :--- | :--- | :--- |
| `name` | `string` | `"generic-application"` | Nome base da aplicação. |
| `environment` | `string` | `""` | Sufixo de ambiente (`prd`, `dev`, `infra`). Se omitido, o sufixo é removido dos nomes dos recursos. |
| `annotations` | `object` | `{}` | Annotations adicionais para os metadados do Deployment. |
| `image.repository` | `string` | `"hello-world"` | Imagem do container. |
| `image.tag` | `string` | `"latest"` | Tag/versão da imagem. |
| `image.pullPolicy` | `string` | `"IfNotPresent"` | Política de download da imagem (`Always`, `IfNotPresent`, `Never`). |
| `image.pullSecrets` | `list` | `[]` | Lista de Secrets para autenticação em registros privados. |
| `initContainers.enabled` | `boolean` | `false` | Habilita initContainers. |
| `initContainers.containers` | `list` | `[]` | Definição dos containers de inicialização em formato YAML. |
| `health.liveness.enabled` | `boolean` | `false` | Habilita a probe de Liveness. |
| `health.readiness.enabled` | `boolean` | `false` | Habilita a probe de Readiness. |
| `health.metrics.port` | `string` | `"server"` | Porta para raspagem de métricas (Prometheus). |
| `resources.min.cpu` | `string` | `"100m"` | Solicitação mínima de CPU (`requests.cpu`). |
| `resources.min.memory` | `string` | `"128Mi"` | Solicitação mínima de memória (`requests.memory`). |
| `resources.max.cpu` | `string` | `"200m"` | Limite máximo de CPU (`limits.cpu`). |
| `resources.max.memory` | `string` | `"256Mi"` | Limite máximo de memória (`limits.memory`). |
| `resources.autoscaling.enable` | `boolean` | `false` | Ativa o Horizontal Pod Autoscaler (HPA). |
| `service.enabled` | `boolean` | `false` | Cria o recurso Service. |
| `service.type` | `string` | `"ClusterIP"` | Tipo do serviço (`ClusterIP`, `NodePort`, `LoadBalancer`). |
| `service.ports` | `list` | `[]` | Lista de portas expostas pelo Service. |
| `ingress.enabled` | `boolean` | `false` | Ativa o Ingress para roteamento externo. |
| `ingress.host` | `string` | `"app.example.com"` | Hostname do Ingress. |
| `ingress.ingressClassName` | `string` | `"traefik"` | Ingress Controller utilizado (`traefik`, `nginx`). |
| `env` | `object` | `{}` | Chave-valor convertidos automaticamente para o ConfigMap da aplicação. |
| `secrets` | `list` | `[]` | Mapeamento de Secrets existentes para variáveis de ambiente ou montagem de arquivos. |
| `persistence.enabled` | `boolean` | `false` | Ativa o suporte a armazenamento persistente. |
| `persistence.existingClaim` | `string` | `""` | Nome de um PVC já existente no cluster (ignora a criação do `pvc.yaml`). |
| `persistence.volumes` | `list` | `[]` | Lista de volumes persistentes a serem montados. |
| `scheduling.enabled` | `boolean` | `false` | Ativa regras de agendamento nos nós. |
| `scheduling.antiAffinity` | `boolean` | `true` | Evita que Pods da mesma aplicação sejam agendados no mesmo nó físico. |

## Fluxo de Desenvolvimento e Atualização do Chart

Siga estas etapas sempre que precisar alterar templates ou adicionar novos parâmetros ao `values.yaml`.

### 1. Atualizar o `Chart.yaml`
Sempre incremente a versão no `charts/generic-application/Chart.yaml` respeitando o SemVer:

```yaml
version: 0.1.2 # Incremente esta versão
```

## 2. Validar a Sintaxe (Lint)

```bash
helm lint charts/generic-application
```

### 3. Testar a Renderização dos Templates

```bash
# Teste usando o arquivo local de testes
helm template generic-application ./charts/generic-application -f ./tests/minecraft.yaml --debug

# Teste com valores de um repositório GitOps externo
helm template generic-application ./charts/generic-application -f ../flyfood-gitops/k8s/apps/flyfood-api/dev-values.yaml --debug
```

### 4. Empacotar o Chart

```bash
helm package ./charts/generic-application
```

### 5. Atualizar o Index

```bash
helm repo index . --url https://oprimogus.github.io/helm-charts
```

### 6. Deploy

Faça commits e push para o repositório para atualizar o index e os charts disponíveis.