# CI-CD-end-to-end

CI/CD Pipeline for Kubernetes Deployment using Jenkins and monitoring using Prometheus and Grafana

## Project Overview

This project demonstrates how to build a complete CI/CD pipeline that integrates:

 Docker for containerization

 Kubernetes for container orchestration

 Jenkins for CI/CD automation

 Prometheus and Grafana for monitoring

The pipeline automatically:

Clones the source code

Builds a Docker image

Pushes the image to Docker Hub

Deploys the application to a Kubernetes cluster

# Project Architecture
Developer → GitHub → Jenkins Pipeline → Docker Build & Push → Kubernetes Cluster → Prometheus & Grafana

# Kubernetes Cluster Setup (kubeadm)

Cluster setup includes:

1 Master Node (k8smaster)

2 Worker Nodes (k8sworker1, k8sworker2)

1️⃣ Set Hostnames

2️⃣ Assign Static IP

nmcli con mod "connection name" ipv4.method manual \
ipv4.addresses 192.168.x.x/24 \
ipv4.gateway 192.168.x.x \
ipv4.dns "192.168.2.254 8.8.8.8 8.8.4.4"

nmcli con down "connection name"
nmcli con up "connection name"

3️⃣ Update /etc/hosts

192.168.0.xxx k8smaster
192.168.0.xxx k8sworker1
192.168.0.xxx k8sworker2

4️⃣ Disable SELinux

setenforce 0
sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux

5️⃣ Disable Firewall & Configure Networking

systemctl disable firewalld

cat <<EOF | tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

sysctl --system

6️⃣ Install Kubernetes & Docker

yum install kubeadm docker -y

systemctl enable kubelet
systemctl start kubelet

systemctl enable docker
systemctl start docker

7️⃣ Disable Swap

swapoff -a

8️⃣ Initialize Kubernetes Cluster

kubeadm init

9️⃣ Install Pod Network (Calico)

kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

🔟 Join Worker Nodes

Run the kubeadm join command generated from the master node on each worker node.

 # Run Jenkins in Docker
docker run -u 0 --privileged \
--name jenkins \
-it -d \
-p 8080:8080 \
-p 50000:50000 \
-v /var/run/docker.sock:/var/run/docker.sock \
-v $(which docker):/usr/bin/docker \
-v /home/jenkins_home:/var/jenkins_home \
jenkins/jenkins:latest

 # Application Deployment

Jenkins pipeline deploys a Node.js application:

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
# Jenkins Pipeline Stages

Clone Source Code – Pull code from GitHub

Build Docker Image – docker.build("username/nodeapp")

Login to Docker Hub – Using Jenkins stored credentials

Push Image – Push built image to Docker Hub

Deploy to Kubernetes – Apply deployment & service manifests

# Monitoring Kubernetes Cluster

Monitor cluster health and application metrics using Prometheus and Grafana.

1️⃣ Install Prometheus

Use Prometheus  Helm chart to deploy Prometheus to your cluster.

Prometheus scrapes metrics from nodes, pods, and services.

kubectl apply -f prometheus-deployment.yaml

2️⃣ Install Grafana

Deploy Grafana in the cluster and connect it to Prometheus as a data source.

kubectl apply -f grafana-deployment.yaml

3️⃣ Access Grafana

Forward Grafana port:

kubectl port-forward svc/grafana 3000:3000

Login (default: admin/admin) and configure dashboards.


4️⃣ Monitor Metrics

Node and Pod CPU/Memory usage

Kubernetes cluster health

Application response times

Prometheus collects metrics, and Grafana visualizes them in real-time dashboards.

# Credentials Management

Docker Hub credentials are securely stored inside Jenkins:

Jenkins → Manage Jenkins → Credentials

# Technologies Used

🐳 Docker

☸️ Kubernetes (kubeadm)

🔄 Jenkins

🟢 Java

📊 Prometheus

📈 Grafana

