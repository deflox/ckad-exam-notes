* Docker CMD and ENTRYPOINT vs Kubernetes
* Difference between ReplicaSet and Deployment
* Why after `kubectl create token <serviceaccount>` there is still displayed `<none>` when running the `kubectl describe sa <serviceaccount>`
* How to edit an existing deployment?
* Taunts and Node Affinity (what happens to existing nodes)
* How to switch namespace permanently?
* Why is https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/#:~:text=Get%20a%20shell%20to%20nginx%20Container%3A not documented in when `kubectl exec -h`
* apply, replace, patch: difference https://stackoverflow.com/questions/47241626/what-is-the-difference-between-kubectl-apply-and-kubectl-replace
* Implication of using `kubectl set image deployment/myapp-deployment <new-image-version>` to update a deployment
* Why in for NetworkPolicy can I use either `-` or not?
* Where to find templates for YAML fines in doc?
* `kubeadm`
* Questions directly in notes of udemy course.
* Do I need to configure authentication
* If no verbs are assigned to a role, are all verbs allowed?
* How to assign that a role may only perform actions within one namespace?
  * Role's are only created in a namespace and therefore also only control access in this namespace
* When I can "" be set for GroupAPI?
* Different types of Secrets?
* YAML practice at KodeKloud
* Practice:
  * Blue-Green Deployment
  * Secret ConfigMaps