## 1. Understand the networking configuration on the cluster nodes
Reference is [here](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/network/networking.md).

There are 4 distinct networking problems to solve:
- Highly-coupled container-to-container communications
- Pod-to-Pod communications
- Pod-to-Service communications
- External-to-internal communications
### Explore Kubernetes Environment
- What is the port the kube-scheduler is listening on in the master node? 
- Notice that ETCD is listening on two ports. Which of these have more client connections established? 
- Inspect the kubelet service and identify the network plugin configured for Kubernetes. 
- What is the path configured with all binaries of CNI supported plugins? 
- What is the CNI plugin configured to be used on this kubernetes cluster? 

<details><summary>show</summary><p>

```bash
netstat -nplt
netstat -anp | grep etcd 
netstat -anp | grep etcd | grep 2379 | wc -l
netstat -anp | grep etcd | grep 2380 | wc -l
ps -aux | grep kubelet
systemctl status kubelet.service
ls /etc/cni/net.d/
```


</p></details>

### Choosing different CNI networking solution
- Deploy weave-net networking solution to the cluster
- Deploy calico networking solution to the cluster
- Deploy flannel networking solution to the cluster

<details><summary>show</summary><p>

- Deploy weave-net. Documentation is [here](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/).
  ```bash
  kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"
  ```
- Deploy calico. Documentation is [here](https://docs.projectcalico.org/v3.8/getting-started/kubernetes/).
  ```bash
  kubectl apply -f https://docs.projectcalico.org/v3.8/manifests/calico.yaml
  ```
- Deploy flannel. Documentation is [here](https://github.com/coreos/flannel#deploying-flannel-manually).
  ```bash
  kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
  ```

</p></details>

## 2. Understand Pod networking concepts

<details><summary>show</summary><p>
  


</p></details>

## 3. Understand Service Networking

<details><summary>show</summary><p>
  


</p></details>

## 4. Deploy and configure network load balancer

<details><summary>show</summary><p>
  


</p></details>

## 5. Know how to use Ingress rules

<details><summary>show</summary><p>
  


</p></details>

## 6. Know how to configure and use the cluster DNS

<details><summary>show</summary><p>
  


</p></details>

## 7. Understand CNI

<details><summary>show</summary><p>
  


</p></details>


