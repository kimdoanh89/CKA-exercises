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
Download calico.yaml file. Looking for the CALICO_IPv4POOL_CIDR. Note that the default is 192.168.0.0/16. If you are using a different pod CIDR, change it accordingly in the calico.yaml file.
```bash
wget https://docs.projectcalico.org/v3.8/manifests/calico.yaml
cat calico.yaml | grep -a2 CIDR
```
Need to [turn off all swap devices](https://serverfault.com/questions/684771/best-way-to-disable-swap-in-linux) before initalize the master. Initialize the master using the following command. Save the kubeadm join output to use when adding workers to the cluster.
```bash
kubeadm init --kubernetes-version=1.14.1 --pod-network-cidr=192.168.0.0/16 | tee kubeadm-init.out
```
```bash
kubeadm join 10.0.2.15:6443 --token m3jpro.pvufj1envk6mx3g5 \
    --discovery-token-ca-cert-hash sha256:822885260222721f04296d96d490ddf9de568cd3507b735a1c21a5185755042e
```
Exit root and following instructions for the regular user
```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
Copy calico.yaml file from root to regular user space, and apply the calico file with kubectl
```bash
cp /root/calico.yaml .
kubectl apply -f calico.yaml
```
Apply [kubectl autocompletion ](https://kubernetes.io/docs/reference/kubectl/cheatsheet/#kubectl-autocomplete)
```bash
source <(kubectl completion bash) # setup autocomplete in bash into the current shell, bash-completion package should be installed first.
echo "source <(kubectl completion bash)" >> ~/.bashrc # add autocomplete permanently to your bash shell.
```


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
