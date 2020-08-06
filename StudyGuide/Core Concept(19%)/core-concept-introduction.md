### Kubernetes Concept. 

What are pods? 
How do you check if pods are available?
How do you run a pod ?

#### Creating a pod 
With the use of kubectl command a pod can be created easily. 
 ``kubectl run nginx --image=nginx``
  this pulls a nginx image from docker hub and creates a pod in our cluster.      
     
Now let's create a pod with a yaml file

a kubernetes yaml file has the following fields

 + apiVersion
 + kind: refers to type of object to be created
 + metadata: data about the object(), the spacing should be correct
 + spec: this is a dictionary, then it has a list of containers and has some list.
 
 these are top-level required fields they must be in your yaml file
 
 #Controllers in Kubernetes
 ### Replication controllers
 This enables running more than once instance of a pod which provides high availability.
 Ensures that the specified number of pods are running at all time.
 Helps in balancing and scaling pods in the cluster.
 
 Replica Set and Replication Controller have the same purpose but are not the same, the first is the new way things are done 
 and the later is the older way of doing things. 
 
 
 Let us look at creating Replication controller
 
 apiVersion: v1
 kind: ReplicationController
 metadata:
   name: myapp-rc
   labels:
      app: myapp
      type: frontend
 spec:
   template:
      metadata:
        name: myapp-pod
        labels:
           app: myapp
           type: frontend
      spec:
        containers
           - name: nginx-container
             image: nginx
   template: 3
   
The following is the Replica set

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
     app: myapp
     type: front-end
spec: 
  template:
      metadata:
         name: myapp-pod
         labels:
           app: myapp
           type: front-end
      spec:
        containers:
          - name: nginx
            image: nginx
      
  replicas: 3
  selector:
     matchLabels: 
        type: front-end
        
The replicaSet monitors the pods and if anyone fails it re-start the pods, it uses labels to know which pods 
to restart when the pods are no longer available. The template definition section is 
required to add the required configuration.

Keep this command at your finger tips
``kubectl create -f ``
``kubectl delete``
``kubectl replace -f``
``kubectl scale --scale=(number of pods to be created)``
``kubectl edit replicaset (name of the replicaset)``

kubectl scale replicaset (name of replicaset) --replicas=2 or more, this command can also be used to scale down the number
of pods if wanted


#### deployments

rolling updates? 

This is high up in the hierachy, creates a kubernetes object called deployment. 


there are two types of deployment strategies
  you deploy the old instances and install all new instance(recreate strategy)
  updates is seamless, (rolling update), this is the default way of deployment.
  
  The following are necessary for deployments
   `` kubectl create -f (deployment-definition.yaml)``
   `` kubectl get deployments``
   `` kubectl apply -f (deployment-definition.yaml)``
   ``kubectl set image deployment/(app-name-in-the-deployment) (imageName=image)``
   ``kubectl rollout status (deployment/(app-name-in-the-deployment))``
   `` kubectl rollout  history     ""  /              ""``
   ``kubectl rollout undo deployment/(appName)``
  
  
  
  ##Networking in Kubernetes
  
  each node always have an ip address, and this nodes has an network that consists of pods with its own ip address
  You need a networking solution to manage network ips in each nodes, that use simple routing technique.
  
 ## Services
 this enables communication from inside the cluster with the outside world, 
 this is also an object, we have different type of services
 ###### NodePort:  
         NodePort opens a specific port on your node/VM and when that port gets traffic, that traffic is forwarded directly to the service.         
         There are a few limitations and hence its not advised to use NodePort         
         - only one service per port         
         - You can only use ports 30000-32767         
         - Dealing with changing node/VM IP is difficult
         
 ###### ClusterIP:
 ClusterIP is the default kubernetes service. 
 This service is created inside a cluster and can only be accessed by other pods in that cluster. 
 So basically we use this type of service when we want to expose a service to other pods within the same cluster.  
 This service is accessed using kubernetes proxy.      
 
 
 ###### LoadBalancer
 
 This is the standard way to expose service to the internet. All the traffic on the port is forwarded to the service.
 It's designed to assign an external IP to act as a load balancer for the service.  
 There's no filtering, no routing. LoadBalancer uses cloud service. 
  we however have few limitations
    - every service exposed will have its own ip address, this over time gets very expensive. 



## Micro service architecture
 the use of a sample application voting application
  deployments is the best way of deployment application pods, as this also creates ReplicaSets



