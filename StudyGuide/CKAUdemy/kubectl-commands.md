


## Kubectl Commands for CKA

###  Pod

kubectl run (podname) --image=(image-name)\
``kubectl run nginx --image=nginx --restart=Never``
``kubectl get pods``\
``kubectl describe pod (pod-name) -n (namespace name)`` \

To generate a pod yaml and not create it\
`` kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml``



### ReplicaSet

kubectl create -f (replicaset-definition-file.yml)\
``kubectl create -f rc-definition.yml`` \
``kubectl get replicaset``\
``kubectl get pods``\
``kubectl edit replicaset (replicasets name)``\
``kubectl delete replicaset (replicasets name)``\
``kubectl  replace -f replicaset-defintion.yml``\
`` kubectl scale --replicas=6 replicaset (name of replicaset)``

#### Deployments

``kubectl get deployments``\
``kubectl get replicaset``\
``kubectl get pods``\
``kube get all``

or for the exams
`` kubectl create deployment --image=nginx nginx``\
 then to generate a deployment yaml \
 `` kubectl create deployment --image=nginx nginx --dry-run -o yaml``\
 Then generate a deployment yaml file with 4 replicas and then save it to a file\
 `` kubectl create deployment --image=nginx nginx --dry-run -o yaml > nginx-deployment.yaml``


