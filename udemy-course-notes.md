- [Core Concepts](#core-concepts)
  - [RepliacaController/ReplicaSet](#repliacacontrollerreplicaset)
  - [Namespaces](#namespaces)
- [Multi-Container Pods](#multi-container-pods)
  - [Init Containers](#init-containers)
- [Observability](#observability)
  - [Readiness](#readiness)
  - [Liveness](#liveness)
  - [Container Logging](#container-logging)
  - [Monitor and Debug Applications](#monitor-and-debug-applications)
- [POD Design](#pod-design)
  - [Labels, Selectors and Annotations](#labels-selectors-and-annotations)
  - [Rolling Updates \& Rollbacks in Deployments](#rolling-updates--rollbacks-in-deployments)
    - [More Strategies](#more-strategies)
  - [Jobs](#jobs)
  - [CronJob](#cronjob)
- [Services \& Networking](#services--networking)
  - [Ingress Networking](#ingress-networking)
  - [Network Policies](#network-policies)
    - [Developing Network Policies](#developing-network-policies)
- [State Persistence](#state-persistence)
  - [Persistent Volumes](#persistent-volumes)
  - [Persistend Volume Cliams](#persistend-volume-cliams)
- [Security](#security)
  - [Authentication](#authentication)
  - [KubeConfig](#kubeconfig)
  - [API Groups](#api-groups)
  - [Authorization](#authorization)
  - [Role Based Access Control](#role-based-access-control)
  - [Cluster Roles](#cluster-roles)
  - [Adminssion Controllers](#adminssion-controllers)
    - [Create Own Admission Controllers](#create-own-admission-controllers)
- [API Versioning](#api-versioning)
  - [API Versions](#api-versions)
    - [API Derecations](#api-derecations)
- [Customization](#customization)
  - [Custom Resource Definitions](#custom-resource-definitions)
  - [Custome Resource Controller](#custome-resource-controller)
  - [Operator Framework](#operator-framework)
- [Helm](#helm)
  - [Concepts](#concepts)

# Core Concepts

## RepliacaController/ReplicaSet
* https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
* Are controlling pods in the cluster and ensuring that they are fail-safe meaning if a pod fails, a new instance is created automatically
* Are almost the same but ReplicaSet requires a `selector` definition while it is optional for ReplicaController
* Api-Version for ReplicaSet is `apps/v1`

## Namespaces
* Used to group different items within a cluster
* Kuberenetes creates a "Default" namespace by default
* Other namespaces: `kube-system` and `kube-public`
* Assign quotes to namespaces
* Namespaces can be defined in the metadata section of an object definition

# Multi-Container Pods
* Sidecar Pattern: Logging-Agent alongside Webserver. 
* Adapter Pattern: Container inside POD that processes logs before sending it to central server.
* Ambassador Pattern: Container handels descision to which environment logs should be send to.

## Init Containers
* Are allowed to perform one task and terminate
* They are specified in a separate section in the pod definition file
* They all must complete until the actual contains in the pod can start

https://kubernetes.io/docs/concepts/workloads/pods/init-containers/

# Observability

## Readiness
* By default, pods are marked as ready, once the container is running
* But they actual application may not actually be ready (e.g. a webserver takes another 2-3 minutes until it is actually running)
* Therefore, Readiness probes can be configured on the pod to customize the behavior when a Pod is marked as ready 

## Liveness
* Kubernetes usually restarts a Pod when the container is failing
* But there may be a case where the container is actually up and running but the application is actually not working at all
* Therefore, Liveness probes can be configured on the pod allow the 

## Container Logging
* By default, all logs made to sdtout are accessable via command: `kubectl logs <pod-name> <container-name>`
* With the flag `-f` the logs can be watched live

## Monitor and Debug Applications
* There is no standardized monitoring solution that is shipped with kubernetes
* For the developer exam, only basic knowledge on the `metrics-server` is required
* The metric-server recevies monitoring metrics from the `cAdvisor` component that lives inside the `kubelet` component

```
# view node statistics
kubectl top node

# view pod statistics
kubectl top pod
```

# POD Design

## Labels, Selectors and Annotations
* Labels allow to categorize different items within a kubernetes cluster
* With Selectors, the items can be filtered using their labels
  * `--selector app=App1` to select items only with label key app with value App1
* Annotations are another way to specify meta information about an object but is not used to select objects

## Rolling Updates & Rollbacks in Deployments
* There exists different deployment strategys to update an existing deployment to the newest version of an app (meaning a newer version of the image)
  * Recreate: Delete all existing instances and create instnace of the new version
  * Rolling Update (default): Repliace each instance with new version, one by one (one pod may be down for update at a time)
* Under the hood, the deployment will create a new replicaset and depending on the strategy, will replace the old replicaset with the new replicaset

```
kubectl create -f deployment.yaml # create deployment
kubectl apply-f deployment.yaml # make changes to the deployment
# alternative (but this will cause the deployment file of the current deployment which one could have stored locally, to be out of sync with the actual deployment):
kubectl set image deployment/myapp-deployment <new-image-version>

kubectl edit deployment # komplettes deployment editieren

```

### More Strategies
* Blue Green: Blue (old) still takes all request, while Green (new) is already deployment. Tests are performed on green and when deemed ok, everything is switched at once from Blue to Green
  * Not natively supported in kubernetes but can be achieved with services:
    * Services connects to pods using labels (e.g. blue). When green is ready, switch the selector in the service from blue to green and the service now routes all traffic to the pods of the green deployment
* Canary: Only small percentage of traffic is routed to the new version. If everything looks good, the new version is rolled out in the deployment
  * Not natively supported in kuberentes but can be achived with services:
    * New deployment is created with the minimum amount of pods (1). 
    * All pods in the current and new deployment receive common label to which the service may route traffic
    * Finally, if everything is ok, the new version is actually rolled out with the current deployment 
  * Limited control over the traffic split. We can only rely on number of pod. E.g. if 5 pod in current deployment and 1 pod in new deployment, the traffic split is 83% vs 17% but if the current number would be higher (99), then only 1% of traffic would go to new deployment

## Jobs
* Default behavior of Pod's is, that they are always restarted once they terminate or complete, which is desired for user facing applications, but not for batch like workloads (`restartPolicy: Always`)
* Jobs can manage multiple Pod's to complete one batch operation
  * One could for example define a Pod definition and specify that there are 3 completions needed for this pod. The job then runs Pod's until at least three Pod complete their task sucessfully

## CronJob
* Is a wrapper object for a `Job` that encapsulates the job into an `jobTemplate` and allows running jobs repeatatly

# Services & Networking
* Services make Pods or group of Pods available to the outside
* NodePort Service:
  * Listens to ports on the node and forwards requests to this port to a defined set of pods
    * TargetPort: Port on the POD to forward traffic to (if not provided it is assumed to be the same as Port)
    * Port: Port of the service itself
    * Service IP: The service itself also has a IP address within the cluster
    * NodePort: Port that is exposed on the node (30000-32767) (if not provided a random availabel port will be taken from the range)
  * Labels and selectors are used to select pods on the cluster
  * Also acts as a Random load balancer, if more than one pod is found with the label selector
  * NodePort services are also distributed across nodes, meaning the same port can be used to access the same pod on each node
* ClusterIP Service:
  * Create internal virtual IPs inside the cluster to enable communication to other components in the cluster
  * It is basically a instrument to add a virtual IP in front of 1..n pods
  * Spec:
    * TargetPort: The port of the pod this service should forward traffic to
    * Port: The port of the service where incoming traffic should be received
* LoadBalancer Service:
  * Provisions a loadbalancer in supported cloud environments

## Ingress Networking
* Ingress Controllers enable you to have multiple different apps behind one NodePort (or one LoadBalancer)
* They are not directly provided by Kubernetes but instead are provided by different applications (like nginx)
* Ingress Configurations are directly editable via `kubectl edit`
* https://kubernetes.github.io/ingress-nginx/examples/rewrite/

## Network Policies
* Ingress Traffic: Incoming Traffic
* Egress Traffic: Outgoing Traffic
* All Pods are in a virtual private network that spans the whole cluster
  * By default, all components can talk to each other (All Allow Rule)
* Network Policies allow us to restrict network traffic
* Network Policies work for certain Pods that are identified via label

### Developing Network Policies
* We do not need a separate rule for responses because once a request is allowed the response is automatically allowed too
* Once we assign a NetworkPolicy to a POD, this POD will not be able to communicate at all with anyone nor can the pod receive any requests
* It is also possible to allow connections comming directly from IP addresses
* Network Policys relie on the network solutions that kubernetes uses and some do not support Network Policies

```
payroll-service.default.svc.cluster.local
```

# State Persistence
* Pods are transient by nature, meaning if they shut down their data is deleted
* So we can create volumes to save persistent data
* `hostPath` is not recommended on multi-host environments

## Persistent Volumes
* Instead of configuring volumes on each Pod, persistent volumes offer a cluster-wide solution which can be first configured and afterwards used by Pods

## Persistend Volume Cliams
* Persisted Volumes must be claimed by Persisted Volume Claims
* When Persistent Volumes are created, the Persistent Volume Claim looks out for a claim with the same access modes that is big enough to fulfil its claim and claims it
* To make the selection more specific, labels can be used
* Once a persistent volume has been claimed, the policy decides what happens to it, if a claim is deleted:
  * Retain: Do not delete persistet volume
  * Delete: Do delete the persistet volume
  * Recycle: Clear data on persistet volume and make it available again

# Security
* First and foremost the hosts of the kubernetes cluster need to be secured
  * no password authentication, only SSH
* Authorization is implemented using RBAC (Role Based Access Control)

## Authentication
* All requests go through the `kube-apiserver` and every request is authorized
  * does not matter if used with kubectl or directly with HTTP API access
* Authentication mechanisms:
  * Static Password files (must pass during starting the kube-apiserver (`--basic-auth-file`))
  * Static Token files (similar as password files (`--token-auth-file`))
  * Certificates
  * Identity services (LDAP)

## KubeConfig
* By default lies in user home directory at `$HOME/.kube/config`. Is setup with:
  * Clusters: What clusters are available
  * Users: What users exists on the system
  * Contexts: Marrys Clusters and Users together to assign Users to Clusters
    * Need to specificy default-context to use: `kubectl config use-context prod-user@production`
    * Context can also contain a namespace field which specifies the namespace that is used with this context
* Allows configuring the kubectl command environment
* You may use `certificate-authority-data` instead of `certificate-authority` to pass whole certificate inside the config instead of refering to a file (in base 64)

## API Groups
* The API is grouped into certain groups (like /api /version /logs etc.)
  * core group: all core functionalities
  * names group: contains newer functionalities that are more distinctly grouped
    * named -> API Groups -> Resources -> Verbs
* kubectl proxy can used if you do not want to provide certs etc. to the curl command while accessing the HTTP api

## Authorization
* As Admin you are allowed to perform any action on the server
* Mechanisms:
  * Node Authorization:
  * Attribute Based Authorization (ABAC):  
    * Allows to assigns permissions direclty to user using a policy file in JSON format
    * Maintenance is cumbersome as every change requires a restart of the kube API service
  * Role Base Authorization (RBAC):
    * Instead of directly assigning permissions to users, the permissions are grouped in a role and roles are assigned to users
  * Webhook:
    * Can be used if the access control should be externalized
  * AlwaysAlow (default): Everything is allowed
  * AlwaysDeny: Everything is denied
* When starting the kube-apiserver, multiple authorization mechanisms can be specified and they are then used in the order they were specified

## Role Based Access Control
* Roles are created using kubernetes API objects (Role object)
* Roles are assigned to users using the RoleBinding kubernetes object
* Are namespace scoped, they only give permissions to objects inside the same namespace they live in

## Cluster Roles
* For resources that are cluster-wide meaning that they cannot be associated with a namespace
  * e.g. view, create, delete nodes
* But can also be used for namespace scoped resources, which then means that the role has access to the given permissions in all namespaces

## Adminssion Controllers
* With RBAC you only achieve the restriction of operations on specific API-objects
* Admission controllers allow to specify more specific restrictions, like not using specific image-repositories or running images as root user etc.
* There are some default admission controllers like `AlwaysPullImages`, `DefaultStorageClass`, ...
* `kube-apiserver -h | grep enable-admission-plugins` to view all admission controllers
  * or prefix: `kubectl exec kube-apiserver-controlplane -n kube-system --` if run in KubeADM environment
* Can be enabled or disabled with `--enable-admission-plugins` or `--disable-admission-plugins`
* Can also change requests itself (like the `NamespaceAutoProvision` admission controller can first create a namespace if it does not exist yet)

`/etc/kubernetes/manifests/kube-apiserver.yaml`
`ps -ef | grep kube-apiserver | grep admission-plugins`

* Types of Admission Controllers:
  * ValidatingAdmissionController: Are validating the requests
  * Mutating Admission Controller: Are changing the requests
* Orders matter, because if a validating is before a mutating the request may fail even though the mutating would have prevented this

### Create Own Admission Controllers
* There are two special admission controllers: MutatingAdmissionWebhook and ValidatingAdmissionWebhook, that call a webhook on a custom own backend server where we can customize the behavior
* To custmize we create a Validating/MutatingWebhookConfiguration kubernetes API object that creates


# API Versioning

## API Versions
* There exist different version for API-objects:
  * /v1: Is generally available and supported
  * /v1alpha1: Is in very early stage and there is no commitment to support
  * /v1beta1: Committment to be supported and generally available but not yet fully tested
* Some API objects have multiple currently supported versions
* `preferredVersion`: Default version used to query objects
* `storageVersion`: Default version used when storing objects into etcd
* Can be set when starting the API-server when settings the `--runtime-config` parameter
  * Contains comma-separated list of API-version you'd like to enable

### API Derecations
* APIs can only be removed by incrementing the API group
* Versions must be supported for different type of API versions. So releasing a newer beta version for example will not automatically remove the older version, it will instead be marked as deprecated
  * alpha: 0 versions need to be supported
  * beta: 3 or 9 months whatever is longer need to be supported
  * stable: 3 or 9 month whatever is longer need to be supported
* Alpha versions cannot deprecate beta and ga versions
* Beta cannot deprecate ga versions
* And ga can deprecate both alpha and beta versions
* In order for a new release becoming the preferred version, there must be at least one release that already supported both versions
* APIs should be able to round trip without loss of information (meaning we should go to v1alpha1 -> v1alpha2 -> v1alpha1 wihtout loss of information)
* With `kubectl convert` we can convert older YAML versions to newer versions

# Customization

## Custom Resource Definitions
* Controllers are responsible for reacting to changes for objects inside the etcd database and applying these changes (like changing the amount of pods within a deployment)
* Every resource has a controller that tracks and applies changes to its resource object
* To create an own resource a Custom Resource Definition (CRD) object has to be created in the kubernetes cluster
* This will allow you to create and store your custom resources in etcd but nothing is done with them because no controller exist yet

## Custome Resource Controller
* Are written in Go and allow to continueisly query the etcd database for changes and react to them
* To start one can clone the sample-repo: https://github.com/kubernetes/sample-controller.git
* To start the controller build in Go:
  * `./sample-controller -kubeconfig=$HOME/.kube/config`

## Operator Framework
* Allows to combine a custom resource definition and a custom controller so that for example if a custom resource is created, the controller is automatically provisioned etc.
* All operators are available at http://operatorhub.io

# Helm
* Normally inside kubernetes objects are all treated independenly and they have no relation to each other
* So deploying a large app may be hard because we need to manage all this objects that belong to an app
* Helm adds another abstraction layer over kubernetes that is application-aware and packages kubernetes object into one single application and allows installing them with one command
* Settings can be customized with a values.yaml file where we can change certain cettings

## Concepts
* For each kubernetes object the corresponding YAML definition is defined within a template where certain values are set as variables
* These variables are then replaced by helm using the values from the `values.yaml` file
* Finally, there also exist a Chart.yaml that provides general metadata for helm package
* Find charts here: https://artifacthub.io

```
helm search hub ... # to search official hub
helm search repo ... # to search repo added

helm install <release> . # to install from local directory (if it has been downloaded before)
```