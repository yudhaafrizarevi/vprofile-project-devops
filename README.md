# 🚀 VProfile Multi-VM DevOps Project

A multi-VM DevOps infrastructure project built using **Vagrant and VirtualBox** to simulate a production-like application environment.

This project demonstrates the deployment and integration of a Java web application with multiple backend services running on separate virtual machines.

The infrastructure consists of:

- Nginx as a reverse proxy
- Apache Tomcat as the application server
- MariaDB as the database server
- Memcached as the caching server
- RabbitMQ as the message broker

---

## 🏗️ Architecture

```text
                    ┌──────────────┐
                    │    Client    │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    Nginx     │
                    │    web01     │
                    │192.168.56.11 │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    Tomcat    │
                    │    app01     │
                    │192.168.56.12 │
                    └──────┬───────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
     ┌─────────┐     ┌───────────┐    ┌──────────┐
     │ MariaDB │     │ Memcached │    │ RabbitMQ │
     │  db01   │     │   mc01    │    │  rmq01   │
     │ .15     │     │   .14      │    │   .13    │
     └─────────┘     └───────────┘    └──────────┘
```

### Request Flow

```text
Client
  │
  ▼
Nginx (web01)
  │
  ▼
Apache Tomcat (app01)
  │
  ├──► MariaDB (db01)
  │
  ├──► Memcached (mc01)
  │
  └──► RabbitMQ (rmq01)
```

---

## 🖥️ Virtual Machines

| VM | Role | Function | IP Address | Operating System |
|---|---|---|---|---|
| `web01` | Nginx | Reverse proxy and application entry point | `192.168.56.11` | Ubuntu 22.04 |
| `app01` | Apache Tomcat | Runs the Java web application | `192.168.56.12` | CentOS Stream 9 |
| `rmq01` | RabbitMQ | Message broker and message queue | `192.168.56.13` | CentOS Stream 9 |
| `mc01` | Memcached | In-memory caching service | `192.168.56.14` | CentOS Stream 9 |
| `db01` | MariaDB | Persistent database storage | `192.168.56.15` | CentOS Stream 9 |

---

## 🧩 Component Roles

### 🌐 web01 — Nginx Reverse Proxy

**Nginx** acts as the entry point for incoming client requests.

Responsibilities:

- Receives HTTP requests from users.
- Acts as a reverse proxy.
- Forwards requests to the Tomcat application server.
- Provides a single access point to the application.

```text
Client
   ↓
Nginx (web01)
   ↓
Tomcat (app01)
```

---

### ☕ app01 — Apache Tomcat Application Server

**Apache Tomcat** is the application server responsible for running the Java web application.

Responsibilities:

- Runs the Java application.
- Processes application requests.
- Connects the application to backend services.
- Hosts the deployed `.war` application file.

The application is built using **Maven** and deployed to Tomcat as a `.war` file.

```text
Nginx
   ↓
Tomcat
   ↓
Java Web Application
```

---

### 🗄️ db01 — MariaDB Database Server

**MariaDB** stores the application's persistent data.

Responsibilities:

- Stores user data.
- Stores application records.
- Provides data persistence.
- Handles database queries from the application.

```text
Java Application
       ↓
    MariaDB
```

---

### ⚡ mc01 — Memcached Cache Server

**Memcached** is used as an in-memory caching system.

Responsibilities:

- Stores frequently accessed data temporarily.
- Reduces repeated database queries.
- Improves application response time.
- Reduces database workload.

```text
Java Application
       ↓
   Memcached
       ↓
 Faster data access
```

---

### 🐇 rmq01 — RabbitMQ Message Broker

**RabbitMQ** is used as a message broker for asynchronous communication.

Responsibilities:

- Handles message queuing.
- Enables asynchronous communication between services.
- Helps decouple application components.
- Allows messages to be processed independently.

```text
Java Application
       ↓
    RabbitMQ
       ↓
  Message Queue
```

---

## 🛠️ Technologies and Their Purpose

| Technology | Purpose |
|---|---|
| Vagrant | Automates the creation and management of virtual machines |
| VirtualBox | Provides the virtualization platform |
| Nginx | Acts as a reverse proxy and application entry point |
| Apache Tomcat | Runs the Java web application |
| Java 17 | Runtime environment for the Java application |
| Maven | Builds and packages the Java application |
| MariaDB | Stores persistent application data |
| Memcached | Provides in-memory caching |
| RabbitMQ | Handles message queuing and asynchronous communication |
| Git | Used for source code management |
| Linux | Operating system environment for the infrastructure |

---

## 📋 Prerequisites

Before starting this project, make sure you have installed:

- [Vagrant](https://developer.hashicorp.com/vagrant/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Git

You also need:

- At least **8 GB RAM** recommended
- Internet connection
- Virtualization enabled in BIOS/UEFI
- Vagrant Host Manager plugin

Install the Vagrant Host Manager plugin:

```bash
vagrant plugin install vagrant-hostmanager
```

---

## 🚀 Deployment Steps

Follow these steps in order.

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/yudhaafrizarevi/vprofile-project-devops.git
cd vprofile-project-devops
```

---

### 2️⃣ Start the Virtual Machines

Provision all five virtual machines:

```bash
vagrant up
```

This will create:

```text
web01
app01
rmq01
mc01
db01
```

Check the VM status:

```bash
vagrant status
```

All virtual machines should be running.

---

### 3️⃣ Configure MariaDB

Login to the database server:

```bash
vagrant ssh db01
```

Follow the detailed instructions:

```text
docs/01-mariadb.md
```

MariaDB will be configured as the main database for the application.

The database stores persistent application data such as user accounts and application records.

---

### 4️⃣ Configure Memcached

Login to the Memcached server:

```bash
vagrant ssh mc01
```

Follow:

```text
docs/02-memcached.md
```

Memcached will be configured as the application's caching service.

It helps reduce database load and improve application performance.

---

### 5️⃣ Configure RabbitMQ

Login to the RabbitMQ server:

```bash
vagrant ssh rmq01
```

Follow:

```text
docs/03-rabbitmq.md
```

RabbitMQ will be configured as the message broker.

It provides message queuing and asynchronous communication between application components.

---

### 6️⃣ Configure Apache Tomcat

Login to the application server:

```bash
vagrant ssh app01
```

Follow:

```text
docs/04-tomcat.md
```

Tomcat will be configured to run the Java web application.

The application will be built using Maven and deployed as a `.war` file.

---

### 7️⃣ Build and Deploy the Application

Inside the `app01` VM:

1. Install and configure Maven.
2. Clone the application source code.
3. Update the application configuration.
4. Configure the backend server details.
5. Build the application using Maven.
6. Deploy the generated `.war` file to Tomcat.

The application configuration connects to:

```text
MariaDB   → db01
Memcached → mc01
RabbitMQ  → rmq01
```

The general application flow is:

```text
User Request
     ↓
Nginx
     ↓
Tomcat
     ↓
Java Application
     │
     ├──► MariaDB
     ├──► Memcached
     └──► RabbitMQ
```

---

### 8️⃣ Configure Nginx

Login to the web server:

```bash
vagrant ssh web01
```

Follow:

```text
docs/05-nginx.md
```

Nginx acts as a reverse proxy and forwards incoming traffic to the Tomcat application server.

The final request flow is:

```text
Client
   ↓
Nginx (web01)
   ↓
Tomcat (app01)
   ↓
Java Application
```

---

## 🌐 Access the Application

After completing all configuration and deployment steps, open:

```text
http://192.168.56.11
```

The application is accessed through the Nginx reverse proxy.

---

## 📸 Project Result

### Users List

![Users List](screenshots/users-list.png)

### User Details

![User Details](screenshots/user-details.png)

### RabbitMQ

![RabbitMQ](screenshots/rabbitmq.png)

---

## 📚 Documentation

Detailed setup instructions for each service are available in the `docs/` directory:

- [MariaDB Setup](docs/01-mariadb.md)
- [Memcached Setup](docs/02-memcached.md)
- [RabbitMQ Setup](docs/03-rabbitmq.md)
- [Tomcat Setup](docs/04-tomcat.md)
- [Nginx Setup](docs/05-nginx.md)

---

## 🛑 Stop the Virtual Machines

To stop all virtual machines:

```bash
vagrant halt
```

---

## 🗑️ Destroy the Virtual Machines

To completely remove all virtual machines:

```bash
vagrant destroy -f
```

---

## 🎯 Project Objective

This project was created to demonstrate practical knowledge of:

- Multi-VM infrastructure provisioning
- Vagrant automation
- VirtualBox virtualization
- Linux server administration
- Private network configuration
- Hostname management
- Reverse proxy configuration
- Java application server deployment
- Maven application build process
- Database integration
- In-memory caching
- Message broker configuration
- Basic DevOps infrastructure architecture

---

## 📌 Project Summary

This project simulates a production-like application infrastructure where each service is deployed on a dedicated virtual machine.

The infrastructure is designed as follows:

```text
                    ┌──────────────┐
                    │    Client    │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    Nginx     │
                    │    web01     │
                    └──────┬───────┘
                           │
                           ▼
                    ┌──────────────┐
                    │    Tomcat    │
                    │    app01     │
                    └──────┬───────┘
                           │
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
       MariaDB         Memcached        RabbitMQ
        db01              mc01            rmq01
```

This architecture demonstrates how different infrastructure components work together to support a Java web application.

---

⭐ This project is part of my DevOps learning portfolio.
