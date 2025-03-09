# Links
* Kubectl Reference: https://kubernetes.io/docs/reference/kubectl/generated/kubectl/
* POD Yaml file: https://kubernetes.io/docs/concepts/workloads/pods/#using-pods
* https://medium.com/@pranay.shah/nano-text-editor-tricks-for-ckad-exam-3b07f80dfe77
* https://kubernetes.io/docs/reference/kubectl/quick-reference/

* https://www.youtube.com/watch?v=tS2S926PriQ
* https://www.youtube.com/watch?v=2T86xAtR6Fo
* https://www.youtube.com/watch?v=rnemKrveZks

## Documentation:
* CLI Reference: https://kubernetes.io/docs/reference/kubectl/
* Object API: https://kubernetes.io/docs/reference/kubernetes-api/

## Touched Objects in the course

### Basic Concepts
* Pod: https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/pod-v1/
* ReplicationController: https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/replication-controller-v1/
* ReplicationSet: https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/replica-set-v1/
* https://kubernetes.io/docs/reference/kubernetes-api/workload-resources/deployment-v1/
* Namespace: https://kubernetes.io/docs/reference/kubernetes-api/cluster-resources/namespace-v1/

### Configuration
* ConfigMap: https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/config-map-v1/
* Secret: https://kubernetes.io/docs/reference/kubernetes-api/config-and-storage-resources/secret-v1/
* ServiceAccount: https://kubernetes.io/docs/reference/kubernetes-api/authentication-resources/service-account-v1/
* LimitRange: https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/limit-range-v1/
* ResourceQuota: https://kubernetes.io/docs/reference/kubernetes-api/policy-resources/resource-quota-v1/

# Kubernetes Architecture
 
* Control Plane Node = master Node
  * have to be linux
  * three is the usual amount you need.
  * they should be separated in different data centers.
  * they should be an odd number to avoid split brain or deadlook
    * split brain means that there is no majority of either failed nodes and they do not know what to do
  * consists of:
    * kube-apiserver
    * etcd cluster store
    * kube-controller-manager (watch loop meaning it constantly looks for changes)
    * kube-scheduler: delegates tasks to worker nodes
* Worker Node = slave node
  * kubelet
  * container runtime (pluggable with the CRI (container runtime interface))
  * kube-proxy: handles the node networking
* Declarative Model:
  * describe what you want in a config file not how to do it
* Pod:
  * runs one or multiple containers
  * shared execution environment which is used by the containers
  * usually there is one container for one pod
  * smallest managable executable in k8s
  * are scheduled to a single node
* service interface:
  * provides stable IP to load balance requests to healthy PODs
  * abstracts the whole access to pods
  * labels are used to tie pods to a service
  * NodePort Service
    * ensures that we can connect to any server-node and kubernetes makes sure to route to the correct pod
  * ClusterIP: access pods from within the cluster
* deployment object:
  * wraps controllers (ReplicaSet)
* objects:
  * the API offers a catalogue of objects that can be send to the API to do something

# Kubernetes Services

# Kubernetes Deployments

# Core Concepts:
* Pod
* ReplicaSet (and ReplicaController)
* Deployment
* ConfigMaps

## Namespaces
* Used to group different items within a cluster
* Kuberenetes creates a "Default" namespace by default
* Other namespaces: `kube-system` and `kube-public`
* Assign quotes to namespaces
* Namespaces can be defined in the metadata section of an object definition

Connect to other services in other namespaces:

# DNS
```
<service-name>.<namespace>.svc.cluster.local
```

# Hands-On

* Config home directory: `~/.kube/config`

# Dump



Briefly, kubectl apply uses the provided spec to create a resource if it does not exist and update, i.e., patch, it if it does. The spec provided to apply need only contain the required parts of a spec, when creating a resource the API will use defaults for the rest and when updating a resource it will use its current values.

The kubectl replace completely replaces the existing resource with the one defined by the provided spec. replace wants a complete spec as input, including read-only properties supplied by the API like .metadata.resourceVersion, .spec.nodeName for pods, .spec.clusterIP for services, and .secrets for service accounts. kubectl has some internal tricks to help you get that right, but typically the use case for replace is getting a resource spec, changing a property, and then using that changed, complete spec to replace the existing resource.

The kubectl replace command has a --force option which actually does not use the replace, i.e., PUT, API endpoint. It forcibly deletes (DELETE) and then recreates, (POST) the resource using the provided spec.

# Pods
* Pods are the smallest single unit inside a kubernetes cluster
* They can contain one ore multiple containers (although one container is usually the default)
* Certain properties of a POD cannot be edited
```
# pods

## display all pods
kubectl get pods

## display single pod in detail
kubectl describe pod <name>

## see logs of pod
kubectl log <name>

## delete a pod
kubectl delete pod <name>

## create pod via file
kubectl apply -f <name>.yaml

## 

# get resource 
kubectl get (pods|)

# describe any object in the cluster
kubectl describe (pod|) name

kubectl apply -f <file>.yml
kubectl get
kubectl describe Pod

kubectl delete -f pod.yml

imparative way to expose service:
kubectl expose pod hello-pod --name=hello-svc --target-port=8080 --type=NodePort

```

# RepliacaController/ReplicaSet
* https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
* Are controlling pods in the cluster and ensuring that they are fail-safe meaning if a pod fails, a new instance is created automatically
* Are almost the same but ReplicaSet requires a `selector` definition while it is optional for ReplicaController
* Api-Version for ReplicaSet is `apps/v1`
```
## 

```

# Namespaces

# 

# Links
* Service Discovery: https://nigelpoulton.com/demystifying-kubernetes-service-discovery/
* https://www.nigelpoulton.com/post/demystifying-kubernetes-service-discovery


