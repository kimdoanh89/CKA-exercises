## 1. Understand the Kubernetes API primitives
All the Kubernetes API reference is [here](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.14/).

All the kubectl commands reference is [here](https://v1-14.docs.kubernetes.io/docs/reference/generated/kubectl/kubectl-commands).
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


## 2.Understand the Kubernetes cluster architecture
## 3.Understand Services and other network primitives
