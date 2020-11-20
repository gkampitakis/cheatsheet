<h1 align="center">Kubernetes</h1>

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
kubectl drain <node-name> --grace-period=<seconds> --ignore-daemonsets=true
# You can set a --pod-selector to point to a specific pod by label
``` 

- Label Node

```bash
kubectl label node <node-name> <label-key>=<label-value>
```

- Create docker-hub secret

```bash
kubectl create secret docker-registry docker-hub --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```
- Get all pods in a specific node

```bash
kubectl get pods --all-namespaces  --field-selector spec.nodeName=<node-name>
```

- Node taints

```bash
# Set node taint
kubectl taint nodes node1 key=value:NoSchedule
# and remove 
kubectl taint nodes node1 key=value:NoSchedule-
```
The taint has key `key`, value `value`, and taint effect can be `NoSchedule`, `NoExecute` or `PreferNoSchedule`.

- Kubectl Configuration File

```bash
kubectl config view
```

### Access the API server

- Kubectl get API token 

```bash
export TOKEN=$(kubectl describe secret -n kube-system $(kubectl get secrets -n kube-system | grep default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d '\t' | tr -d " ")
```

- Get the API server endpoint

```bash
export APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")
```

Finally you can reach the API server using `curl`

```bash
curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure
```

Also you can use the client certificate, client keym and certificate authority data from the `.kube/config` file. You need to extract them and then encode them.

```bash 
curl $APISERVER --cert encoded-cert --key encoded-key --cacert encoded-ca
```

**Note**:  You can also find a lot of commands in [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
