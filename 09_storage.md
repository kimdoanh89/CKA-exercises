## 1. Understand persistent volumes and know how to create them
## 2. Understand access modes for volumes
## 3. Understand persistent volume claims primitive
## 4. Understand Kubernetes storage objects
## 5. Know how to configure applications with persistent storage


Create a 'Persistent Volume' with the given specification.
- Volume Name: pv-log
- Storage: 100Mi
- Access modes: ReadWriteMany
- Host Path: /pv/log

Create a 'Persistent Volume Claim' with the given specification.
- Volume Name: claim-log-1
- Storage Request: 50Mi
- Access modes: ReadWriteOnce
