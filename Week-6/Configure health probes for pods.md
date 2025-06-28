#  Configuring Health (Liveness & Readiness) Probes for Pods in AKS

## 1.  Why Health Probes Matter

- **Liveness probes** monitor if a container is alive. Kubernetes restarts the container when the probe fails.
- **Readiness probes** check if the container is ready to receive traffic. Failed readiness probes remove the pod from service endpoints without restarting it. :contentReference[oaicite:1]{index=1}

---

## 2.  Basic HTTP Probe Configuration in AKS

```yaml
readinessProbe:
  httpGet:
    path: /
    port: 80
  periodSeconds: 3
  timeoutSeconds: 1

livenessProbe:
  httpGet:
    path: /healthz
    port: 80
  periodSeconds: 10
  timeoutSeconds: 2
  
To apply this, create or update your Deployment:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 2
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myimage:latest
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
          periodSeconds: 3
          timeoutSeconds: 1
        livenessProbe:
          httpGet:
            path: /healthz
            port: 80
          periodSeconds: 10
          timeoutSeconds: 2
          
3.  Deploy & Check Configuration
kubectl apply -f myapp-deployment.yaml
kubectl get pods
kubectl describe pod <pod-name>

Look for Liveness and Readiness sections under Events and Container statuses to confirm presence and behavior.

4.  Insights for AKS Using Application Gateway Ingress
-AKS with Application Gateway Ingress Controller (AGIC) auto-provisions an HTTP GET probe when no custom probe is set.
-AGIC allows customizing probe paths and timeouts via pod spec—ensuring ingress accurately detects pod health. 
-Limitations: port must match the container's exposed port; certain fields like initialDelaySeconds and successThreshold are unsupported with AGIC. 

5.  Probe Customization Example
Here's a common production-grade setup:
livenessProbe:
  httpGet:
    path: /healthz
    port: 80
  initialDelaySeconds: 30
  periodSeconds: 10
  timeoutSeconds: 5
  failureThreshold: 3

readinessProbe:
  httpGet:
    path: /ready
    port: 80
  initialDelaySeconds: 10
  periodSeconds: 5
  timeoutSeconds: 2
  failureThreshold: 3
  
-initialDelaySeconds: Waits before starting probes.
-failureThreshold: Number of failed checks before marking failure. 

6.  Common Pitfalls & Tips
-Mismatch issues: Ensure the probe port and target container port match exactly. If they don’t, probes will fail silently. 
-AGIC restrictions: Some fields (like successThreshold) aren’t honored; stick to supported fields. 
-Startup probes: For slower-starting containers, use them to avoid premature liveness failures.

##  Next Steps

-   Confirm your probe works: use `kubectl describe pod` and check `/healthz`, `/ready`.
    
-   Integrate with ingress—AGIC picks up liveness/readiness hooks.
    
-   Consider adding **Startup probes** for slow-boot applications.