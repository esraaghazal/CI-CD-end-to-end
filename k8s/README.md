# Kubernetes Deployment
Overview

This project demonstrates a full-stack VProfile application deployed on a Kubernetes cluster. It includes all key components of a modern cloud-native application:

Database: MySQL with persistent storage

Cache: Memcached

Message Broker: RabbitMQ

Application Server: Java Web App (Tomcat)

Frontend: NGINX

Ingress: NGINX Ingress Controller for external traffic

Secrets & Configs: Secure credentials and environment configuration

This setup provides a complete example of multi-tier application deployment in Kubernetes.

# Architecture

Components and Interactions:

<img width="1065" height="606" alt="image" src="https://github.com/user-attachments/assets/a94aa257-b026-425b-ad51-0dc0baab9682" />




Secrets: app-secret stores DB and RabbitMQ credentials securely.

PersistentVolumeClaim (PVC) ensures MySQL data persists.

ClusterIP Services allow internal communication.

Ingress handles routing external traffic to the application.

# Deployment Steps

1. Create Secrets
  kubectl apply -f k8s/secrets.yaml
2. Deploy Database
  kubectl apply -f k8s/vprodb-deployment.yaml
  kubectl apply -f k8s/vprodb-service.yaml
3. Deploy Cache (Memcached)
  kubectl apply -f k8s/vpromc-deployment.yaml
  kubectl apply -f k8s/vprocache-service.yaml
4. Deploy Message Broker (RabbitMQ)
  kubectl apply -f k8s/vpromq01-deployment.yaml
  kubectl apply -f k8s/vpromq01-service.yaml
5. Deploy Application (Tomcat)
  kubectl apply -f k8s/vproapp-deployment.yaml
  kubectl apply -f k8s/vproapp-service.yaml
6. Deploy Frontend (NGINX)
  kubectl apply -f k8s/vproweb-deployment.yaml
  kubectl apply -f k8s/vproweb-service.yaml
7. Deploy Ingress
  kubectl apply -f k8s/ingress.yaml

# Verify Deployment

Check all resources:

  kubectl get all

Check Pods logs:

  kubectl logs <pod-name>

Access the application via browser :



Internal connections (used by the app):

MySQL: vprodb:3306

Memcached: vprocache01:11211

RabbitMQ: vpromq01:5672
