## Understand deployments and how to perform rolling update and rollbacks
### Create the DaemonSet with nginx image, update the ds with newer version of the nginx server.
<details><summary>show</summary>
<p>

create the yaml file
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
        image: nginx:1.12.1-alpine
        ports:
        - containerPort: 80
```

using the yaml file to create the ds
```bash
kubectl create -f ds.yaml
```

check the updateStrategy. DaemonSet has two update strategy types: 
- OnDelete: after you update a DaemonSet template, new DaemonSet pods will only be created when you manually delete old DaemonSet pods. 
- RollingUpdate:  after updating a DaemonSet template, old DaemonSet pods will be killed, and new DaemonSet pods will be created 
automatically.
The default is RollingUpdate
```bash
kubectl get ds ds-one -o yaml | grep -a1 updateStrategy:
```

</p>
</details>


## Know various ways to configure applications
## Know how to scale applications
## Understand the primitives necessary to create a self-healing application
