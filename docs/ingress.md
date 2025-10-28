# Ingress

Ingress is all about accessing multiple web applications through a single LoadBalancer Service.

LoadBalancer Services only provide a one-to-one mapping between internal Services and cloud load balancers. This means a cluster with 25 internet facing apps will need
25 cloud load balanacers. Ingress fixes this by letting you expose multiple Services through a single cloud load balancer.

Ingress it's defined in the networking.k8s.io/v1 API sub-group.

> [!IMPORTANT]
>
> Kubernetes doesn't have a builtin Ingress controller, meaning you need to install one.

You deploy Ingress resources with rules telling the controller how to route requests.

> [!IMPORTANT]
>
> Ingress operates at the layer 7 of the OSI model a.k.a the application layer. This means it can inspect HTTP headers
> and forward traffic based on hostname and paths.

## Ingress classes

Ingress classes allows you to run multiple Ingress controllers on a single cluster.

1. Map each Ingress controller to its own Ingress class.
2. When creating Ingress objects, assign them to an Ingress class.

> [!NOTE]
> 
> You install an Ingress controller, then you create Ingress objects which are list of rules governing how incomming traffic is routed to applications on the cluster.
