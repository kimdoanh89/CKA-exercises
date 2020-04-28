## 1. Understand the Kubernetes API primitives
API overview is [here](https://kubernetes.io/docs/reference/using-api/api-overview/).

All the Kubernetes API reference is [here](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.14/).

All the kubectl commands reference is [here](https://v1-14.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands).

### Create a static pod named static-busybox that uses the busybox image and the command sleep 1000
```bash
kubectl run --restart=Never --image=busybox static-busybox --dry-run -o yaml --command -- sleep 1000 > /etc/kubernetes/manifests/static-busybox.yaml
```

### Use JSON PATH query to retrieve the osImages of all the nodes and store it in a file /opt/outputs/nodes_os_x43kj56.txt
```bash
kubectl get nodes -o jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt
```


### Create an NGINX Pod
<details><summary>show</summary>
<p>

```bash
kubectl run nginx --generator=run-pod/v1 --image=nginx
```

</p>
</details>

### Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
<details><summary>show</summary>
<p>

```bash
kubectl run nginx --generator=run-pod/v1 --image=nginx --dry-run -o yaml > nginx.yaml
```

</p>
</details>

### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)
<details><summary>show</summary>
<p>

```bash
kubectl create deployment nginx --image=nginx --dry-run -o yaml > nginx-deployment.yaml
```
Open the nginx-deployment.yaml file that is created and modify replicas: 4
    
</p>
</details>

### Create a Service named nginx of type NodePort to expose deployment nginx's port 80 on port 30080 on the nodes:
<details><summary>solusion 1</summary>
<p>

```bash
kubectl expose deploy nginx --type=NodePort --port=80 --dry-run -o yaml > nginx-service.yaml
```
Open and modify the file nginx-service.yaml, to add nodePort: 30080

</p>
</details>

<details><summary>solusion 2</summary>
<p>

Expose deployment nginx, this way the nodePort is randomly choosen between 30000-32767 , then use kubectl edit to modify the port of the service to nodePort: 30080
```bash
kubectl expose deploy nginx --type=NodePort --port=80 
kubectl edit svc nginx
```

</p>
</details>

### Create a pod redis with image redis:alpine and label tier=db
<details><summary>show</summary>
<p>

```bash
kubectl run redis --generator=run-pod/v1 --image=redis:alpine -l tier=db
```

</p>
</details>

### Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379 with the label tier=db
<details><summary>show</summary>
<p>

```bash
kubectl expose pod redis --name redis-service --port=6379 -l tier=db
```

</p>
</details>

### How a pod in a name space reach the service in another namespace (for example: dev namespace)
<details><summary>show</summary>
<p>

using something like:
```bash
db-service.dev.src.cluster.local
```

</p>
</details>


## 2.Understand the Kubernetes cluster architecture
## 3.Understand Services and other network primitives
