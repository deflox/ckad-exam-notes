# Why in for NetworkPolicy can I use either `-` or not?

* `podSelector` and `namespaceSelector` and combined as an AND rule and
* `ipBlock` is combined with the other block as an OR rule
* Every block could theoretically contain all three attributes and also not also all three attributes

```yaml
spec:
  ingress:
  - from:
    - podSelector:
      ...
      namespaceSelector:
      ...
    - ipBlock:
      ...
```

# `ENTRYPOINT` and `CMD` and how it is handled in kubernetes

In Docker, they have the following meaning:
* Only `ENTRYPOINT` defined: Cannot be overridden, running container with `docker run <container> <arg1> <arg2>` will append the `<arg1>` and `<arg2>` to the the executable that is defined as entrypoint
* Only `CMD` defined: Will be completely overridden when suplying arguments. When running `docker run <container> <arg1> <arg2>`, `<arg1>` must be some kind of executable otherwise it does not make sense

Source: https://stackoverflow.com/a/34245657

In Kubernetes:
| command | args | outcome                                                                                                                |
|---------|------|------------------------------------------------------------------------------------------------------------------------|
| Yes     | Yes  | Completely overriddes ENTRYPOINT and CMD and executes the command with the appended arguments                          |
| Yes     | No   | Completely overriddes ENTRYPOINT and CMD and executes the command                                                      |
| No      | Yes  | Args are appended to the ENTRYPOINT. If CMD is used instead of ENTRYPOINT, then the arguments act as their own command |
| No      | No   | Whatever is defined in the image is used                                                                               |

Source: https://stackoverflow.com/a/66078726

# Set context permantely with kubectl

Either edit the file `$home/.kube/config` and set the `namespace` attribute for the current context:
```yaml
contexts:
- context:
    cluster: kubernetes
    namespace: default
    user: kubernetes-admin
  name: kubernetes-admin@kubernetes
```

Or do it via command line:
```
$ kubectl config set-context --current --namespace=<namespace-name>
```

# `ReplicaSet` vs `Deployment`

A `ReplicaSet` maintains an amount of desired pods. If one pod is deleted manually, the replica set will recreate the pod to reach the desired amount of pods. A deployment is one more wrapper around replica sets which also maintains a deployment history and allows rollback to a previous state.

# Why after `kubectl create token <serviceaccount>` there is still displayed `<none>` when running the `kubectl describe sa <serviceaccount>`?

Because the token is not directly associated anymore with a service account. When running `kubectl create token <serviceaccount>` the token will be displayed but will not be associated with the service account.

# Taint vs. NodeAffinity vs. NodeSelector

* With taints nodes can be tainted and using `tolerations` pod can be configured to tolerate certain node. Works with taints
* Node affinity allows scheduling pods based on labels on nodes or labels on other pods
  * Is more flexible than NodeSelector because depending on the modes:
    * requiredDuringSchedulingIgnoredDuringExecution 
    * preferredDuringSchedulingIgnoredDuringExecution 
  * Allows more advanced logic with Exists, NotIn etc.
* NodeSelector also allows to select only a node with a certain label
  * Allows only key-value pairs

# Meaning of `""` for GroupAPI

* Only refers to core API resources (meaning they have nothing in front of the version `v1` for `apiVersion`). Otherwise e.g.:
  * `apps/v1`: `["apps"]`
  * `authorization.k8s.io/v1`: `["authorization.k8s.io"]`
  * `storage.k8s.io/v1`: `["storage.k8s.io"]`