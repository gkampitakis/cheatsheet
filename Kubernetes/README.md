# Kubernetes

## Commands

- Port forward into pod/svc 
 
Useful for when you want to have access to a pod locally.

```bash
kubectl port-forward <pod-name> -n <namespace> <host-port>:<pod-port>

# You can portforward to a service as well

kubectl port-forward service/<service-name> -n <namespace> <host-port>:<pod-port>
```

- Copy a file from your machine inside a pod

For more details you can also check `kubectl cp --help`.

```bash
tar cf - package.json | kubectl exec -i <pod-name> -- tar xf - -C /path/inside/pod
```

- Restart a deployment

```bash
kubectl -n <namespace> rollout restart deployment <name>
```

- Set Current Context (e.g namespace)

For managing different contexts you can also have a look at [kubectx](https://github.com/ahmetb/kubectx).

```bash
kubectl config set-context --current --namespace <namespace>
```

- Get Current Context

```bash
kubectl config get-context
```

- Scale a deployment

```bash
kubectl  scale --replicas=5  deployment/<deployment-name> -n  <namespace>
```

- Deployment  operations

```bash 
# Watch update status for deployment
kubectl rollout status deploy/<deployment>

# Pause/resume deployment
kubectl rollout pause/resume deploy<deployment>

# Rollback to revision
kubectl rollout undo deploy<deployment> (optional) --to-revision=1
```

- List resources

```bash
# List all resources in a namespace
kubectl get all -n <namespace>

#List a resource in all namespaces
kubectl get <deployment/ingress/hpa etc> --all-namespaces
```

- Get metrics for a pod from metrics-server

```bash 
kubectl get --raw /apis/metrics.k8s.io/v1beta1/<namespace>/default/pods/<pod-name> | jq

#or

kubeclt top pod <pod-name> -n namespace 
```

- Get all images from all running pods

```bash 
kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.containers[*]}{.image}{", "}{end}{end}' |\
sort
```

- Get all nodes by selector

```bash 
kubectl get nodes --selector='<label-key>=<label-value>'
```

- Uncordon Node ( set is again as schedulable usually after draining)

```bash
kubectl uncordon <node-name>
```

- Drain node

```bash
kubectl drain node <node-name> --grace-period=<seconds> --ignore-daemonsets=true
``` 

- Label Node

```bash
kubectl label node <node-name> <label-key>=<label-value>
```

**Note**:  You can also find a lot of commands in [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
