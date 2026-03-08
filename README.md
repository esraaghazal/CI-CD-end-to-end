# CI/CD Pipeline for Kubernetes Deployment using Jenkins with Monitoring
Project Overview

This project demonstrates a complete End-to-End CI/CD pipeline for deploying an application to a Kubernetes cluster using Jenkins, Docker, and Helm, with Prometheus and Grafana for monitoring.

The pipeline automates the full software delivery lifecycle starting from code commit by the developer until deployment and monitoring inside the Kubernetes cluster.

The project integrates the following tools:

Docker for containerization

Jenkins for CI/CD automation

Kubernetes for container orchestration

Helm for Kubernetes package management

Prometheus for metrics collection

Grafana for metrics visualization

# Project Architecture

The following architecture represents the CI/CD workflow used in this project.

Pipeline Flow:

Developer → GitHub → Jenkins Pipeline → Docker Build → Docker Hub → Helm Deployment → Kubernetes Cluster → Prometheus & Grafana Monitoring

<img width="1644" height="418" alt="image" src="https://github.com/user-attachments/assets/757cb954-859b-4681-b7bd-f1ffa1a4e0f0" />




<img width="1115" height="581" alt="image" src="https://github.com/user-attachments/assets/71726387-e695-4b83-9ad5-1417b025ea6b" />



# Kubernetes Cluster Setup (kubeadm)

The Kubernetes cluster is created using kubeadm and contains:

Node	Role
k8smaster	Master Node
k8sworker1	Worker Node
k8sworker2	Worker Node

# Monitoring Kubernetes Cluster

The Kubernetes cluster is monitored using Prometheus and Grafana, both deployed using Helm charts.

 Install Prometheus using Helm
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

Install Prometheus stack

helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring

Prometheus collects metrics from:

Cluster components (Kubernetes Nodes Pods Services)

Install Grafana

Grafana is included in the kube-prometheus-stack Helm chart.

Access Grafana using port forwarding:

kubectl port-forward svc/prometheus-grafana 3000:80

Open in browser:

http://localhost:3000

Default login:

username: admin
password: admin
Metrics Monitored

Prometheus collects metrics and Grafana visualizes them in dashboards.


k8smaster	Master Node
k8sworker1	Worker Node
k8sworker2	Worker Node
