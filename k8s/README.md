# Kubernetes Deployment

## Overview

This project demonstrates a full-stack application deployed on a Kubernetes cluster. It includes all key components of a modern cloud-native application:

- **Database**: MySQL with persistent storage
- **Cache**: Memcached
- **Message Broker**: RabbitMQ
- **Application Server**: Java Web App (Tomcat)
- **Frontend**: NGINX
- **Ingress**: NGINX Ingress Controller for external traffic
- **Secrets & Configs**: Secure credentials and environment configuration

This setup provides a complete example of multi-tier application deployment in Kubernetes.

## Architecture

Components and Interactions:

![Architecture Diagram](https://github.com/user-attachments/assets/f83395c9-17c6-4325-8e30-8e8d4ba70ceb)

- **Secrets**: `secret.yaml` stores DB and RabbitMQ credentials securely.
- **PersistentVolumeClaim (PVC)**: Ensures MySQL data persists.
- **ClusterIP Services**: Allow internal communication.
- **Ingress**: Handles routing external traffic to the application.

## Deployment Steps

1. **Create Secrets**
   ```
   kubectl apply -f secret.yaml
   ```

2. **Deploy Database**
   ```
   kubectl apply -f dbdeploy.yaml
   kubectl apply -f dbservice.yaml
   ```

3. **Deploy Cache (Memcached)**
   ```
   kubectl apply -f mcdep.yaml
   kubectl apply -f mcservice.yaml
   ```

4. **Deploy Message Broker (RabbitMQ)**
   ```
   kubectl apply -f rmdeploy.yaml
   kubectl apply -f rmqservice.yaml
   ```

5. **Deploy Application (Tomcat)**
   ```
   kubectl apply -f appdeploy.yaml
   kubectl apply -f appservice.yaml
   ```

6. **Deploy Ingress**
   ```
   kubectl apply -f appingress.yaml
   ```

## Verify Deployment

Check all resources:
```
kubectl get all
```

Check Pods logs:
```
kubectl logs <pod-name>
```

Access the application via browser.

### Internal Connections (used by the app):
- MySQL: db-service:3306
- Memcached: mc-service:11211
- RabbitMQ: rmq-service:5672

## Files Description

- `secret.yaml`: Kubernetes Secret for storing sensitive data like database and RabbitMQ credentials.
- `dbdeploy.yaml`: Deployment manifest for MySQL database.
- `dbservice.yaml`: Service manifest for MySQL database.
- `mcdep.yaml`: Deployment manifest for Memcached cache.
- `mcservice.yaml`: Service manifest for Memcached cache.
- `rmdeploy.yaml`: Deployment manifest for RabbitMQ message broker.
- `rmqservice.yaml`: Service manifest for RabbitMQ message broker.
- `appdeploy.yaml`: Deployment manifest for the Java web application (Tomcat).
- `appservice.yaml`: Service manifest for the Java web application.
- `appingress.yaml`: Ingress manifest for routing external traffic to the application.
