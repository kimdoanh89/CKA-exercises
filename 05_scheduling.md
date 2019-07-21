## 1. Use label selectors to schedule Pods

### We have deployed a number of PODs. They are labelled with 'tier', 'env' and 'bu'. How many PODs exist in the 'dev' environment?
<details><summary>show</summary>
<p>

```bash
kubectl get pod --selector=env=dev
```

</p>
</details>

### How many PODs are in the 'finance' business unit ('bu')?
<details><summary>show</summary>
<p>
  
```bash
kubectl get pod --selector=bu=finance
```

</p>
</details>

### How many PODs and other objects including ReplicaSets, etc. are in the 'prod' environment?
<details><summary>show</summary>
<p>
  
```bash
kubectl get all --selector=env=prod
```

</p>
</details>

### Identify the POD which is 'prod', part of 'finance' BU and is a 'frontend' tier?
<details><summary>show</summary>
<p>
  
```bash
kubectl get pod --selector=env=prod,bu=finance,tier=frontend
```

</p>
</details>


## 2. Understand the role of DaemonSets

[Here](https://github.com/kimdoanh89/CKA-exercises/blob/master/01_app_lifecycle_management.md#create-the-daemonset-with-nginx-image-update-the-ds-with-newer-version-of-the-nginx-server-change-the-updatestrategy-to-ondelete).

## 3. Understand how resource limits can affect Pod scheduling

### Create the deployment hog with the image vish/stree
<details><summary>show</summary>
<p>
  
```bash
kubectl create deployment hog --image vish/stress
```

</p>
</details>


### Save the deployment to yaml file and add the memory limits of 4Gi and memory requests of 2500Mi
<details><summary>show</summary>
<p>
  
```bash
kubectl get deployment hog -o yaml --export > hog.yaml
vi hog.yaml 
```
```yaml        
        imagePullPolicy: Always
        name: stress
        resources:
          limits:
            memory: "4Gi"
          requests:
            memory: "2500Mi"

```
```bash
kubectl replace -f hog.yaml
```

</p>
</details>

### Edit the hog configuration file (limits of 1 cpu, 4Gi memory with requests of 0.5 cpu, 500Mi memory), and add arguments for stress to consume CPU and memory (-cpus 2 -mem-total 900Mi -mem-alloc-size 100Mi -mem-alloc-sleep 1s). 
<details><summary>show</summary>
<p>
  
```bash
kubectl get deployment hog -o yaml --export > hog.yaml
vi hog.yaml 
```
```yaml        
        imagePullPolicy: Always
        name: stress
        resources:
          limits:
            cpu: "1"
            memory: "4Gi"
          requests:
            cpu: "0.5"
            memory: "2500Mi"
        args:
        - -cpus
        - "2"
        - -mem-total
        - "900Mi"
        - -mem-alloc-size
        - "100Mi"
        - -mem-alloc-sleep
        - "1s"
```
```bash
kubectl delete deployment hog
kubectl create -f hog.yaml
```
**When a pod tries to exceed the resource limits**
- In case of CPU, K8s throttles the CPU so that it does not go beyond the specified limit.
- In case of memory, a container can use more memory resources than its limit. If the pod tries to consume more memory than its limit constantly, the Pod will be terminated .

Using kubectl top pod, you can see that the hog pod consumes around 987m of CPU (~ 1vCPU = 1000m) even when the container tries to use 2 units of CPU with the command -cpus 2, and 929Mi of memory. 
```bash
kubectl top pod
```

</p>
</details>

### Edit the hog configuration file with limits of 1 cpu, 500Mi memory with the same args above (pod tries to consumes more memory than limit)

<details><summary>show</summary>
<p>

The pod will be terminated. It tries to restarts and will be terminated again and again.

</p>
</details>



## 4. Understand how to run multiple schedulers and how to configure Pods to use them
## 5. Manually schedule a pod without a scheduler
### Manually schedule a pod with the nginx image to the master node without a scheduler
<details><summary>show</summary>
<p>

Adding the nodeName field with the node you want to schedule the pod:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx

spec:
  containers:
  - image: nginx
    name: nginx
  nodeName: master
```

**A quick note on editing PODs and Deployments**

**Edit a POD**

Remember, you CANNOT edit specifications of an existing POD other than the below.
- spec.containers[*].image
- spec.initContainers[*].image
- spec.activeDeadlineSeconds
- spec.tolerations

For example you cannot edit the environment variables, service accounts, resource limits of a running pod. But if you really want to, you have 2 options:

1. Run the kubectl edit pod <pod name> command.  This will open the pod specification in an editor (vi editor). Then edit the required properties. When you try to save it, you will be denied. This is because you are attempting to edit a field on the pod that is not editable. A copy of the file with your changes is saved in a temporary location as shown above. You can then delete the existing pod, then create a new pod with your changes using the temporary file by running the commands:
   ```bash
   kubectl delete pod webapp
   kubectl create -f /tmp/kubectl-edit-ccvrq.yaml
   ```
2. The second option is to extract the pod definition in YAML format to a file using the command, then make the changes to the exported file using an editor (vi editor). Then delete the existing pod and create a new pod with the edited file.
   ```bash
   kubectl get pod webapp -o yaml > my-new-pod.yaml
   vi my-new-pod.yaml
   kubectl delete pod webapp
   kubectl create -f my-new-pod.yaml
   ```
  
**Edit Deployments**

With Deployments you can easily edit any field/property of the POD template. Since the pod template is a child of the deployment specification,  with every change the deployment will automatically delete and create a new pod with the new changes. So if you are asked to edit a property of a POD part of a deployment you may do that simply by running the command
```bash
kubectl edit deployment my-deployment
```

</p>
</details>

### Find and kill the static pod in a node

<details><summary>show</summary>
<p>
  
Show the kubelet service.
```bash  
sudo systemctl cat kubelet.service
sudo systemctl status kubelet.service
```
find the --config option
```bash
Environment="KUBELET_KUBECONFIG_ARGS=--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --kubeconfig=/etc/kubernetes/kubelet.conf"

Environment="KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml"
```
Look in to the file /var/lib/kubelet/config.yaml, looking for staticPodPath
```bash
staticPodPath: /etc/just-to-mess-with-you
```

</p>
</details>

## 6. Display scheduler events
 

