# Deep Dive

## Services

### Load Balancer

- don't want to use the cloud native load balancer
  - how can i do it via DNS configuration?
- NodePort?

### Service Network

- kube-proxy `IPVS mode`

## RBAC and Admission Control

- `ClusterRole` and `RoleBinding`
- `kubectl config set-credentials --client-certificate --client-key`
- PodSecurityPolicy

### Jobs

- jobs
- cronjobs

### Autoscaling

- Pod resoruce requriests and limits
- ResourceQuotas

### CustomResourceDefinition

- create a new kind

## Servicee Messhes

- Severless: OpenFAAS, KNative
- Service Meshes: Istio
- Prometheus
- Security
