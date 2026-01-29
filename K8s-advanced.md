# K8S-ADVANCED (K3s)

## Internals / Node Failures / CNI / Ingress

| Command                                          | Purpose                 |
| ------------------------------------------------ | ----------------------- |
| `kubectl get componentstatuses`                  | Control plane health    |
| `kubectl get events -A --sort-by=.lastTimestamp` | Cluster events          |
| `kubectl describe node`                          | Node diagnostics        |
| `kubectl cordon node`                            | Mark node unschedulable |
| `kubectl drain node --ignore-daemonsets`         | Safe node drain         |
| `kubectl uncordon node`                          | Re-enable scheduling    |
| `kubectl get pods -o wide`                       | Pod-node mapping        |
| `kubectl exec -it p -- ip a`                     | Pod networking          |
| `kubectl get cm -A`                              | ConfigMaps              |
| `kubectl get ingress -A`                         | Ingress resources       |
| `kubectl describe ingress ing`                   | Ingress debug           |
| `kubectl top node`                               | Node resources          |
| `crictl ps`                                      | Container runtime state |
| `crictl logs id`                                 | Runtime logs            |
