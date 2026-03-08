# VProfile Application Deployment on Kubernetes

## Project Overview

This project demonstrates how to deploy the **VProfile application** on a Kubernetes cluster using Kubernetes manifests.
The deployment includes multiple microservices such as **database, caching service, messaging service, and the main application**.

The goal of this project is to practice container orchestration concepts and deploy a multi-tier application using **Kubernetes Deployments, Services, and Secrets**.

---

# Architecture

The application consists of the following components:

* **VProfile Application** – Main web application
* **MySQL Database** – Stores application data
* **Memcached** – Used for caching
* **RabbitMQ** – Messaging service for asynchronous communication

  <img width="1019" height="708" alt="image" src="https://github.com/user-attachments/assets/6127d28e-6289-4d5e-bd3e-e0d97bd25406" />


All components communicate internally inside the Kubernetes cluster using **ClusterIP services**, while the application is exposed externally using a **LoadBalancer service**.

---

# Kubernetes Resources Used

## 1. Secret

A Kubernetes **Secret** is used to securely store sensitive information such as passwords.

Example values stored:

* MySQL root password
* RabbitMQ password

The passwords are stored in **Base64 encoded format**.

---

## 2. Deployments

Deployments are used to manage and run the application containers.

Deployments created in this project:

* `vprodb` → MySQL database container
* `vpromc` → Memcached container
* `vpromq01` → RabbitMQ container
* `vproapp` → Main VProfile application

Each deployment ensures that the specified number of pods are running and automatically recreates pods if they fail.

---

## 3. Services

Kubernetes **Services** are used to allow communication between pods.

Services created in this project:

| Service Name    | Purpose            | Port  |
| --------------- | ------------------ | ----- |
| vprodb          | Database service   | 3306  |
| vprocache01     | Memcached service  | 11211 |
| vpromq01        | RabbitMQ service   | 5672  |
| vproapp-service | Application access | 80    |

Internal services use **ClusterIP** for internal communication inside the cluster.

The application service uses **LoadBalancer** to expose the application externally.

---

# Init Containers

The application deployment uses **initContainers** to ensure that required services are available before starting the main application container.

The init containers wait for:

* Database service
* Memcached service

This prevents the application from starting before its dependencies are ready.

---

# Container Images Used

| Component      | Image                |
| -------------- | -------------------- |
| Application    | vprofile/vprofileapp |
| Database       | vprofile/vprofiledb  |
| Memcached      | memcached            |
| RabbitMQ       | rabbitmq             |
| Init Container | busybox              |

---

# How to Deploy

Apply the Kubernetes manifests using the following command:

```bash
kubectl apply -f .
```

Verify that all pods are running:

```bash
kubectl get pods
```

Check services:

```bash
kubectl get svc
```

---

# Accessing the Application

Once the **LoadBalancer service** is created, get the external IP using:

```bash
kubectl get svc vproapp-service
```

Open the application in the browser using:

```
http://<EXTERNAL-IP>
```

---

# Key Kubernetes Concepts Practiced

* Kubernetes Deployments
* Kubernetes Services
* Secrets Management
* Init Containers
* Multi-tier Application Deployment
* Internal Service Communication
* LoadBalancer Exposure

---

# Project Purpose

This project is part of my **DevOps learning journey**, where I practiced deploying a multi-tier application on Kubernetes and managing dependencies between services.

---
