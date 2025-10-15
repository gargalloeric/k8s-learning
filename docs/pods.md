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

So, if we have multiple containers in the same Pod (a multi-container Pod) this will be avaliable to the outside world through the same IP address. If the containers inside the Pod
need to communicate with each other, they can use the `localhost`interface.

> [!NOTE]
> You should use multi-container Pods when your application has thightly coupled components needeing to share resources such as memory and storage.

## Lifecycle

Pods are created, live and die. Anytime a Pod dies, Kubernetes replaces it with a new one.

> [!NOTE]
> Although the new created Pod it's identically to the old one, it has a new ID and a new IP.

## Immutability

Pods are immutable. You never change them once they are running.

If you need to update a Pod, **you always replace it with a new one** that carries the update.

> [!IMPORTANT]
> When we talk about updating a Pod, we're ALWAYS referring to deleting the old one and starting a new one.

## Deployments

Even though Kubernetes works with Pods, you will almost always deploy them with a higher-level controller such as _Deployments, StatefulSets_ and _DaemonSets_.

Deployments provide really useful features to our Pods like self-healing, scaling, rolling updates, and versioned rollbacks to stateless apps.

## Services

Pods are managed by Controllers that can replace the Pods thus changing the Pod ID and IP address, this makes Pods unreliable.

Clients cannot make reliable connections to individual Pods as Kubernetes doesn't guarantee they will exists.

Services provide a reliable name and IP, and load balances requests to the Pods behind it.
