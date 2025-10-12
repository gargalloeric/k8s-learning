# Intro to Kubernetes

Kubernetes is an open source orchestrator of containerized cloud-native microservices applications. An orchestrator is a system that deploys
and dynamically responds to changes.

Kubernetes can:

- Deploy applications.
- Scale them up and down based on demand.
- Self-heal them when things break.
- Perform rollouts and rollbacks.
- And more.

At it's core Kubernetes does two essential things:

1. Abstracts infrastructure.
2. Simplifies applications portability.

**Kubernetes is the de facto platform for cloud-native applications.**

## Kubernetes principles of operation

Kubernetes is both of the following:

- A cluster
- An orchestrator

### Cluster

A Kubernetes cluster is one or more nodes providing CPU, memory, and other resources for application use.

There are two types of nodes:

1. Control plane nodes
2. Worker nodes

> **Control plane nodes must be Linux servers**, but worker nodes can be Linux or Windows.

Control plane nodes implement the Kubernetes intelligence. Every cluster needs at least one. However, you should have three or five for
high availability (HA).

Every control plane node **runs every control plane service**.

Worker nodes are where you run your **business application**.

It's a production best practice to run all user apps on worker nodes, allowing control plane nodes to allocate their
resources to cluster-related operations.

**The major control plane services**

- API server: It's the frontend of Kubernetes and all commands and requests go through it. It exposes a RESTful API over HTTPS, and all requests are
subject to authentication and authorization.

- The cluster store: It's in charge of storing the desired state of all applications and cluster components. It's the only stateful part of the control plane.

- Controllers: Are used to implement a lot of the cluster intelligence. Each controller runs as a separate process in the control plane. The most common controllers are:

  - Deployment controller
  - StatefulSet controller
  - ReplicaSet controller

> Kubernetes also runs a controller manager that is responsible of spawning and managing the individual controllers.

- The scheduler: Watches the API server for new work tasks and distributes the work amongst the healthy worker nodes.

- The cloud controller manager: If the cluster is on a public cloud (AWS, Azure, GCP, etc...) it will run a _cloud controller manager_ that integrates the cluster with
cloud services, such as instances, load balancers and storage.

In summary, the **control plane** implements the brains of Kubernetes.

**The major worker node components**

- Kubelet: It's the main Kubernetes agent and handles all communications with the cluster.

> The kubelet perform the following tasks:
>
> - Watches the API server for new tasks.
> - Executes the appropiate runtime to execute tasks.
> - Reposts tasks status to the API server.

- Runtime: Every worker node has one or more runtimes (like docker, containerd, etc...) for executing tasks.

- Kube-proxy: Implements cluster networking and load balances traffic to tasks running on the node.

### Packaging apps for Kubernetes

Kubernetes runs a lot of things, but before it can run on Kubernetes, they first need to be wrapped in Pods.

Pods are a thin wrapper that abstracts different types of tasks so they can run on Kubernetes.

Pods are wrapped in higher-level controllers for advanced features like scaling, self-healing, rollouts, etc...

We can think about it like:

1. The Pod is the thin abstraction layer that allows Kubernetes to manage our containerized application.
2. The Pod are wrapped into another abstractions to add capabilities to the Pod.

Each layer of wrapping adds something.

### The desired state

It works on three principles:

- Desired state: It's what you want.

- Observed state: It's what you have.

- Reconciliation: Is the process of keeping observed state in sync with desired state.
