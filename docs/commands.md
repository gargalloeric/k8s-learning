# Commands

## Apply a manifest

This command sends the yml file to the API server defined in the current context of your _kubeconfig_ file.

```bash
kubectl apply -f file.yml
```

> [!NOTE]
>
> It also authenticates the request using the credentials defined in the _kubeconfig_ file.

## Get Pods

```bash
kubectl get pods
```

This command has some useful options like:

1. **-o wide** gives a few more columns to the output, like the node where the Pod is being executed.
2. **-o yaml** give you everything Kubernetes knows about the object.
3. **--watch** to get updates in real time of the pods.

> [!NOTE]
>
> The output of __-o yaml__ has two main sections, spec and status. One shows the desire state and the second one the observed state.

## Describe Pods

```bash
kubectl describe pod <pod>
```

Provides a nicely formatted overview of an object, including lifecycle events.

## Pod logs

```bash
kubectl logs <pod>
```

In a multi-container Pod, this command will show the logs from the first container in the Pod. This
behaviour can be overwritten by using the flag `--container <container-name>`.

> [!NOTE]
>
> You can obtain the order of the containers in the Pod and their names using the describe command.

## Executing commands in Pods

This command can be used in two diffrent ways:

1. Remote command execution: sends the command to the container from your local shell. The container executes the commad and returns the output to your shell.
2. Execution sessions: connect your local shell to the container's shell. It's like being logged onto the container.

```bash
kubectl exec <pod> -- <command>
```

```bash
# Interactive shell
kubectl exec -it <pod> -- sh
```

In a multi-container Pod, this command will show the logs from the first container in the Pod. This
behaviour can be overwritten by using the flag `--container <container-name>`.

> [!NOTE]
>
> You can obtain the order of the containers in the Pod and their names using the describe command.

## Get Services

```bash
kubectl get services
```

## Get a specific service

You would run the get services command to list all services and then:

```bash
kubectl get service <service>
```

## Delete Pods

In order to delete pods you have two options:

1. Use the following command:

```bash
kubectl delete pod <pods>

# Example: kubectl delete pod pod1 pod2 pod3 ...
```

2. Use the manifest to delete all the objects (Pods, Services, etc...) explicitly defined on the manifest:

```bash
kubectl delete -f manifest.yml

# Example: kubectl delete -f manifest1.yml -f manifest2.yml -f manifest3.yml ...
```

## Delete Services

```bash
kubectl delete service <services>

# Example: kubectl delete service service1 service2 service3 ...
```

## Get namespaces

```bash
kubectl get namespaces
```

## Crete a namespace

Using the imperative way (CLI):

```bash
kubectl create namespace <name>
```

Using the declarative way:

```bash
kubectl apply -f <manifest>
```

## Delte a namespace

```bash
kubectl delete namespace <name>
```

> [!NOTE]
>
> Deleting a namespace will also remove all the Pods, Services and other objects
> associated to it.

## Configure kubectl to run all commands agains a specific namespace

```bash
kubectl config set-context --current --namespace <name>
```
