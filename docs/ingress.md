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

## Main Difference with a Service (Type LoadBalancer)

A Kubernetes Service of type LoadBalancer provisions an external cloud load balancer with its own external IP address. This load balancer forwards incoming traffic directly to the Pods backing that Service. Each LoadBalancer service results in a separate cloud load balancer resource being allocated (which may incur additional costs). It is suitable for exposing a single service externally with a dedicated IP.

An Ingress, on the other hand, is a higher-level Kubernetes resource that manages external HTTP/HTTPS access to multiple Services in a cluster. Instead of provisioning multiple cloud load balancers, typically only one external load balancer is provisioned for the Ingress Controller (itself usually exposed via a LoadBalancer service or NodePort). The Ingress Controller listens to Ingress resource rules and routes incoming requests based on hostnames, paths, or other rules to the internal cluster IP Services, which then route traffic to the Pods.

In summary, Services of type LoadBalancer expose individual services with dedicated cloud load balancers, while Ingress provides a single entry point with routing rules to direct traffic to multiple services inside the cluster. This makes Ingress more cost-effective and flexible for HTTP/HTTPS traffic routing.
