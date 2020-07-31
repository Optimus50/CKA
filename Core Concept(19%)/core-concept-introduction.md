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



