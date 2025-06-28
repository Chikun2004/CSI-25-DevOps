#  Configuring Horizontal Pod Autoscaling (HPA) in AKS

##  What is HPA?
The **Horizontal Pod Autoscaler (HPA)** automatically adjusts the number of pod replicas in a deployment based on observed metrics—commonly CPU utilization—between a specified **minReplicas** and **maxReplicas** :contentReference[oaicite:1]{index=1}.

---

##  Prerequisites

- AKS cluster with Kubernetes ≥ 1.10
- `kubectl` and `az` CLIs installed & configured
- **Metrics Server** deployed (native in AKS ≥ 1.10) :contentReference[oaicite:2]{index=2}
- Your deployment defines CPU **requests** (required for CPU-based scaling) :contentReference[oaicite:3]{index=3}

---

##  1. Define a Deployment with CPU Requests

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hpa-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hpa-demo
  template:
    metadata:
      labels:
        app: hpa-demo
    spec:
      containers:
      - name: demo
        image: nginx
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 100m
          limits:
            cpu: 200m
            
 2. Create an HPA Resource
Using imperative CLI:
kubectl autoscale deployment hpa-demo \
  --cpu-percent=50 \
  --min=1 \
  --max=10
Or declarative manifest (hpa.yaml):
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: hpa-demo
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: hpa-demo
  minReplicas: 1
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
        
(Use v2beta2 to support richer metrics and behaviors) 
Apply the manifest:
kubectl apply -f hpa.yaml

 3. Check HPA Status
kubectl get hpa
kubectl describe hpa hpa-demo
You'll see current CPU usage, replica count, and scaling events 

 4. Test Autoscaling
Generate load to trigger the HPA:
kubectl run -i --tty load-generator \
  --image=busybox \
  -- sh -c "while true; do wget -q -O- http://hpa-demo; done"
  
Monitor:
kubectl get pods --watch
kubectl get hpa
You'll see the replicas increase when CPU utilization goes above 50% and scale down when it falls 

 5. Understand Cooldown & Behavior
HPA polls metrics every ~60 s (default), while Metrics Server updates every ~60 s 
Scale-down delay default is ~5 min to prevent rapid oscillation; no cooldown on scale-up 

 6. Optional: Cluster Autoscaler
To accommodate more pods, use Cluster Autoscaler to scale nodes automatically. HPA + Cluster Autoscaler work together 

##  Clean Up
kubectl delete hpa hpa-demo
kubectl delete deployment hpa-demo
kubectl delete svc hpa-demo` 

---
##  Next Steps

-   Explore custom or external metrics
    
-   Tune **behavior** using `v2beta2` API (e.g., scaleUp/scaleDown policies)
    
-   Combine with **node auto-provisioning** or **ACI virtual nodes** for burst scaling