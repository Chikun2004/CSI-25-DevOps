
#  Kubernetes Cluster Setup using Minikube

##  Prerequisites
Ensure the following are installed:
- [VirtualBox](https://www.virtualbox.org/) or [Docker](https://docs.docker.com/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)

---

##  Step 1: Install Minikube

### For Windows (Using Chocolatey)
```bash
choco install minikube
```

### For Linux (Ubuntu/Debian)
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

### For macOS (Homebrew)
```bash
brew install minikube
```

---

##  Step 2: Start Minikube Cluster

### Using Docker Driver
```bash
minikube start --driver=docker
```

### Using VirtualBox
```bash
minikube start --driver=virtualbox
```

 This will take a few minutes.

---

##  Step 3: Verify the Cluster is Running
```bash
minikube status
kubectl get nodes
```

---

##  Step 4: Enable the Kubernetes Dashboard (Optional)
```bash
minikube dashboard
```
 This opens the Kubernetes dashboard in your browser.

---

##  Step 5: Access Services Running in the Cluster
Use the following to expose and view your service:
```bash
minikube service <service-name>
```

---

##  Step 6: Stop and Delete the Cluster

### Stop Minikube
```bash
minikube stop
```

### Delete Minikube Cluster
```bash
minikube delete
```

---

##  Additional Useful Commands

### SSH into Minikube VM
```bash
minikube ssh
```

### List Minikube Addons
```bash
minikube addons list
```

### Enable an Addon (e.g., metrics-server)
```bash
minikube addons enable metrics-server
```

---

##  Summary

| Task | Command |
|------|---------|
| Start Cluster | `minikube start` |
| Check Status | `minikube status` |
| Get Nodes | `kubectl get nodes` |
| Dashboard | `minikube dashboard` |
| Stop Cluster | `minikube stop` |
| Delete Cluster | `minikube delete` |

>  You now have a fully functioning Kubernetes cluster using Minikube!
