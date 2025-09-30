# Debug Operations in Kubernetes

This document provides an overview of the most common ways to troubleshoot your pods with the `kubectl` command. 

The [`kubectl` command](https://kubernetes.io/docs/reference/kubectl/) is the Kubernetes Command Line Interface (CLI) tool. This command uses the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) to communicate with your cluster's [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane). You can use this command to interact with Kubernetes in the CLI. Use this command to manage your pods and applications in Kubernetes. 

## Review Your Current Pods

Reviewing the pods in your Kubernetes cluster can confirm that they are active and operating as expected. 

### List Your Pods

Listing your pods can provide a quick overview of their status. 

To return a list of your current pods, use the following command:

```shell
kubectl get pods [--namespace <namespace>]
```

Your command might resemble the following example:

```shell
kubectl get pods --namespace default
```
```shell
NAME                                   READY   STATUS    RESTARTS   AGE
app2-79fd488c99-88csb                  1/1     Running   0          124m
kubernetes-bootcamp-658f6cbd58-27x6s   1/1     Running   0          79m
my-app-59854d5646-g7n6c                1/1     Running   0          124m
```

**Note**: The `--namespace` flag is **optional**. If you do not provide a `--namespace`, Kubernetes will return the pods in your `default` namespace.

The STATUS field contains your pod's current status. If a pod that should be active displays a status other than `Running`, then you may need to investigate further. Potential statuses include:

| Status              | Description                                                      |
|---------------------|------------------------------------------------------------------|
| Running             | The pod is active and operational.                               |
| Pending             | The pod is spawning.                           |
| Failed              | The pod's containers terminated, but one failed.                 |
| Succeeded           | The pod's containers terminated successfully.                    |

For more detailed information about this command, read the [`kubectl get`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) documentation.

### Review Detailed Pod Information

Reviewing the detailed information about your pods can provide more in-depth information about the pod's status. When you issue this command, Kubernetes will return the pod's current status, a full list of its containers, and any error messages.  For example, when you issue this command, the `Events` section might show that failed to run and why. 

To review more detailed information about your pod, use the following command:

```shell
kubectl describe pod <pod name>
```

Your command might resemble the following example:

```shell
kubectl describe pod app3
```
```shell
Name:             app3
Namespace:        default
Priority:         0
Service Account:  default
Node:             docker-desktop/192.168.65.3
Start Time:       Sun, 28 Sep 2025 14:15:19 -0500
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.1.0.9
IPs:
  IP:  10.1.0.9
Containers:
  my-container:
    Container ID:   docker://4da47a869aa2c8c1973a6009b7889c8685976d085c46a6364376b73d9c9871aa
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:d5f28ef21aabddd098f3dbc21fe5b7a7d7a184720bc07da0b6c9b9820e97f25e
    Port:           <none>
    Host Port:      <none>
    State:          Running
      Started:      Sun, 28 Sep 2025 14:15:22 -0500
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-lxpt7 (ro)
...
Events:                      <none>
```

For more detailed information about this command, read the [`kubectl describe`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe) documentation.

## Review Your Log Files

Reviewing your pod's logs can ensure that your pod is operating normally. This is useful when you want to review what happened inside your pod. When you issue this command, Kubernetes returns the most recent log entries from your containers.

To display your pod's logs, use the following command:

```shell
kubectl logs <pod> [-c <container>]
```

Your command might resemble the following example:
```shell
kubectl logs app3
```
```shell
2025/09/28 19:15:22 [notice] 1#1: using the "epoll" event method
2025/09/28 19:15:22 [notice] 1#1: nginx/1.29.1
2025/09/28 19:15:22 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14+deb12u1)
2025/09/28 19:15:22 [notice] 1#1: OS: Linux 6.6.87.2-microsoft-standard-WSL2
2025/09/28 19:15:22 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2025/09/28 19:15:22 [notice] 1#1: start worker processes
2025/09/28 19:15:22 [notice] 1#1: start worker process 29
2025/09/28 19:15:22 [notice] 1#1: start worker process 30
2025/09/28 19:15:22 [notice] 1#1: start worker process 31
2025/09/28 19:15:22 [notice] 1#1: start worker process 32
```

**Note**: If your pod only has one container, you do not need to use the `-c <container> option`. However, you must specify a container if your pod contains more than one container. 

For more detailed information about this command, read the [`kubectl logs`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs) documentation. 

## Issue Commands Directly to Your Pod

Use the `kubectl exec` command to issue commands directly to your pod's containers, review configuration files, or log in to an interactive shell. This is useful when you need to review what is currently happening in your pod, and can help you ensure that no other errors exist on the system. Sometimes the `kubectl logs` command does not reveal any errors, but further examination of the pod's container might. 

To issue commands inside and interact with your pod, use the following command:

```shell
kubectl exec <pod> [-c <container>] [<flags>] -- <command>
```

Your command might resemble one of the following examples:

| Command                               | Description                                   |
|---------------------------------------|-----------------------------------------------|
| `kubectl exec -it <pod> -- bash`      | Open an interactive shell in your pod.   |
| `kubectl exec <pod> -- ls /`          | List the contents of the root directory.     |
| `kubectl exec <pod> -- env`           | Show the container’s environment variables.  |

For more detailed information about this command, read the [`kubectl exec`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#exec) documentation. 

## Debug Your Pod with a New Container

If your pod will not start, is crashing repeatedly, or does not have an interactive shell, use the `kubectl debug` command. This command can add an [ephemeral container](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) in the specified pod or create a copy of a pod. Use this to debug a pod without worrying about changing an existing container in your production environment. 

To debug your pod, use the following command:
```shell
kubectl debug <pod> [command]
```

To start an ephemeral container, your command might resemble the following example:
```shell
kubectl debug -it app3 --image=ubuntu
```
This command adds an ephemeral container to the `app3` pod and uses `ubuntu` to provide a lightweight, interactive debugging environment. Use this when you need to debug a pod that is running. 

To copy a pod so that you can debug it, your command might resemble the following example:
```shell
kubectl debug app3 -it --image=ubuntu --copy-to=app3-debug
```
This command copies the existing `app3` pod to a new pod named `app3-debug` that uses Ubuntu. Use this when a pod does not start, or when you need to avoid changing the existing pod. 

For more detailed information about this command, read the [`kubectl debug`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#debug) documentation. 

## References
- [The `kubectl` command](https://kubernetes.io/docs/reference/kubectl/)
- [What is Kubernetes](https://kubernetes.io/docs/concepts/overview/)
