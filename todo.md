* Docker CMD and ENTRYPOINT vs Kubernetes
* Difference between ReplicaSet and Deployment
* Why after `kubectl create token <serviceaccount>` there is still displayed `<none>` when running the `kubectl describe sa <serviceaccount>`
* How to edit an existing deployment?
* Taunts and Node Affinity (what happens to existing nodes)
* How to switch namespace permanently?
* Implication of using `kubectl set image deployment/myapp-deployment <new-image-version>` to update a deployment
* Where to find templates for YAML fines in doc?
* `kubeadm`
* Questions directly in notes of udemy course.
* Do I need to configure authentication
* If no verbs are assigned to a role, are all verbs allowed?
* How to assign that a role may only perform actions within one namespace?
  * Role's are only created in a namespace and therefore also only control access in this namespace
* When I can "" be set for GroupAPI?
* Different types of Secrets?
* Practice:
  * Blue-Green Deployment
  * Secret ConfigMaps
* Read and understand: https://www.nigelpoulton.com/post/demystifying-kubernetes-service-discovery
* Read through certificate tips from udemy course
* Document where one can find examples for YAML definitions in documentation.
* Are multiple terminals possible in exam?
* `storageClassName` was not discussed in lecture? Is it always StoreClass?
* Permantely switch namespace?
* https://linuxcommand.org/tlcl.php
* Worth considering? https://learn.kodekloud.com/user/courses/ultimate-certified-kubernetes-application-developer-ckad-mock-exam-series?title=Ultimate
* VI how to delete line till end
  * how to delete everything following afterwards
* Can you not edit an replicaset with `k edit`?
  * Answer: Yes you can. But you need to manually delete the pods so that the replicaset recreates them automatically
* requiredDuringSchedulingIgnoredDuringExecution 
* ["/bin/sh", "-c", "sleep 4800"] vs ["sleep", "4800"]
* https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/26359180#questions/15546554
* Difference between RequestQuota and LimitRange
* "Create a pod with resource requests cpu=0.5, memory=1Gi and limits cpu=1, memory=2Gi in namespace one"