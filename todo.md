* Do I need to configure authentication
* If no verbs are assigned to a role, are all verbs allowed?
* How to assign that a role may only perform actions within one namespace?
  * Role's are only created in a namespace and therefore also only control access in this namespace
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
* Can you not edit an replicaset with `k edit`?
  * Answer: Yes you can. But you need to manually delete the pods so that the replicaset recreates them automatically
* requiredDuringSchedulingIgnoredDuringExecution 
* ["/bin/sh", "-c", "sleep 4800"] vs ["sleep", "4800"]
* https://www.udemy.com/course/certified-kubernetes-application-developer/learn/lecture/26359180#questions/15546554
* Difference between RequestQuota and LimitRange
* "Create a pod with resource requests cpu=0.5, memory=1Gi and limits cpu=1, memory=2Gi in namespace one"
* Whats the usecase for hostPort
* Mistake: "Create an nginx deployment of 2 replicas, expose it via a ClusterIP service on port 80. Create a NetworkPolicy so that only pods with labels 'access: granted' can access the pods in this deployment and apply it"