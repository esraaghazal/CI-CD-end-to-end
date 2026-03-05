# VProApp - Full Dockerized Multi-Tier Architecture

##  Project Overview

This project demonstrates a production-like multi-tier architecture using Docker Compose.
The application stack includes:

* Nginx (Web Layer / Reverse Proxy)
* Tomcat (Java Application Server)
* MySQL (Database)
* Memcached (Caching Layer)
* RabbitMQ (Message Broker)

The goal of this project is to simulate a real-world enterprise deployment environment using containers.

---

##  Architecture Diagram

```
Client
   ↓
Nginx (Port 80)
   ↓
Tomcat Application (Port 8080)
   ↓
 ├── MySQL (Port 3306)
 ├── Memcached (Port 11211)
 └── RabbitMQ (Port 15672)
```

---

##  Services Explanation

### 1️⃣ Web Layer – Nginx (vproweb)

* Acts as a Reverse Proxy.
* Listens on Port 80.
* Forwards incoming HTTP traffic to the application container.
* Serves as the public entry point of the system.

---

### 2️⃣ Application Layer – Tomcat (vproapp)

* Runs the Java WAR application.
* Listens on Port 8080.
* Handles business logic.
* Communicates with database, cache, and message queue.
* Uses a named volume for persistence.

---

### 3️⃣ Database Layer – MySQL (vprodb)

* Stores application data.
* Listens on Port 3306.
* Uses a named volume (`vprodbdata`) to persist data.
* Root password is set using environment variables.

---

### 4️⃣ Cache Layer – Memcached (vprocache01)

* Improves performance by caching frequently accessed data.
* Reduces database load.
* Listens on Port 11211.

---

### 5️⃣ Message Broker – RabbitMQ (vpromq01)

* Enables asynchronous communication.
* Used for background tasks and decoupled processing.
* Exposes management UI on Port 15672.
* Default credentials are configured via environment variables.

---

##  Docker Compose Configuration

Key features of the docker-compose setup:

* Each service runs in its own isolated container.
* Services communicate using Docker’s internal network.
* Named volumes ensure data persistence.
* Ports are exposed for external access when required.

To run the project:

```
docker-compose up -d
```

To rebuild images:

```
docker-compose up --build
```

To stop services:

```
docker-compose down
```

---

##  Request Flow

1. User sends HTTP request to Port 80.
2. Nginx receives the request.
3. Nginx forwards it to the Tomcat application.
4. The application may:

   * Query MySQL for data.
   * Retrieve cached data from Memcached.
   * Send/receive messages via RabbitMQ.
5. The response is returned back to the user.

---



