### Scheduling

How scheduling works in kubernetes
    - nodeName by default is not set.
    - the scheduler picks the pod and assigns the pod to the appropriate node.
    - pods without node remain in a pending state
    
In a situation where the scheduling node does not exist, you have to manually add nodeName to your yaml file manually.

### Labels and Selector
 are standard methods to group things together, based on needs and criteria, they are properties attached to each item. 
 Selectors help your filter items. 
    How are they used in Kubernetes?
         kubernetes use labels internally to connect objects

####### Taints and Toleration
 What is actually taint and toleration?
        taint are set on nodes while tolerant are set on pod, the idea behind this can be simplified using a person and a bug illustration
        a person=node and a bug=pod, a person can be tainted and a bug that is not tolerant to this taint can not get to the person  \
        but when a bug is tolerant to the person, then the bug can get to the person. That is how this is done with nodes and pods in Kubernetes.\
        
   How do you do this?(Taints-Node)
   
     `` kubectl taint node node-name key=value:taint-effect``
  
Kubernetes does not taint the master node, but only in the worker node.

##### Node selectors

  This are used to label the nodes, so that pods can be deployed to that particular node. For example, when a node is label Large or small, this label is then added in the pod
  which allows the pod to be deployed to that particular node. But in a scenario where you have a complex labels like medium or small medium, then node affinity is used.

#### Node affinity
 the primary purpose of this, ensures pods are host in specific nodes\
 there are two types of node affinity types
          available:
                  requiredDuringSchedulingIgnoredDuringExecution
                  preferredDuringSchedulingIgnoredDuringExecution
  A pod has two property state before it gets to its final state, during scheduling and during execution
               
               
          planned:
                  requiredDuringSchedulingRequiredDuringExecution
 Taint and affinities both combined to ensure each pods are deployed to their specific node.
 
##### Resources and Limits
   when a container tries to use more cpu than its assigned cpu, by default kubernetes throttles the cpu, but when a container tries\
   to use more memory than its allotted memory, the pod crashes and is terminated
  
     

#### DaemonSets
  DaemonSets ensures one one replica of the pod is added and present in all nodes in a cluster, 
  use case, monitoring agents in the cluster, networking, kube-proxy are used cases of daemonSet
   how does a daemonSet works? it uses the nodeAffinity and nodeSchedule to deploy pods in nodes


#### Static Pods







###### Multiple Schedulers




   
