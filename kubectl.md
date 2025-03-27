* https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands
* https://kubernetes.io/docs/reference/kubectl/quick-reference/

# Useful flags:
* `--all-namespaces`: show things/resources in all namespaces at once
* `--dry-run=client`: only show the resource output of the object to be created
* `-n <namespace-name>` after kubectl to execute command in different namespace
* `--no-headers=true | wc -l` to count returned objects
* `-A` shows all objects regardless of namespace
* `k api-resources` show resource names and their aliases
* `--as` can be added to any user to perform the operation with this user
* `-l <label>` can be used to filter object by label. can also be used to label a pod when using `kubectl run`
* `--show-labels`: E.g. print an object with get and also get the labels of the object `kubectl get node node01 --show-labels`
* `man 5 crontab`: to view documentation for crontab expressions
* `-w` watch results of a kubectl command
* `--rm` immediately destroy pod after creation (e.g. for busybox)
* `-L` allows displaying labels als columns with the `kubectl get` command

```
k get pod -l "app in (v1,v2)"
```

# Shortcuts
* `po` for POD
* `rs` for ReplicaSet
* `deploy` for Deployments
* `svc` for Service
* `ns` for Namespaces
* `netpol` for Network Policy
* `pv` for Persistent Volumes
* `pvc` for Persistent Volume Claims
* `sa` for Service Accounts

# Difference between `apply`, `patch`, `replace`, `edit`:
* `replace -f <file>`: Will completely replace the whole object with the contents of the specified file, so every attribute that was not specified explicitly in the file will be lost if it did already exist.
* `apply -f <file>`: Will apply changes to the object by merging the contents of the input file with the attribute contents of the target object:
  * Attribute missing in the file but present on the object → Not touched
  * Attribute present in both file & object → Updated to match the file
  * Attribute present in the file but missing in the object → Added to the object
  * Attribute missing in both file & object → No change
* `edit <resource> <name>`: Will open an editor with all current attributes and their values of the object. 
  * If you REMOVE an attribute → It will be removed from the object.
  * If you ADD a new attribute → It will be added to the object.
  * If you MODIFY an existing attribute → It will be updated accordingly.
* `path`: Allows only updating a targeted set of attributes and their values. The input works best in JSON format, because JSON can be flatlined to just one string. But YAML is theoretically also possible:
  * `kubectl patch deployment my-deployment -p '{"spec": {"template": {"spec": {"containers": [{"name": "my-app", "resources": {"limits": {"cpu": "500m"}}}]}}}}'`
  * `kubectl patch deployment my-deployment -p "$(cat patch.yaml)"`

# Imperative Commands
* Labelling is only possible imperatvely:
  * `kubectl run`
* Check on which node pod is deployed: `kubectl ge tpod -o wide`
* Objects without imparative commands:
  * PersistentVolumeClaim
  * PersistentVolume
  * LimitRange
```bash
# create a pod with image nginx
k run pod-name --image=nginx

# create job
k create job <name> --image=<image> -- <command>

# create a replicaset
# currently not possible imperatively

# create a deployment images nginx and 3 replicas
kubectl create deployment deployment-name --image=nginx --replicas=3

# create services
k create service (clusterip|loadbalancer|nodeport) service-naem

# create role
kubectl create role developer-role --verb=get --resource=pods

# create rolebinding

# create new user in kubeconfig
kubectl config set-credentials martin --client-key=/root/martin.key --client-certificate=/root/martin.crt

# create context binding
k config set-context developer --cluster=kubernetes --user=martin

# create request quota
kubectl create quota my-rq --namespace=one --hard=requests.cpu=1,requests.memory=1Gi,limits.cpu=2,limits.memory=2Gi

kubectl expose deployment my-webapp --name front-end-service --type NodePort --port 80
kubectl create ingress ingress-name --rule="domain.com/path/*=service-name:service-port"

# create job from cronjob
kubectl create job --from=cronjob/sample-cron-job sample-job
```

Commands vs. Arguments:
```bash
kubectl run <name> --image=<image> --command -- sleep 30
```
results in...
```yaml
- command:
    - sleep
    - "30"
```
```bash
kubectl run <name> --image=<image> -- sleep 30
```
results in...
```yaml
- args:
    - sleep
    - "30"
```
... there is no such flag for the kubectl create job command, because there it is always treated as a command

# Vi/Vim
* `:u` undo last command

# Expose
* Can be used to expose either pods or services
```
kubectl expose pod redis --port=6379 --name=redis-service
```
# `kubectl set`
* Allows 

# Describe

# Replace

# Apply

# Exec

# Taint

# Label

# Get
```
kubectl get [pod|rs|deployment]

kubectl get rs new-replica-set -o yaml
```

# Edit
* TODO: How to change to nano?
```
kubectl edit <type> <name>
```

# Delete
```
kubectl delete <type> <name>
kubectl delete -f <filename>
kubectl delete pod --all # delete all pods
```

# Run
```
kubectl run webapp-green --image="kodekloud/webapp-color" --dry-run=client -o yaml -- "--color=green"
```

## Scale

## Logs
* Prints logs of a container that are written to stdout and stderr
```
kubectl logs -f <podname> # -f will show live logs

# show logs of a previous instance of a pod
kubectl logs nginx -p
```

## Rollout
```
kubectl rollout status <deployment> # display current status of deployment
kubectl rollout history <deployment> # display history of deployment
```

## Auth
```
# to check if the user is allowed to create deployments
kubectl auth can-i create deployments
# to check if a specific user has access to a certain permission
kubectl auth can-i create deployments --as other-user
# and to include if he's allowed within a namespace
kubectl auth can-i create deployments --as <user-name> --namespace <namespace-name>
```

## Api-Resources
```
# check api-resources that are either namespaces or not
kubectl api-resources --namespaces=<true|false>
```

## Explain

## Convert
