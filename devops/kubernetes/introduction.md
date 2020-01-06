# An Introduction to Kubernetes

[DO Article](https://www.digitalocean.com/community/tutorials/an-introduction-to-kubernetes)

Kubernetes, at its basic level, is a system for running and coordinating containerized applications across a cluster of machines. It is a platform designed to completely manage the life cycle of containerized applications and services using methods that provide predictability, scalability, and high availability.

## Kubernetes Architecture

At its base, Kubernetes brings together individual physical or virtual machines into a cluster using a shared network to communicate between each server. This cluster is the physical platform where all Kubernetes components, capabilities, and workloads are configured.

### Overview

- k8s runs applications and services in containers
- underlying components make sure the desired state of the applications matches the actual state of the cluster
- to start an app or service, submit a declarative plan in JSON or YAML

### Master Server

One server is the master server. This server acts as a gateway and brain for the cluster by exposing an API for users and clients, health checking other servers, deciding how best to split up and assign work (known as “scheduling”), and orchestrating communication between other components. The master server acts as the primary point of contact with the cluster and is responsible for most of the centralized logic Kubernetes provides.

> components on the master server work together to accept user requests, determine the best ways to schedule workload containers, authenticate clients and nodes, adjust cluster-wide networking, and manage scaling and health checking responsibilities.

**etcd** -- distributed key-value store. K8s stores configuration data here
**kube-apiserver** - main management point of the entire cluster as it allows a user to configure Kubernetes’ workloads and organizational units
**kubectl** - client that can interact with the k8s cluster from a local computer
**kube-controller-manager** - manages different controllers that regulate the state of the cluster, manage workload life cycles, and perform routine tasks. Watches `etcd` for changes
**kube-scheduler** - process to assign workloads to specific nodes in the cluster
**cloud-controller-manager** - interacts with various infrastructure providers to understand and manage the state of resources in the cluster

### Nodes

Responsible for accepting and running workloads using local and external resources. Receives work instructions from master server and does whatever it needs to to route and forward traffic appropriately

**container runtime** - responsible for starting and managing containers
**kubelet** - esponsible for relaying information to and from the control plane services, as well as interacting with the `etcd` store to read configuration details or write new values. Recieves manifest and assumes responsibility for maintaining the state of the work on the node server
**manifest** - defines workload and operating parameters
**kube-proxy** - manage individual host subnetting and makes services available to other components. The process forwards requests to the correct containers, can do primitive load balancing, and is generally responsible for making sure the networking environment is predictable and accessible, but isolated where appropriate

Worker nodes are the servers where your workloads (i.e. containerized applications and services) will run. A worker will continue to run your workload once they’re assigned to it, even if the master goes down once scheduling is complete

### Kubernetes Objects and Workloads

Kubernetes uses additional layers of abstraction over the container interface to provide scaling, resiliency, and life cycle management features. Instead of managing containers directly, users define and interact with instances composed of various primitives provided by the Kubernetes object model.

#### Pods

Most basic unit that Kubernetes deals with.

A pod generally represents one or more containers that should be controlled as a single application. Pods consist of containers that operate closely together, share a life cycle, and should always be scheduled on the same node. They are managed entirely as a unit and share their environment, volumes, and IP space. In spite of their containerized implementation, you should generally think of pods as a single, monolithic application to best conceptualize how the cluster will manage the pod’s resources and scheduling.

Horizontal scaling is generally discouraged on the pod level because there are other higher level objects more suited for the task. Users are encouraged to work with higher level objects that use pods or pod templates as base components but implement additional functionality.

#### Replication Controllers and Replication Sets

Often, when working with Kubernetes, rather than working with single pods, you will instead be managing groups of identical, replicated pods. These are created from pod templates and can be horizontally scaled by controllers known as replication controllers and replication sets.

**replication controller** - object that defines a pod template and control parameters to scale identical replicas of a pod horizontally by increasing or decreasing the number of running copies. Responsible for ensuring that the number of pods deployed in the cluster matches the number of pods in its configuration. Can also perform rolling updates to roll over a set of pods to a new version one by one, minimizing the impact on application availability.

**replication sets** - An iteration on the replication controller design with greater flexibility in how the controller identifies the pods it is meant to manage. Replication sets are beginning to replace replication controllers because of their greater replica selection capabilities, but they are not able to do rolling updates to cycle backends to a new version like replication controllers can. Instead, replication sets are meant to be used inside of additional, higher level units that provide that functionality

#### Deployments

**deployments** - use replication sets as a building block, adding flexible life cycle management functionality to the mix. Deployments are a high level object designed to ease the life cycle management of replicated pods

#### Stateful Sets

**stateful sets** - specialized pod controllers that offer ordering and uniqueness guarantees. Primarily, these are used to have more fine-grained control when you have special requirements related to deployment ordering, persistent data, or stable networking. For instance, stateful sets are often associated with data-oriented applications, like databases, which need access to the same volumes even if rescheduled to a new node.

Stateful sets provide a stable networking identifier by creating a unique, number-based name for each pod that will persist even if the pod needs to be moved to another node. Likewise, persistent storage volumes can be transferred with a pod when rescheduling is necessary. The volumes persist even after the pod has been deleted to prevent accidental data loss.

When deploying or adjusting scale, stateful sets perform operations according to the numbered identifier in their name. This gives greater predictability and control over the order of execution, which can be useful in some cases.

#### Daemon Sets

**daemon sets** - specialized form of pod controller that run a copy of a pod on each node in the cluster (or a subset, if specified). This is most often useful when deploying pods that help perform maintenance and provide services for the nodes themselves

For instance, collecting and forwarding logs, aggregating metrics, and running services that increase the capabilities of the node itself are popular candidates for daemon sets. Because daemon sets often provide fundamental services and are needed throughout the fleet, they can bypass pod scheduling restrictions that prevent other controllers from assigning pods to certain hosts. As an example, because of its unique responsibilities, the master server is frequently configured to be unavailable for normal pod scheduling, but daemon sets have the ability to override the restriction on a pod-by-pod basis to make sure essential services are running.

#### Jobs and CRON jobs

**jobs** provide a more task-based workflow where the running containers are expected to exit successfully after some time once they have completed their work. Jobs are useful if you need to perform one-off or batch processing instead of running a continuous service

**cron jobs** provide an interface to run jobs with a scheduling component. Cron jobs can be used to schedule a job to execute in the future or on a regular, reoccurring basis. Kubernetes cron jobs are basically a reimplementation of the classic cron behavior, using the cluster as a platform instead of a single operating system

### Other Kubernetes Components

#### Services

**service** is a component that acts as a basic internal load balancer and ambassador for pods. A service groups together logical collections of pods that perform the same function to present them as a single entity

Any time you need to provide access to one or more pods to another application or to external consumers, you should configure a service. For instance, if you have a set of pods running web servers that should be accessible from the internet, a service will provide the necessary abstraction. Likewise, if your web servers need to store and retrieve data, you would want to configure an internal service to give them access to your database pods.

Although services, by default, are only available using an internally routable IP address, they can be made available outside of the cluster by choosing one of several strategies.

**NodePort** works by opening a static port on each node’s external networking interface. Traffic to the external port will be routed automatically to the appropriate pods using an internal cluster IP service.

**LoadBalancer** service type creates an external load balancer to route to the service using a cloud provider’s Kubernetes load balancer integration. The cloud controller manager will create the appropriate resource and configure it using the internal service service addresses

#### Volumes and Persistent Volumes

Kubernetes uses its own volumes abstraction that allows data to be shared by all containers within a pod and remain available until the pod is terminated. This means that tightly coupled pods can easily share files without complex external mechanisms. Container failures within the pod will not affect access to the shared files. Once the pod is terminated, the shared volume is destroyed, so it is not a good solution for truly persistent data.

**Persistent volumes** are a mechanism for abstracting more robust storage that is not tied to the pod life cycle. Instead, they allow administrators to configure storage resources for the cluster that users can request and claim for the pods they are running. Once a pod is done with a persistent volume, the volume’s reclamation policy determines whether the volume is kept around until manually deleted or removed along with the data immediately. Persistent data can be used to guard against node-based failures and to allocate greater amounts of storage than is available locally.

#### Labels and Annotations

**label** - semantic tag that can be attached to Kubernetes objects to mark them as a part of a group. These can then be selected for when targeting different instances for management or routing. For instance, each of the controller-based objects use labels to identify the pods that they should operate on. Services use labels to understand the backend pods they should route requests to
