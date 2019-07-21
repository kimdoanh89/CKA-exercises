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
          requests:
            memory: 2500Mi
          limits:
            memory: 4Gi
```
```bash
kubectl replace -f hog.yaml
```

</p>
</details>


## 4. Understand how to run multiple schedulers and how to configure Pods to use them
## 5. Manually schedule a pod without a scheduler
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

### StaticPod
systemctl show kubelet.service
find the --config option
Environment=KUBELET_KUBECONFIG_ARGS=--bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf\x20--kubeconfig=/etc/kubernetes/kubelet.conf KUBELET_CONFIG_ARGS=--config=/var/lib/kubelet/config.yaml

Look in to the file /var/lib/kubelet/config.yaml, looking for staticPodPath
staticPodPath: /etc/just-to-mess-with-you

## 6. Display scheduler events
 

