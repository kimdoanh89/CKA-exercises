## 1. Understand Kubernetes cluster upgrade process

### Upgrade from v1.14.1 to 1.14.4
<details><summary>show</summary>
<p>

Follow instructions from [Kubernetes document](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade-1-14/).

The upgrade workflow at high level is the following:

- Upgrade the primary control plane node.
- Upgrade additional control plane nodes.
- Upgrade worker nodes.

#### Find the latest stable 1.14 version:
```bash
sudo -i
apt update
apt-cache policy kubeadm
# find the latest 1.14 version in the list
# it should look like 1.14.x-00, where x is the latest patch
```

#### Upgrade the first control plane node (master):
```bash
apt-mark unhold kubeadm kubelet && apt-get update && apt-get install -y kubeadm=1.14.4-00 && apt-mark hold kubeadm
kubeadm upgrade plan
kubeadm upgrade apply v1.14.4
```

Upgrade the kubelet and kubectl on the control plane node:
```bash
apt-mark unhold kubelet kubectl && apt-get update && apt-get install -y kubelet=1.14.4-00 kubectl=1.14.4-00 && apt-mark hold kubelet kubectl
```
Restart kubelet:
```bash
systemctl restart kubelet
```
#### Upgrade additional control plane nodes:
```bash
sudo kubeadm upgrade node experimental-control-plane
```
#### Upgrade worker nodes:
Upgrade kubeadm on all worker nodes:
```bash
apt-mark unhold kubeadm kubelet && apt-get update && apt-get install -y kubeadm=1.14.4-00 && apt-mark hold kubeadm
```
Going back to master node and cordon the worker node. Prepare the node for maintenance by marking it unschedulable and evicting 
the workloads:
```bash
kubectl drain worker1 --ignore-daemonsets --delete-local-data
```
Enter the worker node and Upgrade the kubelet config:
```bash
sudo -i
kubeadm upgrade node config --kubelet-version v1.14.4
```
Upgrade kubelet and kubectl, and restart kubectl
```bash
apt-mark unhold kubelet kubectl && apt-get update && apt-get install -y kubelet=1.14.4-00 kubectl=1.14.4-00 && apt-mark hold kubelet kubectl
systemctl restart kubelet
```
#### Uncordon the worker node and verify the status of the cluster:
```bash
kubectl uncordon worker1
kubectl get nodes
```

</p>
</details>

## 2. Facilitate operating system upgrades
## 3. Implement backup and restore methodologies
