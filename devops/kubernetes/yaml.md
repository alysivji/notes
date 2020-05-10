# YAML Specification

https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/

> Every Kubernetes object includes two nested object fields that govern the object’s configuration: the object spec and the object status. The spec, which you must provide, describes your desired state for the object–the characteristics that you want the object to have. The status describes the actual state of the object, and is supplied and updated by the Kubernetes system. At any given time, the Kubernetes Control Plane actively manages an object’s actual state to match the desired state you supplied.

- apiVersion - Which version of the Kubernetes API you’re using to create this object
- kind - What kind of object you want to create
- metadata - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
- spec - What state you desire for the object

Use the API reference to find out the `spec` for each format https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.17/

## Links

- [writing a deployment spec](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#writing-a-deployment-spec)
