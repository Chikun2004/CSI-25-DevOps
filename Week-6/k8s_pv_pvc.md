
# Kubernetes PersistentVolume (PV) and PersistentVolumeClaim (PVC)

##  Introduction
In Kubernetes, **PersistentVolume (PV)** and **PersistentVolumeClaim (PVC)** are used to manage storage resources. They decouple storage provisioning from usage, allowing dynamic or static volume management.

---

## 1.  PersistentVolume (PV)
###  What is a PV?
- A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes.
- PV is a cluster resource, just like a node.

###  Example: Static PersistentVolume YAML
```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
  persistentVolumeReclaimPolicy: Retain
```

---

## 2.  PersistentVolumeClaim (PVC)
###  What is a PVC?
- A request for storage by a user.
- Specifies size, access modes, and optionally storage class.

###  Example: PersistentVolumeClaim YAML
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

---

## 3.  Pod Using PVC
###  Example Pod that uses the PVC
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-using-pvc
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: my-storage
  volumes:
    - name: my-storage
      persistentVolumeClaim:
        claimName: my-pvc
```

---

## 4.  Commands to Apply and Verify

###  Step-by-Step:
```bash
# Create PV
kubectl apply -f pv.yaml

# Create PVC
kubectl apply -f pvc.yaml

# Create Pod
kubectl apply -f pod.yaml

# Verify PV and PVC
kubectl get pv
kubectl get pvc
kubectl describe pvc my-pvc
```

---

## 5.  Reclaim Policy
- **Retain**: Manual cleanup.
- **Recycle**: Basic scrub (deprecated).
- **Delete**: Automatically deletes associated storage resource.

---

##  Summary Table

| Component | Description                            |
|-----------|----------------------------------------|
| PV        | Physical storage on the cluster        |
| PVC       | User's request for storage             |
| Reclaim   | What happens when PVC is deleted       |

---

##  Tips
- PVCs abstract underlying storage for portability.
- Use StorageClass for dynamic provisioning.
- Always check `accessModes` and `reclaimPolicy`.

