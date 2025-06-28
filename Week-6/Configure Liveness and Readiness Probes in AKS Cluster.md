# Configure Liveness and Readiness Probes in AKS Cluster

##  Introduction
In Kubernetes, **liveness probes** detect if an application is running, and **readiness probes** check if it's ready to serve traffic. Proper probe configuration helps ensure high availability and proper rollout of applications.

---

##  Prerequisites

- Azure CLI (`az`) installed
- Kubernetes CLI (`kubectl`) installed
- AKS cluster configured and running
- `kubectl get nodes` shows active nodes

---

## 1.  Sample Application Deployment with Probes

###  Create a Deployment YAML (`probes-deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: probe-demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: probe-app
  template:
    metadata:
      labels:
        app: probe-app
    spec:
      containers:
      - name: probe-container
        image: nginx
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 5
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 5
2.  Apply the Deployment
      kubectl apply -f probes-deployment.yaml
3.  Verify the Probes
# Check pod status
kubectl get pods

# Describe the pod to see probe status
kubectl describe pod <pod-name>

4.  Testing Failure Scenarios
 Force liveness failure:
Modify the deployment to point livenessProbe path to /fail:
livenessProbe:
  httpGet:
    path: /fail
    port: 80
Then re-apply and observe the pod restarting:
   kubectl apply -f probes-deployment.yaml
   kubectl describe pod <pod-name>
5.  Clean Up
   kubectl delete -f probes-deployment.yaml
   
 Best Practices
Probe Type	Purpose	Restart Pod?
Liveness Probe	Check if app is alive:	 Yes
Readiness Probe	Check if app is ready to serve	: No

 Summary
Liveness Probe restarts containers when they fail health checks.
Readiness Probe ensures traffic is sent only to ready pods.
Use kubectl describe pod to debug probe issues.