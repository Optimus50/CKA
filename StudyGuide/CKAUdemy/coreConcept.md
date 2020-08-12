# Cluster Architecture
   Worker nodes: This is responsible for hosting application as containers
   Master node:  Manage, plan, schedule, and monitor nodes
   Etcd: the key value store of the cluster
   
   ######### Master and Worker nodes consist of the following components
      - Kube scheduler: 
      - Node Controller: 
      - Kube api-server: 
      - container runtime:
      - kubelet: 
      - kube-proxy:
      
#Etcd
  What is ETCD?
   this is a distributed key value store .that is simple, secure and fast
  What is key-value Store?
    this a kind of storage with keys and values, 
  How to operate ETCD
  this is simple, download, extract and run binary.
  How does ETCD works in Kubernetes?
    the etcd server stores information about the nodes, pods, configs, secrets, accounts, roles, binding, etc. 
    when a change is complete, it is updated in the etcd cluster.
    
###### kube-api-server
 This is the primary management component in the kubernetes.
 this is the workflow. 
  kubectl ......  ---> kube-api-server: the request is authenticated and validated data is retrieved ---> ETCD( data about the cluster is retrieved here) ----> response and sent to the user
      
 Scheduler monitors the ETCD, if a pod is created, it picks this information that there is a pod with no node assigned, the scheduler then assigns a node that the pod will be assign 
 and communicates that to the Api-server.
 the Api-server than updates this information in the ETCD server, it then passes this information to the kubelet in the appropriate worker node.
 the kubelet then creates the pod in the node and instructs the container run time engine to deploy the application image.
 once done the kubelet updates the status to the api server and the api-server updates this status in the ETCD. 
   
 This pattern is followed should there be a change in the cluster
 In Summary, the Api-server is responsible for authenticating Users, validating requests, retrieving data from ETCD, updating the ETCD, it is the only component that interacts with the ETCD data store. 
    

#### Kube-controller-manager
This is responsible for monitoring the state of the whole system and brings it to the desired function state,
 eg, the node controller:- it checks the status of the nodes every 5 secs, by checking the health of each node. no heart beat \
     The replication controller:- this monitors the replication set of each pod and ensure that the desired number is available at all time.
                                   it ensures high availability, it allows loading balancing in the cluster, however, in the new kubernetes cluster
                                   replication is achieved with the aid of ReplicaSet, a replicaset requires a selector definition.
                                   why do you have labels and selectors 
                                    replicaset uses labels found in pod to monitor the pods, and ensure they have their desired state\.
                                   
  There are many more controller in the kubernetes cluster. This is seen as the brain in the kubernetes cluster. 
        

##### Kube-scheduler
This is responsible for deciding which pod goes to which node. 
How a scheduler works?
   it first filters the all the nodes based on resources available, 
   then it ranks the remaining nodes based on how many memories will be left after it has been assigned the pod
   the node with a greater amount of memory left is chosen.


##### kubelet
This is like a captain in a ship, when it gets instructions to load a pod in a node from the kube-scheduler, 
it then requests the container run time engine to pull the required image  and run an instance. The kubelet then continues to monitor the state of the pod and containers in it and reports the 
results to the kube-api server on a timely basis.
Installation is manual.

#### kubeProxy
Every pod can reach every other pod in kubernetes via a pod networking solution. 
This is a process that runs on each node, each searches for a new services and uses IP table rules in forwarding traffic to each pod.

### Pods
A pod is a single instance of an application, it is the smallest object you can create in kubernetes.
pods always have a one-to-one relationship to your application container.
 
####### Multi-container pods
          when containers uses a helper containers, then you can have two containers running in a pod.
          
   creating pods with yaml: restudy
        apiVersion: v1
        kind: Pod
        metadata: (data about the object)
           name: myapp-pod
           labels:
              app: myapp
              type: front-end
        spec:
          containers:
            - name: nginx-container 
              image: nginx\
     then use ```kubectl create -f (yaml-file-name.yml)```
     
### ReplicaSet
  The primary purpose of the ReplicaSet is to ensure that a specific number of replica pods 
  matches the actual state at all time. In other words, ReplicaSet make pods scalable. 
  ReplicaSet can be seen as self-healing mechanism, as long as primary conditions are met


### Deployments
 
 ### Namespace
  This is used for separating the work environment where policies are created for each 
  and the use of resources are also separated.
#####operational definition of namespaces
if you want to create a pod in a specific namespace, you need to add the create first a namespace for the pod\
`` kubectl create ns (name of namespace)``\
then if you want to change the current context of your kube-config\
`` kubectl config set-context $(kubectl config current-context) --namespace=prod``\

context are used to manage multiple clusters and environment from the same manage system\
create a resource quota in other to limit the use of resources in the cluster\
You can create a resource-quota with the following yaml file as an example.?
``
apiVersion: v1  
kind: ResourceQuota
metadata: 
    name: compute-quota
    namespace: dev
spec:
   hard:
      pods: "10"
      request.cpu: "4"
      request.memory: 5Gi
      limits.cpu: "10"
      limits.memory: 10Gi
``\

##### Services
 this enables communication between components between applications and the outside world. 
 Services enables loose coupling between microservices in the cluster. This is also an object in kubernetes
 we have\ 
 NodePort Service: listens to ports in a pod and enables accessibility:  
       it has two ports, a port =80 that it uses to connects to other pods port in the cluster, and another NodePort=30000-32767 for external connection.\
       Linking a service with the pod, the labels used for the pod creation is used in the selector option in our service yaml file.
 ClusterIp: a virtual Ip is created in the cluster that connects set of services in the cluster. 
             uses random algorithm to balance loads each pod.
 LoadBalancer: this is used in a cloud portal, where nodes are automatically managed 
 
 
 #### Imperative and Declarative
Which is used in today cluster technology is either with Declarative approach or Imperative
Kubernetes uses  both approaches. The use of kubectl create, expose, replace, set, replace enables things to be done in the imperative approaches\
while the kubectl apply command enables the declarative approach

using ``--dry-run`` creates the resource and to test use ``--dry-run=client``, this tells you if the resource can be created and if the command is right\
Then the ``-o yaml``  will output the resource definitions in yaml format on the screen.
