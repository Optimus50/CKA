#### Question: Create a namespace called "mynamespace" and a pod with image nginx called nginx on this namespace
<details><summary>show</summary>
<p>

```bash
kubectl create namespace mynamespace
kubectl run nginx --image=nginx --restart=Never -n mynamespace
```

</p>
</details>

#### Question: Create the pod that was just described using YAML
 
``kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml``
``kubectl create -f pod.yaml -n mynamespace``
or
`` kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml | kubectl create
-n mynamespace -f -``
