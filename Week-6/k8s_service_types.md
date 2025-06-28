# Kubernetes Service Types: ClusterIP, NodePort, and LoadBalancer

##  What is a Kubernetes Service?
A **Kubernetes Service** is an abstraction that defines a logical set of Pods and a policy by which to access them. Services enable communication between different components of your application or expose it externally.

---

## 1. 🔹 ClusterIP (Default)
###  Description:
- Exposes the service on an internal IP in the cluster.
- Only reachable from within the cluster.
- Default service type.

### 🛠️ Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

###  How to Test:
- Run a `kubectl exec` into a pod and use `curl` to the service name.

---

## 2.  NodePort
###  Description:
- Exposes the service on each Node’s IP at a static port (30000-32767).
- Accessible from outside the cluster via `<NodeIP>:<NodePort>`.

###  Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30080
```

###  How to Test:
- Access via `http://<NodeIP>:30080` in browser or `curl`.

---

## 3.  LoadBalancer
###  Description:
- Exposes the service externally using a cloud provider’s load balancer.
- Works only in supported environments (like AWS, Azure, GCP).

###  Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

###  How to Test:
- Check the external IP using:
```bash
kubectl get svc my-loadbalancer-service
```
- Then access via `http://<external-ip>`.

---
