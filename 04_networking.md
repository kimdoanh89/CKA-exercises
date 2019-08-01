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


