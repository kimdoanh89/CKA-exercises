## Understand deployments and how to perform rolling update and rollbacks
### Create the DaemonSet with nginx image, update the ds with newer version of the nginx server, change the updateStrategy to OnDelete
<details><summary>show</summary>
<p>

Create the yaml file
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: ds-one
spec:
  selector:
    matchLabels:
      name: ds-one
  template:
    metadata:
      labels:
        name: ds-one
    spec:
      containers:
      - name: nginx
        image: nginx:1.10.1
        ports:
        - containerPort: 80
```

Using the yaml file to create the ds
```bash
kubectl create -f ds.yaml
```

Check the updateStrategy. DaemonSet has two update strategy types: 
- OnDelete: after you update a DaemonSet template, new DaemonSet pods will only be created when you manually delete old DaemonSet pods. 
- RollingUpdate:  after updating a DaemonSet template, old DaemonSet pods will be killed, and new DaemonSet pods will be created 
automatically.
The default is RollingUpdate.
```bash
kubectl get ds ds-one -o yaml | grep -a3 updateStrategy:
```
Check the Image of the pod. It should be 1.10.1
```bash
kubectl describe pod ds-one-<tab> | grep Image:
```
Set the new version of nginx server. Check the Image of the pod. It should be 1.12.1-alpine
```bash
kubectl set image ds ds-one nginx=nginx:1.12.1-alpine
kubectl describe pod ds-one-<tab> | grep Image:
```
Check the rollout history. View the settings for various versions
```bash
kubectl rollout history ds ds-one
kubectl rollout history ds ds-one --revision=1
kubectl rollout history ds ds-one --revision=2
```
Change DaemonSet back to the earlier version, check the Image:
```bash
kubectl rollout undo ds ds-one --to-revision=1
kubectl describe pod ds-one-<tab> | grep Image:
```
Change the updateStrategy to OnDelete:
```bash
kubectl edit ds ds-one
```
```yaml
  updateStrategy:
    #rollingUpdate:   <-- Delete these 2 lines
    #  maxUnavailable: 1
    type: OnDelete    <-- Change this line
```
Check the rollout history and undo to revision 2. Check the Image:, still 1.10.1. After delete one pod, the Image of the new Pod will be
1.12.1-alpine
```bash
kubectl rollout history ds ds-one
kubectl rollout undo ds ds-one --to-revision=2
kubectl describe pod ds-one-<tab> | grep Image:    # <-- still 1.10.1
kubectl delete pod ds-one-<tab>
kubectl describe pod ds-one-<tab> | grep Image:    # <-- new pod should be 1.12.1-alpine
```

</p>
</details>


## Know various ways to configure applications
## Know how to scale applications
### Create the Deployment with nginx image, scale the Deployment to replicas=3
<details><summary>show</summary>
<p>
  
```bash
kubectl create deployment nginx --image=nginx
kubectl scale deployment nginx  --replicas=3
```

</p>
</details>

## Understand the primitives necessary to create a self-healing application
