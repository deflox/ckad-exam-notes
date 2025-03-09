* https://kubernetes.io/docs/reference/kubernetes-api/
* https://github.com/dgkanatsios/CKAD-exercises/tree/main
* https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands

# Useful flags:
* `--all-namespaces`: show things/resources in all namespaces at once
* `--dry-run=client`: only show the resource output of the object to be created
* `-n <namespace-name>` after kubectl to execute command in different namespace
* `--no-headers=true | wc -l` to count returned objects
* `-A` shows all objects regardless of namespace
* `k api-resources` show resource names and their aliases
* `--as` can be added to any user to perform the operation with this user
* `-l <label>` can be used to filter object by label
* `--show-labels`: 

# Shortcuts
* po for POD
* rs for ReplicaSet
* deploy for Deployments
* svc for Service
* ns for Namespaces
* netpol for Network Policy
* pv for Persistent Volumes
* pvc for Persistent Volume Claims
* sa for Service Accounts

`man 5 crontab` -> fron cron expression

# Expose

```
kubectl expose pod redis --port=6379 --name=redis-service
```

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
```
kubectl logs -f <podname> # -f will show live logs
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
