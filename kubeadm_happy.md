# Kubernetes Cluster Setup Guide

## 1. Overview

This guide describes the steps to set up a Kubernetes cluster using either a self-managed or provider-managed approach.

### Self-Managed Cluster:
- **Multi Node Cluster**: Using `kubeadm`
- **Single Node Cluster**: Using `Minikube`

### Provider Managed Cluster:
- **AWS**: Using EKS
- **Azure**: Using AKS
- **GCP**: Using GKE

---

## 2. Setup EC2 Instances

### EC2 Instance Details:
- **Type**: t2.micro
- **Security Group**:
  - SSH
  - All traffic (Ports 0-65535)

---

## 3. Execute On All Nodes

### 1. Update Packages
```bash
apt update -y
2. Set Hostname for Each Machine
bash
Copy code
sudo hostnamectl set-hostname "master-node"
exec bash
3. Install Docker
bash
Copy code
apt install docker.io -y
4. Install Containerd
Load Required Modules for Networking
bash
Copy code
sudo modprobe overlay
sudo modprobe br_netfilter
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF
Configure Iptables for Kubernetes
bash
Copy code
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

sudo sysctl --system
Install Curl
bash
Copy code
sudo apt install curl -y
Add Docker Repository and Install containerd
bash
Copy code
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt update -y
sudo apt install -y containerd.io
5. Set Up Containerd Configuration
bash
Copy code
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
Modify Containerd Configuration File
bash
Copy code
sudo vi /etc/containerd/config.toml
# Uncomment the following line:
# SystemdCgroup = true
6. Restart containerd
bash
Copy code
sudo systemctl restart containerd
Verify containerd is running
bash
Copy code
ps -ef | grep containerd
7. Install Kubernetes Tools
Add Kubernetes Repository
bash
Copy code
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
sudo mkdir -p -m 755 /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
8. Disable Swap
bash
Copy code
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
9. Enable Kubelet Service
bash
Copy code
sudo systemctl enable kubelet
4. Execute On Master Node
1. Pull Required Kubernetes Images
bash
Copy code
sudo kubeadm config images pull
2. Initialize Kubernetes Cluster
bash
Copy code
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --ignore-preflight-errors=NumCPU --ignore-preflight-errors=Mem
3. Set Up kubeconfig for Regular User
bash
Copy code
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
Alternatively, if you are root, run:

bash
Copy code
export KUBECONFIG=/etc/kubernetes/admin.conf
4. Deploy Pod Network (Flannel)
bash
Copy code
kubectl apply -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml
5. Display Join Token
bash
Copy code
kubeadm token create --print-join-command
5. End User Machine
1. Install kubectl
bash
Copy code
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
mv ./kubectl /usr/local/bin/kubectl
kubectl version
2. Set Up kubeconfig
bash
Copy code
mkdir ~/.kube
vi ~/.kube/config
3. Copy kubeconfig from Master Node
bash
Copy code
cat /etc/kubernetes/admin.conf
6. Conclusion
You should now have a Kubernetes cluster set up with a master node and any number of worker nodes. You can start deploying applications and managing the cluster with kubectl.

csharp
Copy code

This Markdown file includes all the steps with necessary commands to set up Kubernetes on a multi-node EC2 setup. Simply copy and paste this content into a `.md` file for your documentation.


