# Pod

It's the atom unit of scheduling of Kubernetes. A pod it's the interface used to run containers, VMs, Wasm apps and more.

If it's wrapped in a Pod, Kubernetes can run it.

## Containers

The simplest configuration is a container per Pod, this is one of the main reasons why sometimes, the term Pod is used instead of the term container. Still, there are powerful
use cases for multi-container Pods.

- Helper services.
- Apps with tightly coupled helper functions such as log scrapers.

> [!NOTE]
> Often you will hear the term _sidecar_ which is a fancy word for referring to a helper container that runs in the same Pod as the main app container and provides
> additional services.

Multi-container Pods also help us to implement the single responsbility principle where every container performs a single task.

## Anatomy

Each Pod it's a shared execution environment for one or more containers. This execution environment includes:

- A network stack
- Volumes
- Shared memory
- More...

So, if we have multiple containers in the same Pod (a multi-container Pod) this will be abaliable to the outside world through the same IP address. If the containers inside the Pod
need to communicate with each other, they can use the `localhost`interface.

> [!NOTE]
> You should use multi-container Pods when your application has thightly coupled components needeing to share resources such as memory and storage.

## Lifecycle

Pods are created, live and die. Anytime a Pod dies, Kubernetes replaces it with a new one.

> [!NOTE]
> Although the new created Pod it's identically to the old one, it has a new ID and a new IP.

## Immutability

Pods are immutable. You never change them once they are running.

If you need to update a Pod, you **always replace** it with a new one that carries the update.

> [!IMPORTANT]
> When we talk about updating a Pod, we're ALWAYS referring to deleting the old one and starting a new one.

## Deployments

Even though Kubernetes works with Pods, you will almost always deploy them with a higher-level controller such as _Deployments, StatefulSets_ and _DaemonSets_.

Deployments provide really useful features to our Pods like self-healing, scaling, rolling updates, and versioned rollbacks to stateless apps.

## Services

Pods are managed by Controllers that can replace the Pods thus changing the Pod ID and IP address, this makes Pods unreliable.

Clients cannot make reliable connections to individual Pods as Kubernetes doesn't guarantee they will exists.

Services provide a reliable name and IP, and load balances requests to the Pods behind it.

## Process of deploying a Pod

1. Define the Pod in a YAML _mainfest file_.
2. Post the _manifest_ to the API server.
3. The request is authenticated and authorized.
4. The Pod spec is validated.
5. The scheduler filters nodes based on nodeSelectors, affinity and anti-affinity rules, topology spread constraints, resource requirements and limits, and more.
6. The Pod is assigned to a healthy node meeting all the requirements.
7. The kubelet on the node watches the API server and notices the Pod assignment.
8. The kubelet downloads the Pod spec and asks the local runtime to start it.
9. The kubelet monitors the Pod status and reports status changes to the API server.

> [!NOTE]
> Deploying a Pod is an **atomic operation**. This means a Pod only starts servicing requests when all its containers are running.

> [!NOTE]
> To see the documentation of the Pod spec you can use the command:
> `kubectl explain pod --recursive | more`
> Or if you want an explanation of a specific property (like it's posible values etc...) you can use the command:
> `kubectl explain pod.spec.<property>`

There are two ways to deploy Pods:

- Directly via a Pod manifest (rare).
- Indirectly via a workload resource and controller (most common).

Directly via Pod manifest means that we are deploying the Pod directly on the node, this means that the Pod it's managed only by the kubelet that has a limited set of actions
like starting the containers and stoping them. If the node fails, the kubelet fails with it and cannot do anything to help the Pod.

On the other hand, Pods deployed using workload resources, get all the benefits of being managed by a highly available controller that can restart them on other nodes, scale them
and perform advanced operations.

## The Pod network

Every cluster runs a _pod network_, all Pods are connected to it automatically. The network spans every cluster node and allows every Pod to talk every othe Pod even if the Pods are
running in different nodes of the cluster.

> [!NOTE]
> The Pod network is implemented as a _third party plugin_, you choose a network plugin at cluster build and it configures the Pod network for the entire cluster.

> [!IMPORTANT]
> Newly created clusters implement a very lax network security policy for simplicity. **You should use Kubernetes Network Policies and other measures to secure it.**

## Multi-container Pods Init Containers vs Sidecars

**Init containers**: It runs in the same Pod as the main app, but Kubernetes guarantees that this container will start and finish before the main application container. It will
be run only once. The purpose of init container is to prepare and initialize the environment for the main application container.

> [!INFO]
> If multiple init containers, Kubernetes runs them in the same order as they appear in the manifest. And starts the main application container once all init containers have finished.

**Sidecars containers**: It runs alongiside the main application container, it's job is to provide additional functionality without having to add it to the main application.

You define sidecars as init containers with the `restartPolicy`set to `Always`.
