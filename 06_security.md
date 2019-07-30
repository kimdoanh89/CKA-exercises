## 1. Know how to configure authentication and authorization
### Authentication and authorization
- Create two namespaces, one for production and the other for development.
- Create a new user DevDan and assign a password of lfs258.
- Generate a private key then Certificate Signing Request (CSR) for DevDan.
- Using the newly created request, generate a self-signed certificate using the x509 protocol. Use the CA keys for the
Kubernetes cluster and set a 45 day expiration.
- Update the access config file to add user DevDan and reference the new key and certificate (using set-credentials)
- Create context name DevDan-context with cluster kubernetes, namespace development, and user DevDan
- Create a role name Developer in development namespace:
  - apiGroups: "", "extensions", "apps"
  - resources: "deployments" , "replicasets" , "pods"
  - verbs: ["*"]
- Create a rolebinding name developer-role-binding in development namespace: role developer and user DevDan
- In DevDan-context, Create a new deployment nginx with image nginx, verify it exists, then delete it 
- Create context name ProdDan-context with cluster kubernetes, namespace production, and user DevDan
- Create a role name dev-prod in production namespace:
  - apiGroups: "", "extensions", "apps"
  - resources: "deployments" , "replicasets" , "pods"
  - verbs: "get", "list", "watch"
- Create a rolebinding name production-role-binding in production namespace: role dev-prod and user DevDan
<details><summary>show</summary><p>
  


</p></details>

## 2. Understand Kubernetes security primitives
### Certificate API
- Create a new user DevTrang with password lfs258 (list all users with cat /etc/passwd)
- Create DevTrang.key and DevTrang.csr with openssl.
- Create a Certificate signing request (csr) object from DevTrang.csr to send to the Kubernetes API ([here](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/#create-a-certificate-signing-request-object-to-send-to-the-kubernetes-api)).
- Get csr and approve it.
<details><summary>show</summary><p>
  


</p></details>

### Kubeconfig

<details><summary>show</summary><p>
  


</p></details>

### Roles, Role bindings, cluster roles and cluster role bindings



## 3. Know how to configure network policies
## 4. Create and manage TLS certificates for cluster components

### View the certificates
- Identify the certificate file used for the kube-api server.
- Identify the Certificate file used to authenticate kube-apiserver as a client to ETCD Server.
- Identify the key used to authenticate kubeapi-server to the kubelet server.
- Identify the ETCD Server Certificate used to host ETCD server.
- Identify the ETCD Server CA Root Certificate used to serve ETCD Server (ETCD can have its own CA. So this may be a different CA certificate than the one used by kube-api server).
- What is the Common Name (CN) configured on the Kube API Server Certificate? (OpenSSL Syntax: openssl x509 -in file-path.crt -text -noout).
- Which of the below alternate names is not configured on the Kube API Server Certificate?
- How long, from the issued date, is the Root CA Certificate valid for? (File: /etc/kubernetes/pki/ca.crt)
<details><summary>show</summary><p>
  


</p></details>







## 5. Work with images securely
Reference is [here](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/).
### Image security
- Create a deployment name web with image nginx:alpine, scale to replicas=2
- We decided to use a modified version of the application from an internal private registry. Update the image of the deployment to use a new image from myprivateregistry.com:5000
- Create a secret object with the credentials required to access the registry:
  - Name: private-reg-cred 
  - Username: dock_user 
  - Password: dock_password 
  - Server: myprivateregistry.com:5000 
  - Email: dock_user@myprivateregistry.com
- Configure the deployment to use credentials from the new secret to pull images from the private registry
<details><summary>show</summary><p>
 
- Create and scale deployment 
  ```bash
  kubectl create deployment web --image=nginx:alpine
  kubectl scale deployment web --replicas=2
  kubectl edit deployment web
  ```
- Update deployment using new private registry
  ```yaml    
    template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - image: myprivateregistry.com:5000/nginx:alpine
  ```
- Create a secret object:
  ```bash
  kubectl create secret docker-registry private-reg-cred --docker-server=myprivateregistry.com:5000 \
          --docker-username=dock_user --docker-password=dock_password --docker-email=dock_user@myprivateregistry.com
  ```
- Configure the deployment to use credentials from the new secret:
  ```bash
  kubectl edit deployment web
  ```
  ```yaml
    template:
    metadata:
      labels:
        app: web
    spec:
      imagePullSecrets:
      - name: private-reg-cred
      containers:
      - image: myprivateregistry.com:5000/nginx:alpine
  ```

</p></details>

## 6. Define security contexts
Reference is [here](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/).

**Note**: The User ID defined in the securityContext of the container overrides the User ID in the POD.
### Security Context
- Create a pod 'ubuntu-sleeper' with image 'ubuntu', command 'sleep 4800'
- Check who run the container?
- Edit the pod 'ubuntu-sleeper' to run the sleep process with user ID 1010.
- 

## 7. Secure persistent key value store
