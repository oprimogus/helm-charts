# OprimoGus Helm Charts Repository

Helm Charts repository for Kubernetes infrastructure components and applications.

## 🚀 Charts Repository

To use these charts in your cluster, add the Helm repository:

```bash
helm repo add oprimogus https://oprimogus.github.io/helm-charts
helm repo update
```

---

## 📦 Available Charts

| Chart | Current Version | Description | Documentation |
| :--- | :--- | :--- | :--- |
| **generic-application** | `0.1.1` | Generic chart for deploying containerized applications on Kubernetes. | [Documentation](./charts/generic-application/README.md) |

## 🛠️ Repository Structure

```text
.
├── charts/
│   ├── generic-application/       # Generic chart for workloads
│   │   ├── Chart.yaml
│   │   ├── README.md              # Chart-specific documentation
│   │   ├── templates/
│   │   └── values.yaml
│   └── <other-chart>/             # Future charts (e.g., database-application)
├── tests/                         # Test cases and staging scenarios
│   └── minecraft.yaml
├── index.yaml                     # Helm repository index
└── README.md                      # Repository overview
```

## 🔄 Chart Development and Release Workflow

Follow these steps to modify templates, add parameters, or create new charts in the repository.

### 1. Bump the Chart version
Update the version field in the `Chart.yaml` file of the modified chart (e.g., `charts/generic-application/Chart.yaml`):

```yaml
version: 0.1.2 # Bump following SemVer
```

### 2. Validate syntax (Lint)

```bash
helm lint ./charts/generic-application
```

### 3. Test template rendering

```bash
helm template generic-application ./charts/generic-application -f ./tests/minecraft.yaml --debug
```

### 4. Package the Chart

Run the command at the root of the repository to generate the `.tgz` archive:

```bash
helm package ./charts/generic-application
```

### 5. Update the repository index

```bash
helm repo index . --url https://oprimogus.github.io/helm-charts
```

### 6. Publish changes

```bash
git add .
git commit -m "feat(generic-application): new feature and bump to v0.1.2"
git push origin main
```