# Notes

## kind

oh yah

## Kubectl

```bash
k get resource -o yaml

kubectl diff -f configs/
kubectl apply -f configs/
kubectl delete -f configs/

# https://kubernetes.io/docs/concepts/overview/working-with-objects/field-selectors/
kubectl get pods --field-selector ""
k delete pod -l app=mysql

# https://kubernetes.io/docs/concepts/architecture/nodes/
kubectl describe node <insert-node-name-here>

# https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
kubectl get deployments
kubectl rollout status deployment.v1.apps/nginx-deployment

# to forward ports (find a better way)
kubectl port-forward --namespace default svc/airflow-1584399113 8080:8080

# secrets
kubectl create secret generic mysql --from-literal=password=MYSECRETPASS
```

## Pod

- Atomic unit of kubernetes
- Lifecycle: `Pending` -> `Running` -> `Succeeded`; `Pending` -> `Failed`; `Unknown`

## Services

> When you create a Service, it creates a corresponding DNS entry. This entry is of the form <service-name>.<namespace-name>.svc.cluster.local, which means that if a container just uses <service-name>, it will resolve to the service which is local to a namespace. This is useful for using the same configuration across multiple namespaces such as Development, Staging and Production. If you want to reach across namespaces, you need to use the fully qualified domain name (FQDN).

- map service to endpoint to set up external services
- use `ExternalName` Service
- roundrobin IP addresses
- `ClusterIP` service lets you use names like mysql:3306

## Logging

[k8s docs](https://kubernetes.io/docs/concepts/cluster-administration/logging)

- send to `stderr` or `stdout`

### Ingress

https://www.youtube.com/watch?v=VicH6KojwCI

- customizable way to expose traffic than a cloud load balancer
- ingress object configures access to services from outside the cluster
- designed for `HTTP` and `HTTPS` services
- can provide SSL, name-based, and path-based routing

- controller routes traffic to service based on ingress definition
- usually fronted by a cloud load balancer
- consolidate routing through a single resource

## Database Migrations

https://cloudnativedevopsblog.com/posts/database-migrations/

- create a job and hook it up to helm to run before our deployment

## Secrets

- consumed as environment variables or volumes
- secrets are encoded, not encrypted by default

## ConfigMaps

- decouple configuration from image content
- created from files, directories, or literal values
- values can be referenced as environment variables
- can be mounted as a volume

## Deployment Patterns

- rolling updates -- gradually replace pods with an updated spec
- canary deployments -- deploy, send a small subset of traffic to canary
- blue-green deployments -- maintain two versions of your application deployment: switch traffic from `blue` to `green` by updating selector in service

## Helm

- use helm to update pods when config map changes
- create a simple functional test

## Best Practices

- [Recommended Labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/common-labels/)
- [ephemeral containers for debugging](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/)
- [daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) for logging
- [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) to replace crontab
- [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) to organize information about clusters, usres, namespaces, etc
- [metrics](https://kubernetes.io/docs/concepts/cluster-administration/monitoring/)
- [namespaces](https://kubernetes.io/docs/tasks/administer-cluster/namespaces-walkthrough/) to separate `production` and `staging`
- horizontal pod autoscaler
- Role-Based Access Control
- namespaces == define a virtual cluster to isolate resources for multiple teams or projects (`default` and `kube-system`)
- network policies (defining ingress and egress) using selectors
- [Operators](https://www.youtube.com/watch?v=TKpQNKWRWHo)
  - [kopf](https://github.com/zalando-incubator/kopf)
