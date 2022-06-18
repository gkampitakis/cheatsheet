<h1 align="center">Kubernetes</h1>

## Contents

- [Deployments](#deployments)
  - Restart
  - Scale
  - Rollback
  - Pause/Resume
  - Watch update status 
- [Pods](#pods)
  - Port forward into pod/svc
  - Get all pods in a node
  - Get all pods by status
  - Get all images from all running pods
  - Delete all `Completed` pods in a namespace
  - Get metrics for a pod from metrics-server
  - Start pod from an image
  - Get pod by selector
- [Nodes](#nodes)
  - Get all
  - Drain
  - Uncordon
  - Label
  - Add/Remove taint
- [Misc](#misc)
  - Copy file from your machine inside a pod
  - Create docker-hub secret
  - Copy secret from one namespace to another
  - List all events ordered
  - Do request from one pod do another
  - Set Current Context
  - Get Current Context
  - List resources

- [Useful Tools](#useful-tools)
- [Links](#links)

## Commands

### Deployments
#### Restart

```bash
kubectl rollout restart deployment <deployment> # -n <namespace>
```

#### Scale

```bash
kubectl scale deployment/<deployment> --replicas=<number> # -n <namespace>
```

#### Rollback

```bash
kubectl rollout undo deploy <deployment> # --to-revision=1 -n <namespace>
```

#### Pause/Resume

```bash
kubectl rollout pause/resume deploy <deployment> # -n <namespace>
```

#### Watch update status

```bash
kubectl rollout status deploy <deployment> # -n <namespace>
```

### Pods

#### Port forward into pod/svc

Useful for when you want to have access to a pod locally.

```bash
kubectl port-forward <pod> <host-port>:<pod-port> # -n <namespace>

kubectl port-forward service/<service> <host-port>:<pod-port> # -n <namespace>
```

#### Get all pods in a node

```bash
kubectl get pods --field-selector spec.nodeName=<node> # -A / -n <namespace>
```

#### Get all pods by status

```bash
kubectl get po --field-selector status.phase=<Pending | Running> # -A / -n <namespace>
```

#### Get all images from all running pods

```bash
kubectl get pods --all-namespaces -o=jsonpath='{range .items[*]}{"\n"}{.metadata.name}{":\t"}{range .spec.containers[*]}{.image}{", "}{end}{end}' |\
sort
```

#### Delete all `Completed` pods in a namespace

```bash
kubectl get po --field-selector status.phase=Succeeded --no-headers -n <namespace> | awk '{print $1}'  | xargs kubectl delete po -n <namespace>
```

#### Get metrics for a pod from metrics-server

```bash
kubectl get --raw /apis/metrics.k8s.io/v1beta1/<namespace>/default/pods/<pod> | jq

#or

kubectl top pod <pod> # -n <namespace>
```

#### Start pod from an image

```bash
kubectl run <pod> --image=<image> --restart=Never
```

#### Get a pod by selector

```bash
# e.g. --selector=app=my-app
kubectl get pod --selector=<selector>
```

### Nodes

#### Get all

```bash
kubectl get nodes # --selector='<key>=<value>'
```

#### Drain

```bash
kubectl drain <node> --grace-period=<seconds> --ignore-daemonsets=true
# You can set a --pod-selector to point to a specific pod by label
```

#### Uncordon

```bash
kubectl uncordon <node>
```

#### Label

```bash
kubectl label node <node> <key>=<value>
```

#### Add/Remove taint

```bash
# Set node taint
kubectl taint nodes node1 key=value:NoSchedule
# and remove
kubectl taint nodes node1 key=value:NoSchedule-
```

The taint has key `key`, value `value`, and taint effect can be `NoSchedule`, `NoExecute` or `PreferNoSchedule`.

### Misc

#### Copy a file from your machine inside a pod

For more details you can also check `kubectl cp --help`.

```bash
tar cf - package.json | kubectl exec -i <pod> -- tar xf - -C /path/inside/pod
```

#### Create docker-hub secret

```bash
kubectl create secret docker-registry docker-hub --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

#### Copy a secret from one namespace to another

```bash
kubectl get secret <secret-name> --namespace <source-namespace>  -o yaml | grep -v '^\s*namespace:\s' |  kubectl apply --namespace=<destination-namespace> -f -
```

#### List all events ordered

```bash
kubectl get events --sort-by=.metadata.creationTimestamp # -n <namespace>
```

#### Do request from one pod do another

```bash
curl http://<service>.<namespace>
```

#### Set Current Context

For managing different contexts you can also have a look at [kubectx](https://github.com/ahmetb/kubectx).

```bash
kubectl config set-context --current # -n <namespace>
```

#### Get Current Context

```bash
kubectl config get-context
```

#### List resources

```bash
# List all resources in a namespace
kubectl get all # -n <namespace>

#List a resource in all namespaces
kubectl get <deployment/ingress/hpa etc> -A
```

#### Get image from deployment/pod

```bash
# Deployment
kubectl get deployment <deployment> --jsonpath='{.spec.template.spec.containers[*].image}'

# Pod
kubectl get po --selector=app=<app> -o jsonpath='{.items[*].spec.containers[*].image}'

# or 
kubectl get po <pod> -o jsonpath='{.items[*].spec.containers[*].image}'
```

---

**Note**: You can also find a lot of commands in [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)


### Useful Tools

- [Stern](https://github.com/stern/stern) ⎈ Multi pod and container log tailing for Kubernetes -- Friendly fork of
- [Krew](https://krew.sigs.k8s.io/) Krew is the plugin manager for kubectl command-line tool.


### Links

- [Ephemeral containers explained](https://iximiuz.com/en/posts/kubernetes-ephemeral-containers/)
