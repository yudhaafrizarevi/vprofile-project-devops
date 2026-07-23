````markdown
# 🚀 VProfile Multi-VM DevOps Infrastructure

> **A production-like multi-tier application infrastructure built with Vagrant, VirtualBox, and Linux.**

This project demonstrates the deployment and integration of a Java web application across a distributed multi-VM infrastructure.

Each infrastructure component runs on a dedicated virtual machine, creating a production-like environment with separated responsibilities for:

- Web traffic management
- Application processing
- Database storage
- Application caching
- Asynchronous messaging

The complete infrastructure is designed to demonstrate practical DevOps, Linux system administration, networking, and application deployment concepts.

---

## 🏗️ Architecture

```text
                              ┌──────────────┐
                              │    CLIENT    │
                              └──────┬───────┘
                                     │
                                     │ HTTP :80
                                     ▼
                         ┌──────────────────────┐
                         │        web01         │
                         │        NGINX          │
                         │    Reverse Proxy      │
                         │   192.168.56.11       │
                         └──────────┬───────────┘
                                    │
                                    │ HTTP :8080
                                    ▼
                         ┌──────────────────────┐
                         │        app01         │
                         │    Apache Tomcat     │
                         │   Java Web Application│
                         │   192.168.56.12       │
                         └──────────┬───────────┘
                                    │
                 ┌──────────────────┼──────────────────┐
                 │                  │                  │
                 ▼                  ▼                  ▼
        ┌────────────────┐ ┌────────────────┐ ┌────────────────┐
        │      db01      │ │      mc01      │ │      rmq01      │
        │    MariaDB     │ │   Memcached    │ │    RabbitMQ    │
        │ 192.168.56.15  │ │ 192.168.56.14  │ │ 192.168.56.13  │
        └────────────────┘ └────────────────┘ └────────────────┘
````

---

## 🔄 Request Flow

```text
Client
   │
   ▼
Nginx Reverse Proxy
(web01:80)
   │
   ▼
Apache Tomcat
(app01:8080)
   │
   ├──► MariaDB
   │    (db01:3306)
   │
   ├──► Memcached
   │    (mc01:11211)
   │
   └──► RabbitMQ
        (rmq01:5672)
```

---

## 🖥️ Infrastructure Overview

| VM      | Role                | Technology    | Operating System | Private IP      |
| ------- | ------------------- | ------------- | ---------------- | --------------- |
| `web01` | Web & Reverse Proxy | Nginx         | Ubuntu 22.04     | `192.168.56.11` |
| `app01` | Application Server  | Apache Tomcat | CentOS Stream 9  | `192.168.56.12` |
| `rmq01` | Message Broker      | RabbitMQ      | CentOS Stream 9  | `192.168.56.13` |
| `mc01`  | Cache Server        | Memcached     | CentOS Stream 9  | `192.168.56.14` |
| `db01`  | Database Server     | MariaDB       | CentOS Stream 9  | `192.168.56.15` |

All virtual machines communicate through the private network:

```text
192.168.56.0/24
```

---

## 🧩 Component Architecture

### 🌐 web01 — Nginx Reverse Proxy

Nginx serves as the public-facing entry point for the application.

**Responsibilities:**

* Receives incoming HTTP requests.
* Acts as a reverse proxy.
* Forwards requests to the Tomcat application server.
* Provides a single access point to the application.

```text
Client
   │
   ▼
Nginx
(web01)
   │
   ▼
Tomcat
(app01)
```

---

### ☕ app01 — Apache Tomcat Application Server

Apache Tomcat runs the Java web application.

**Responsibilities:**

* Runs the Java application.
* Processes application requests.
* Connects the application to backend services.
* Hosts the deployed `.war` application.

The application is built using **Maven** and deployed to Tomcat as a `.war` file.

```text
Nginx
   │
   ▼
Apache Tomcat
   │
   ▼
Java Web Application
```

---

### 🗄️ db01 — MariaDB Database Server

MariaDB provides persistent storage for the application.

**Responsibilities:**

* Stores application data.
* Stores user account information.
* Handles database queries.
* Provides persistent data storage.

```text
Java Application
       │
       ▼
    MariaDB
```

---

### ⚡ mc01 — Memcached Cache Server

Memcached provides an in-memory caching layer for the application.

**Responsibilities:**

* Stores frequently accessed data temporarily.
* Reduces repeated database queries.
* Improves application response time.
* Reduces database workload.

```text
Java Application
       │
       ▼
   Memcached
       │
       ▼
 Faster Data Access
```

---

### 🐇 rmq01 — RabbitMQ Message Broker

RabbitMQ provides message queuing and asynchronous communication.

**Responsibilities:**

* Handles message queues.
* Enables asynchronous communication.
* Decouples application components.
* Allows messages to be processed independently.

```text
Java Application
       │
       ▼
    RabbitMQ
       │
       ▼
  Message Queue
```

---

## 🛠️ Technology Stack

| Technology    | Purpose                                  |
| ------------- | ---------------------------------------- |
| Vagrant       | Automated VM provisioning and management |
| VirtualBox    | Virtualization platform                  |
| Nginx         | Reverse proxy and web entry point        |
| Apache Tomcat | Java application server                  |
| Java 17       | Application runtime environment          |
| Maven         | Application build and packaging          |
| MariaDB       | Persistent database storage              |
| Memcached     | In-memory caching                        |
| RabbitMQ      | Message broker and message queuing       |
| Git           | Source code management                   |
| Linux         | Server operating system environment      |

---

# 🚀 Getting Started

## 📋 Prerequisites

Before starting the project, install the following:

* [Vagrant](https://developer.hashicorp.com/vagrant/downloads)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* [Git](https://git-scm.com/)

### Recommended System Requirements

* Minimum 8 GB RAM
* Internet connection
* Hardware virtualization enabled in BIOS/UEFI
* Vagrant Host Manager plugin installed

---

## 🔌 Install Vagrant Host Manager

```bash
vagrant plugin install vagrant-hostmanager
```

---

# 🚀 Deployment

## 1️⃣ Clone the Repository

```bash
git clone https://github.com/yudhaafrizarevi/vprofile-project-devops.git
```

Navigate to the project directory:

```bash
cd vprofile-project-devops
```

---

## 2️⃣ Start the Infrastructure

Provision all virtual machines:

```bash
vagrant up
```

Vagrant will create the following infrastructure:

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

Expected result:

```text
web01  running
app01  running
rmq01  running
mc01   running
db01   running
```

---

## 🔄 Deployment Workflow

```text
vagrant up
     │
     ▼
Create Virtual Machines
     │
     ▼
Configure Private Networking
     │
     ▼
Configure Hostnames
     │
     ▼
Install Infrastructure Services
     │
     ├──► MariaDB
     ├──► Memcached
     ├──► RabbitMQ
     ├──► Apache Tomcat
     └──► Nginx
     │
     ▼
Build Java Application
     │
     ▼
Deploy WAR Application
     │
     ▼
Application Ready
```

---

# ⚙️ Service Configuration

## 🗄️ 3. Configure MariaDB

Access the database server:

```bash
vagrant ssh db01
```

Detailed configuration guide:

```text
docs/01-mariadb.md
```

MariaDB is used as the primary database for persistent application data.

---

## ⚡ 4. Configure Memcached

Access the cache server:

```bash
vagrant ssh mc01
```

Detailed configuration guide:

```text
docs/02-memcached.md
```

Memcached is used to reduce database load and improve application performance.

---

## 🐇 5. Configure RabbitMQ

Access the message broker:

```bash
vagrant ssh rmq01
```

Detailed configuration guide:

```text
docs/03-rabbitmq.md
```

RabbitMQ provides message queuing and asynchronous communication between application components.

---

## ☕ 6. Configure Apache Tomcat

Access the application server:

```bash
vagrant ssh app01
```

Detailed configuration guide:

```text
docs/04-tomcat.md
```

Tomcat is configured to run the Java web application.

---

## 📦 7. Build and Deploy the Application

Inside the `app01` VM:

1. Install and configure Maven.
2. Clone the application source code.
3. Configure the application properties.
4. Configure backend service connections.
5. Build the application using Maven.
6. Deploy the generated `.war` file to Tomcat.

The application connects to:

```text
MariaDB   → db01
Memcached → mc01
RabbitMQ  → rmq01
```

Application flow:

```text
User Request
     │
     ▼
   Nginx
     │
     ▼
   Tomcat
     │
     ▼
Java Application
     │
     ├──► MariaDB
     ├──► Memcached
     └──► RabbitMQ
```

---

## 🌐 8. Configure Nginx

Access the web server:

```bash
vagrant ssh web01
```

Detailed configuration guide:

```text
docs/05-nginx.md
```

Nginx acts as a reverse proxy and forwards incoming requests to the Tomcat application server.

Final request flow:

```text
Client
   │
   ▼
Nginx
(web01:80)
   │
   ▼
Tomcat
(app01:8080)
   │
   ▼
Java Application
```

---

# 🌍 Application Access

After completing the infrastructure configuration and application deployment, access the application through:

```text
http://192.168.56.11
```

The application is accessed through the Nginx reverse proxy.

---

# 📊 Service Communication

| Service       | Host    |    Port | Purpose                 |
| ------------- | ------- | ------: | ----------------------- |
| Nginx         | `web01` |    `80` | HTTP reverse proxy      |
| Apache Tomcat | `app01` |  `8080` | Java application server |
| RabbitMQ      | `rmq01` |  `5672` | Message broker          |
| Memcached     | `mc01`  | `11211` | Application caching     |
| MariaDB       | `db01`  |  `3306` | Database server         |

---

# 📸 Project Screenshots

## Users List

![Users List](screenshots/users-list.png)

---

## User Details

![User Details](screenshots/user-details.png)

---

## RabbitMQ

![RabbitMQ](screenshots/rabbitmq.png)

---

# 📚 Documentation

Detailed configuration guides are available in the `docs/` directory:

* [MariaDB Setup](docs/01-mariadb.md)
* [Memcached Setup](docs/02-memcached.md)
* [RabbitMQ Setup](docs/03-rabbitmq.md)
* [Tomcat Setup](docs/04-tomcat.md)
* [Nginx Setup](docs/05-nginx.md)

---

# 🧰 Vagrant Commands

## Start All Virtual Machines

```bash
vagrant up
```

---

## Check VM Status

```bash
vagrant status
```

---

## Access a VM

```bash
vagrant ssh app01
```

---

## Re-run Provisioning

```bash
vagrant provision
```

---

## Reload a VM

```bash
vagrant reload app01
```

---

## Stop All Virtual Machines

```bash
vagrant halt
```

---

## Destroy All Virtual Machines

```bash
vagrant destroy -f
```

---

## Rebuild the Environment

```bash
vagrant destroy -f
vagrant up
```

---

# 🎯 Project Objectives

This project demonstrates practical experience with:

* Multi-VM infrastructure provisioning.
* Vagrant automation.
* VirtualBox virtualization.
* Linux server administration.
* Private network configuration.
* Hostname management.
* Reverse proxy configuration.
* Java application server deployment.
* Maven application build and packaging.
* Database integration.
* In-memory caching.
* Message broker configuration.
* Distributed application architecture.
* Basic DevOps infrastructure practices.

---

# 🎓 Skills Demonstrated

## Infrastructure & DevOps

* Infrastructure provisioning
* Virtual machine management
* Infrastructure as Code concepts
* Reproducible environment deployment
* Multi-tier architecture design

## Linux Administration

* Package management
* Service management with `systemctl`
* Linux networking
* Log analysis
* Process and port troubleshooting

## Networking

* Private network configuration
* Inter-server communication
* Hostname resolution
* Reverse proxy architecture
* TCP connectivity testing

## Application Deployment

* Java application deployment
* Apache Tomcat configuration
* Maven build process
* WAR application deployment
* Backend service integration

---

# 📌 Project Summary

This project simulates a production-like multi-tier application infrastructure where each major service runs on an independent virtual machine.

```text
                     ┌──────────────┐
                     │    CLIENT    │
                     └──────┬───────┘
                            │
                            ▼
                     ┌──────────────┐
                     │    NGINX     │
                     │    web01     │
                     └──────┬───────┘
                            │
                            ▼
                     ┌──────────────┐
                     │    TOMCAT    │
                     │    app01     │
                     └──────┬───────┘
                            │
             ┌──────────────┼──────────────┐
             │              │              │
             ▼              ▼              ▼
          MariaDB       Memcached       RabbitMQ
           db01            mc01           rmq01
```

This architecture demonstrates how web, application, database, caching, and messaging components can work together to support a distributed Java web application.

---

# ⭐ Portfolio Project

This project is part of my hands-on DevOps learning portfolio, demonstrating practical experience in:

* Linux
* Virtualization
* Infrastructure Provisioning
* Networking
* Application Deployment
* Reverse Proxy Configuration
* Database Services
* Caching
* Message Queuing

---

## 👨‍💻 Author

### Yudha Afriza Revi

DevOps and Cloud Infrastructure enthusiast focused on:

* Linux System Administration
* Infrastructure as Code
* Automation
* Virtualization
* Networking
* Application Deployment

🔗 GitHub:

https://github.com/yudhaafrizarevi

---

## 📄 License

This project is intended for educational and portfolio purposes.

````

**Catatan:** README ini sudah saya sesuaikan dengan project yang kamu kirim: `MariaDB`, `Memcached`, `RabbitMQ`, `Apache Tomcat`, dan `Nginx`, dengan 5 VM dan IP yang kamu gunakan.

Sebelum push ke GitHub, pastikan folder screenshot memang ada:

```text
screenshots/
├── users-list.png
├── user-details.png
└── rabbitmq.png
````

dan folder dokumentasi:

```text
docs/
├── 01-mariadb.md
├── 02-memcached.md
├── 03-rabbitmq.md
├── 04-tomcat.md
└── 05-nginx.md
```

Kalau file atau folder tersebut belum ada, hapus dulu bagian link/screenshot-nya dari README agar tidak menampilkan broken links di GitHub.
