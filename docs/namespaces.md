# Namespaces

Namespaces are a way to divide a Kubernetes cluster into multiple virtual clusters.

Namespaces are a form of _soft isolation_ and enable soft _multi-tenancy_. For example you can create namspaces for different environments like **dev**, **test**, **prod**, etc...
and apply different configurations to each.

> [!CAUTION]
>
> A compromised workload in one namespace can easily impact workloads in other namespaces.

> [!NOTE]
>
> We have to be aware that not all objects can be namespaced. For instance, `Nodes` and `PersistentVolumes` are cluster-scoped and you cannot
> deploy them to a specific namespace.
>
> You can check what resources can be namespaced using the following command:
>
> ```bash
> kubectl api-resources
> ```

Unless specify otherwise, Kubernetes deploys objects to the **default** namespace.

Namespaces are a way for multiple tenants to share the same cluster.

_Tenant_ is a loose term that can refer to individual applications, different teams or departments, and even external customers.

It's most common to use namespaces to divide the cluster for use by tenants within the same organization. For example, dividing the production cluster into the following
namespaces to match your organizational structure:

- finance
- hr
- corporate-ops

You'd deploy the finance apps to the finance namespace, hr apps to the hr namespace and corporate apps to the corporate-ops namespace. Each namespace can have it's own users,
permissions, resources quotas, permissions, etc...

> [!IMPORTANT]
>
> If you want hard multi-tenancy and isolation, you should implement different clusters on different hardware. So compromised workloads in one cluster doesn't affect the other.

## Kubernetes default namespaces

1. default: Is where new objects go if you don't specify a namespace when creating the objects.
2. kube-system: Is where control plane components such as the internal DNS service run.
3. kube-public: Is for objects that need to be readable by anyone.
4. kube-node-lease: Is used for node heartbeats and managing node leases.
