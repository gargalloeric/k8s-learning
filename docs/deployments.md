# Deployments

Deployments are the most popular way of running stateless apps on Kubernetes. They add:

- Self-Healing
- Scaling
- Rollouts
- Rollbacks

> [!NOTE]
>
> Deployments follow standard Kubernetes architecture:
>
> 1. A resource
> 2. A controller
>
> Resources define objects and controllers manage them.

**Every Deployment manages one or more identical Pods.**


For instance, an application comprising a web service and a auth service will need two Deployments. One for managing the web Pods and one for managing
the auth Pods.

> [!IMPORTANT]
>
> Behind the scenes Deployments use a different resource to provide self-healing and scaling called _ReplicaSet_.
>
> However, you perform all management via the Deployment and never directly manage the ReplicaSet or Pods.


## Scaling

Kubernetes has several _autoscalers_ that automatically scale your apps and infrastructure. For instance:

- The Horizontal Pod Autoscaler (HPA): Adds and removes Pods to meet current demand.
- The Vertical Pod Autoscaler (VPA): Increases and decreases the CPU and memory allocated to run Pods to meet current demand.
- The Cluster Autoscaler (CA): Adds and removes cluster nodes so you always have enough to run all schedulled Pods.

> [!NOTE]
>
> Sometimes you will hear the word _multi-dimensional autoscaling_ which is a fancy word that makes reference to the combination of two autoscaling methods.
>
> - HPA + CA
> - HPA + VPA
> - ...
