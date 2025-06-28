#  Create and Attach PersistentVolumeClaims (PVCs) to Pods in AKS

## 1.  Prerequisites

- An active AKS cluster with `kubectl` and `az` CLI configured
- A StorageClass available (e.g. `default`, `managed-premium`, or custom)
- `kubectl get storageclass` should list at least one

---

## 2.  Create a PersistentVolumeClaim (PVC)

Save the following as `azure-pvc.yaml`:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-managed-disk
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: managed-premium
  resources:
    requests:
      storage: 10Gi
      
Apply it:
kubectl apply -f azure-pvc.yaml

Verify it's bound:
kubectl get pvc azure-managed-disk

Output should show STATUS: Bound

3.  Define a Pod that Uses the PVC
Create azure-disk-pod.yaml:
apiVersion: v1
kind: Pod
metadata:
  name: disk-demo-pod
spec:
  containers:
  - name: app
    image: nginx
    volumeMounts:
    - mountPath: "/usr/share/nginx/html"
      name: azure-storage
  volumes:
  - name: azure-storage
    persistentVolumeClaim:
      claimName: azure-managed-disk
Apply it:
kubectl apply -f azure-disk-pod.yaml

Check pod status and PVC mount:
kubectl get pods
kubectl describe pod disk-demo-pod

You’ll see the volume is correctly mounted 

4.  Interact with the Mounted Volume
Access the pod:
kubectl exec -it disk-demo-pod -- /bin/bash
cd /usr/share/nginx/html
echo "Hello from AKS PVC!" > index.html
curl http://localhost
You should see your message served by NGINX 
thecloudblog.net

5.  Static Provisioning (Optional)
If you prefer manual PV provisioning:
Create Azure Disk (az disk create ...) 
Define a PersistentVolume with azureDisk spec
Create a matching PVC with volumeName pointing to your PV
Attach PVC to Pod (same as above)

6.  Cleanup
kubectl delete -f azure-disk-pod.yaml
kubectl delete -f azure-pvc.yaml

If using a static PV, don’t forget to delete it too using kubectl delete pv <pv-name> and via az disk delete if needed.