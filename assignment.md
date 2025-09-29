# Debug Operations in Kubernetes

This document is a reference guide for basic debugging of your Kubernetes pods. While this document is not comprehensive, it will provide a high-level overview of the most common ways to troubleshoot your pods with the `kubectl` command. 

The [`kubectl` command](https://kubernetes.io/docs/reference/kubectl/) is the Kubernetes Command Line Interface (CLI) tool. This command uses the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) to communicate with your cluster's control plane. You can use this command to interact with Kubernetes in the CLI. 


## Review Your Current Pods

Review the pods in your Kubernetes cluster to confirm that they operate as expected.

Use the following command to return a list of your currently active pods:

```shell
kubectl get pods --namespace <namespace>
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

The STATUS field contains the current phase of your pods. If a pod is supposed to be active and displays a status other than `Running`, then you may need to investigate further. 

## Review Your Log Files

Review your pod's logs to identify any errors or warnings. Reviewing your pod's logs can also ensure that your pod is operating normally. 

Use the following command to display a pod's logs:

```shell
kubectl logs <pod> 
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

## Interact with Your Pod

Use the `kubectl exec` command to issue commands directly to your pod's containers, review configuration files, or log in to an interactive shell. This can help you ensure that no other errors exist on the system.

Use the following command to interact with your pod:

```shell
kubectl exec <pod> [-c <container>] [<flags>] -- <command>
```

Your command might resemble one of the following examples:

| Command                               | Description                                   |
|---------------------------------------|-----------------------------------------------|
| `kubectl exec -it <pod> -- bash`      | Opens an interactive shell inside your pod.   |
| `kubectl exec <pod> -- ls /`          | Lists the contents of the root directory.     |
| `kubectl exec <pod> -- env`           | Shows the container’s environment variables.  |

## Create a Copy of Your Pod

If your pod will not start, is crashing repeatedly, or does not have an interactive shell, use the `kubectl debug` command. This command creates a clone of the specified pod and lets you work on a clean copy of your pod. Use this to debug a pod without worrying about affecting a pod in your production environment. 

Use the following command to start a debug pod:
```shell
kubectl debug <pod> [flags] [--image=<image-name>] --share-processes [--copy-to=app3-debug]
```


## References
- [The `kubectl` command](https://kubernetes.io/docs/reference/kubectl/)
- [What is Kubernetes](https://kubernetes.io/docs/concepts/overview/)
