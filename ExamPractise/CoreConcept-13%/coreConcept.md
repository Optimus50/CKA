##### Question: Create a namespace called "mynamespace" and a pod with image nginx called nginx on this namespace
<details><summary>show</summary>
<p>

```bash
kubectl create namespace mynamespace
kubectl run nginx --image=nginx --restart=Never -n mynamespace
```

</p>
</details>

##### Create the pod that was just described using YAML
<details><summary>show</summary>
<p>

Easily generate YAML with:

```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml > pod.yaml
```

```bash
cat pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```

```bash
kubectl create -f pod.yaml -n mynamespace
```

Alternatively, you can run in one line

```bash
kubectl run nginx --image=nginx --restart=Never --dry-run=client -o yaml | kubectl create -n mynamespace -f -
```

</p>
</details>


##### Create a  busybox pod (using kubectl command) that runs the command "env". Run it and see the output.

<details><summary>show</summary>
<p>

```bash
kubectl run busybox --image=busybox --commmand --restart=Never -it -- env # it will help in seeing the output or just run it without -it

kubectl run busybox --image=busybox --command --restart=Never -- env # then you can type 
kubectl logs busybox # to get the log files 
```

</p>
</details>
