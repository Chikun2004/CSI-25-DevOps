#  Configuring Taints and Tolerations in Kubernetes

## 1.  Core Concepts

- **Taints** are set on **nodes** to repel pods that **don't tolerate** them.
- **Tolerations** are added to **pods** to allow scheduling onto tainted nodes :contentReference[oaicite:1]{index=1}.
- Common effects:
  - `NoSchedule`: Blocks new pods unless tolerated  
  - `PreferNoSchedule`: Soft blocking  
  - `NoExecute`: Evicts existing pods without toleration :contentReference[oaicite:2]{index=2}

---

## 2.  Add a Taint to a Node

```bash
kubectl taint nodes <node-name> key=value:NoSchedule
Example:
kubectl taint nodes gpu-node gpu=true:NoSchedule
# → node/gpu-node tainted
This prevents scheduling on gpu-node unless pods tolerate gpu=true:NoSchedule .

3.  Define Tolerations in Pod
Pods need a matching toleration to run on the tainted node.
apiVersion: v1
kind: Pod
metadata:
  name: gpu-pod
spec:
  containers:
    - name: app
      image: nginx
  tolerations:
    - key: "gpu"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
Alternatively, using Exists operator:
tolerations:
  - key: "gpu"
    operator: "Exists"
    effect: "NoSchedule"
    
4.  Apply and Verify
kubectl taint nodes gpu-node gpu=true:NoSchedule
kubectl apply -f gpu-pod.yaml
kubectl get pods -o wide

You should now see gpu-pod running on gpu-node.

5.  Use Multiple Taints & Tolerations
If a node has several taints:
kubectl taint nodes nodeX key1=val1:NoSchedule key2=val2:NoExecute

Your pod must include matching tolerations for each taint to avoid scheduling or eviction 

.

6.  Remove a Taint
kubectl taint nodes gpu-node gpu=true:NoSchedule-

This removes the taint, allowing normal scheduling.

7.  Best Practices
Use taints to isolate nodes (e.g., GPU nodes, spot instances)
Always pair with nodeSelector/nodeAffinity if you want pods only on specific node types 
Avoid over-tainting—may lead to pods stuck in Pending state
Use NoExecute cautiously; it can evict running pods 

 Next Steps
Use taints to reserve infrastructure (e.g., GPUs, dedicated workloads)
Combine with node affinity for stronger placement control
Monitor your cluster to ensure efficiency and prevent Pending pods