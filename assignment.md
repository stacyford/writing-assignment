# Debug Operations in Kubernetes

This page provides an overview of how to debug your pods in Kubernetes. This document is not comprehensive, but provides 

The [`kubectl` command](https://kubernetes.io/docs/reference/kubectl/) is the Kubernetes Command Line Interface (CLI) tool. This command uses the [Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/) to communicate with your cluster's control plane. You can use this command to interact with Kubernetes in the CLI. 

## Display Current Pods

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

## Display Log Files

Reviewing the logs for your pods allows you to identify any errors or warnings. 

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

## Debug your Pod

If your pod is still not working correctly


`kubectl exec <pod> `

## Debug your Pod


`kubectl debug`


This command is used to retrieve the logs of a specific pod - do use this when you have to review logs or need to debug a container. Another we will discuss is the `kubectl exec` command. A command that we can use to debug a container from the inside or to explore the enviroment of the container itself.

**Note:** The command `kubectl debug` is another option to considering when debugging a container. This command can be used to create a clone of a pod that does not terminate if an error is experienced inside the container. 



## References
- [The `kubectl` command](https://kubernetes.io/docs/reference/kubectl/)
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-strong-getting-started-strong-

- [What is Kubernetes](https://kubernetes.io/docs/concepts/overview/)
