# Services

Pods are unreliable, and you should never connect to them directly. **You should always connecto to them via a Service.**

Service objects sit in front of one or more identical Pods and expose them via a reliable DNS name, IP address, and port.

Services use _labels_ and _selectors_ to know which Pods to send traffic to.

> [!NOTE]
>
> The Pod has to have all the labels specified in the Service selector in order for the Service to send traffic to the Pod. If
> the Pod doesn't have all the labels, the Service won't send traffic to the Pod. It doesn't matter if the Pod has more labels that the ones defined by the Service.

## Service types

Kubernetes has several types of Services for different use cases and requirements. The mian ones are:

- ClusterIP: Is the most basic and provides a reliable endpoint on the internal Pod network.
- NodePort: Builds on top of ClusterIP and allow external clients to connect via a port on every cluster node.
- LoadBalancer: Builds on top of previous two and integrates with cloud load balancers for extremely simple access from the internet.

### ClusterIP

It's the default Service type. These get a DNS name and IP address that programmed into the internal network and are **only accesible from inside the cluster**.

- The IP is only routable on the internal Pod network.
- The name is automatically registered with the cluster's internal DNS.
- All containers are pre-programmed to use the cluster DNS to resolve names.

### NodePort

Builds on top of ClusterIP by adding a dedicated port on every cluster node that external clients can use. This decicated port it's called _NodePort_.

NodePort Services have two significant limtations:

- They use high-numbered ports between 30000-32767.
- The clients need to know the name or IPs of nodes, as well as whether nodes are healthy.

### LoadBalancer

LoadBalancer Services are the easisest way of expose services to external clients. They simplify NodePort Services by putting a cloud load balancer in front of them.



ClusterIP Services are the default and provide reliable endpoints on the internal cluster network. NodePorts and LoadBalancers provide external endpoints.

LoadBalancers create a load balancer on the underlying cloud platform, as well as all the constructs and mappings to forward traffic from the load balancer to the Pods.
