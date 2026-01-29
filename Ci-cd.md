# CI-CD (Jenkins / ArgoCD / Rancher)

## CLI Operations

| Command                               | Purpose             |
| ------------------------------------- | ------------------- |
| `jenkins-cli.jar help`                | Jenkins CLI help    |
| `java -jar jenkins-cli.jar list-jobs` | List jobs           |
| `java -jar jenkins-cli.jar build job` | Trigger build       |
| `argocd login server`                 | Login ArgoCD        |
| `argocd app list`                     | List apps           |
| `argocd app sync app`                 | Sync app            |
| `argocd app get app`                  | App status          |
| `argocd cluster list`                 | Registered clusters |
| `kubectl get applications -A`         | Argo apps CRD       |
| `rancher kubectl get clusters`        | Rancher clusters    |
| `rancher login`                       | Rancher auth        |
| `rancher projects`                    | List projects       |
| `rancher ps`                          | Rancher workloads   |
