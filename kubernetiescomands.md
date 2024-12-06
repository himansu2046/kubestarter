
Kubernetes Commands Cheat Sheet
Cluster Management
kind create cluster --config kind-cluster-config.yaml --name my-kind-cluster
Creates a Kubernetes cluster using Kind (Kubernetes in Docker) with a specified configuration.

kind get clusters
Lists all the clusters managed by Kind.

kind delete cluster --name my-kind-cluster
Deletes the specified Kubernetes cluster.

Navigating and Viewing Resources
ls
Lists the contents of the current directory.

cd
Changes the current directory.

kubectl get nodes
Displays the list of nodes in the cluster.

kubectl get pods
Lists all pods in the current namespace.

kubectl get pods -o wide
Provides additional details about pods, including their node and IP.

kubectl get pod <pod-name> -o yaml
Displays the detailed YAML configuration of a specific pod.

Pod Management
kubectl run nginx --image=nginx
Deploys a pod named nginx using the official Nginx image.

kubectl run nodeapp --image=himansu2046/node-app
Deploys a pod named nodeapp using the specified image.

kubectl run jenkins --image=jenkins:2.60.3
Deploys a Jenkins pod using the specified Jenkins image.

kubectl delete pod <pod-names>
Deletes the specified pods.

Logs and Debugging
kubectl logs -c nginx
Displays logs for the container nginx in the specified pod.

kubectl describe pod/jenkins
Provides detailed information about the jenkins pod, including events and status.

Namespace Management
kubectl get ns
Lists all namespaces in the cluster.

kubectl create ns dev
Creates a new namespace called dev.

YAML Configuration
vim nginx.yaml
Opens the nginx.yaml file in Vim for editing.

kubectl apply -f nginx.yaml
Applies the configuration in nginx.yaml to the cluster.

cat nginx.yaml
Displays the contents of nginx.yaml.

Miscellaneous
docker ps
Lists running Docker containers.

apt install vim
Installs the Vim text editor.

poweroff
Powers off the system.
