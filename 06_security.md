## 1. Know how to configure authentication and authorization
### Authentication and authorization
- Create two namespaces, one for production and the other for development.
- Create a new user DevDan and assign a password of lfs258.
- Generate a private key then Certificate Signing Request (CSR) for DevDan.
- Using the newly created request, generate a self-signed certificate using the x509 protocol. Use the CA keys for the
Kubernetes cluster and set a 45 day expiration.
- Update the access config file to add user DevDan and reference the new key and certificate (using set-credentials)
- Create context name DevDan-context with cluster kubernetes, namespace development, and user DevDan
- Create a role name Developer in Development namespace:
  - apiGroups: "", "extensions", "apps"
  - resources: "deployments" , "replicasets" , "pods"
  - verbs: ["*"]
- Create a rolebinding name developer-role-binding in Development namespace: role developer and user DevDan
- In DevDan-context, Create a new deployment nginx with image nginx, verify it exists, then delete it 
- Create context name ProdDan-context with cluster kubernetes, namespace production, and user DevDan

## 2. Understand Kubernetes security primitives
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








## 5. Work with images securely


## 6. Define security contexts
## 7. Secure persistent key value store
