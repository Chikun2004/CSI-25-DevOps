
#  Kubernetes Cluster Setup using kubeadm (Step-by-Step)

##  Prerequisites

- 2 or more Linux machines (VMs or physical) – 1 master (control-plane), 1 or more workers
- OS: Ubuntu 20.04+ (or similar)
- 2 GB+ RAM per machine
- Swap disabled
- Internet connectivity
- Root/sudo access

---

##  Step 1: Set Hostnames

### On Master Node
```bash
hostnamectl set-hostname master-node
```

### On Worker Node(s)
```bash
hostnamectl set-hostname worker-node
```

---

##  Step 2: Update and Install Dependencies

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl
```

---

##  Step 3: Disable Swap

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

---

##  Step 4: Configure sysctl

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system
```

---

##  Step 5: Install Container Runtime (containerd)

```bash
sudo apt install -y containerd
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd
```

---

##  Step 6: Install Kubernetes Tools

```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

---

##  Step 7: Initialize Kubernetes on Master Node

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

Copy and save the `kubeadm join` command for worker nodes.

---

##  Step 8: Configure kubectl on Master Node

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

##  Step 9: Deploy Pod Network (e.g., Calico)

```bash
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.26.1/manifests/calico.yaml
```

---

##  Step 10: Join Worker Node(s) to Cluster

Use the `kubeadm join` command from step 7:

```bash
sudo kubeadm join <master-ip>:6443 --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

---

##  Step 11: Verify the Cluster

On master node:
```bash
kubectl get nodes
kubectl get pods --all-namespaces
```

---

##  Optional Useful Commands

- Drain a node:
```bash
kubectl drain <node-name> --ignore-daemonsets
```

- Remove a node:
```bash
kubectl delete node <node-name>
```

- Reset cluster (on node):
```bash
sudo kubeadm reset
sudo rm -rf ~/.kube
```

---

> You have successfully set up a Kubernetes cluster using kubeadm!
