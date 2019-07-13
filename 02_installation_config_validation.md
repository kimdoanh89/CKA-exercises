## 1. Design a Kubernetes Cluster
## 2. Install Kubernetes Masters and Nodes
### 2.1. Install Master
<details><summary>show</summary>
<p>
  
Follow [instructions using native packages management](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management)

Enter root and update, upgrade the system
```bash
sudo -i
apt-get update && apt-get upgrade -y
```
Install docker 
```bash
apt-get install -y docker.io
```
Add new repo for kubernetes
```bash
vim /etc/apt/sources.list.d/kubernetes.list
```
```vim
deb http://apt.kubernetes.io/ kubernetes-xenial main
```
Add a GPG key for the package, and update with new repo
```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
apt-get update
```
Install kubeadm, kubelet, and kubectl
```bash
apt-get install -y kubeadm=1.14.1-00 kubelet=1.14.1-00 kubectl=1.14.1-00
```
Install Container Networking Interface (CNI). Follows intrustions to use [Calico for Network Policy](https://docs.projectcalico.org/v3.8/getting-started/kubernetes/)

</p>
</details>

## 3. Configure secure cluster communications
## 4. Configure a highly-available Kubernetes cluster
## 5. Know where to get the Kubernetes release binaries
## 6. Provision underlying infrastructure to deploy a Kubernetes cluster
## 7. Choose a network solution
## 8. Choose your Kubernetes infrastructure configuration
## 9. Run end-to-end tests on your cluster
## 10. Analyze end-to-end test results
## 11. Run Node end-to-end Tests
## 12. Install and use kubeadm to install, configure, and manage Kubernetes clusters
