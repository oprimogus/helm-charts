# Generic Application Helm Chart

A generic Helm Chart for deploying containerized applications on Kubernetes. Designed to support everything from simple workloads to microservices with persistent storage, scheduling rules, and autoscaling.

---

## 🛠️ Chart Structure

```text
charts/generic-application/
├── Chart.yaml          # Chart metadata (version, name)
├── values.yaml         # Default configuration values
├── README.md           # Chart documentation
└── templates/          # Kubernetes manifest templates
    ├── _helpers.tpl    # Go template helper functions
    ├── configmap.yaml  # Environment variable mapping
    ├── deployment.yaml # Main Deployment manifest
    ├── hpa.yaml        # Horizontal Pod Autoscaling (HPA)
    ├── ingress.yaml    # Ingress routing configuration
    ├── pvc.yaml        # Persistent Volume Claims
    └── service.yaml    # Service exposition
```

---

## ⚙️ Configuration (`values.yaml`)

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `name` | `string` | `"generic-application"` | Base name of the application. |
| `environment` | `string` | `""` | Environment suffix (`prd`, `dev`, `infra`). If omitted, the suffix is stripped from resource names. |
| `annotations` | `object` | `{}` | Additional annotations to add to the Deployment metadata. |
| `image.repository` | `string` | `"hello-world"` | Container image repository. |
| `image.tag` | `string` | `"latest"` | Container image tag/version. |
| `image.pullPolicy` | `string` | `"IfNotPresent"` | Image pull policy (`Always`, `IfNotPresent`, `Never`). |
| `image.pullSecrets` | `list` | `[]` | List of Secret names for authenticating with private registries. |
| `initContainers.enabled` | `boolean` | `false` | Enables initContainers. |
| `initContainers.containers` | `list` | `[]` | Definition of initialization containers in YAML format. |
| `health.liveness.enabled` | `boolean` | `false` | Enables Liveness probe. |
| `health.readiness.enabled` | `boolean` | `false` | Enables Readiness probe. |
| `health.metrics.port` | `string` | `"server"` | Port used for Prometheus metrics scraping. |
| `resources.min.cpu` | `string` | `"100m"` | Minimum requested CPU (`requests.cpu`). |
| `resources.min.memory` | `string` | `"128Mi"` | Minimum requested memory (`requests.memory`). |
| `resources.max.cpu` | `string` | `"200m"` | Maximum allowed CPU limit (`limits.cpu`). |
| `resources.max.memory` | `string` | `"256Mi"` | Maximum allowed memory limit (`limits.memory`). |
| `resources.autoscaling.enable` | `boolean` | `false` | Enables the Horizontal Pod Autoscaler (HPA). |
| `service.enabled` | `boolean` | `false` | Creates the Service resource. |
| `service.type` | `string` | `"ClusterIP"` | Service type (`ClusterIP`, `NodePort`, `LoadBalancer`). |
| `service.ports` | `list` | `[]` | List of ports exposed by the Service. |
| `ingress.enabled` | `boolean` | `false` | Enables Ingress resource for external routing. |
| `ingress.host` | `string` | `"app.example.com"` | Ingress routing hostname. |
| `ingress.ingressClassName` | `string` | `"traefik"` | Ingress Controller class (`traefik`, `nginx`). |
| `env` | `object` | `{}` | Key-value pairs automatically injected into the application's ConfigMap. |
| `secrets` | `list` | `[]` | Mapping of existing Secrets to environment variables or file mounts. |
| `persistence.enabled` | `boolean` | `false` | Enables persistent storage support. |
| `persistence.existingClaim` | `string` | `""` | Name of a pre-existing PVC in the cluster (skips creating `pvc.yaml`). |
| `persistence.volumes` | `list` | `[]` | List of persistent volumes to mount. |
| `scheduling.enabled` | `boolean` | `false` | Enables node scheduling rules. |
| `scheduling.antiAffinity` | `boolean` | `true` | Prevents Pods of the same application from being scheduled on the same physical node. |

---
