# 🚀 VProfile Multi-VM DevOps Infrastructure

> A production-like multi-tier Java application infrastructure provisioned and managed using Vagrant, VirtualBox, and Linux.

---

## 📌 Project Overview

This project demonstrates the deployment of a distributed Java web application across a multi-tier infrastructure environment.

Each major infrastructure component runs on a dedicated virtual machine with a specific responsibility:

- 🌐 Web traffic management
- ☕ Java application processing
- 🗄️ Database storage
- ⚡ Application caching
- 🐇 Asynchronous messaging

The entire infrastructure is provisioned using **Vagrant**, making the environment reproducible, automated, and easy to recreate.

---

## 🏗️ System Architecture

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
```

---

## 🔄 Application Request Flow

```text
Client
   │
   ▼
Nginx Reverse Proxy
web01:80
   │
   ▼
Apache Tomcat
app01:8080
   │
   ▼
Java Web Application
   │
   ├──► MariaDB
   │    db01:3306
   │
   ├──► Memcached
   │    mc01:11211
   │
   └──► RabbitMQ
        rmq01:5672
```

---

## 🖥️ Infrastructure Overview

| VM | Role | Technology | Operating System | Private IP |
|---|---|---|---|---|
| `web01` | Web & Reverse Proxy | Nginx | Ubuntu 22.04 | `192.168.56.11` |
| `app01` | Application Server | Apache Tomcat | CentOS Stream 9 | `192.168.56.12` |
| `rmq01` | Message Broker | RabbitMQ | CentOS Stream 9 | `192.168.56.13` |
| `mc01` | Cache Server | Memcached | CentOS Stream 9 | `192.168.56.14` |
| `db01` | Database Server | MariaDB | CentOS Stream 9 | `192.168.56.15` |

### Private Network

```text
192.168.56.0/24
```

All virtual machines communicate through a dedicated private network.

---

# 🧩 Infrastructure Components

## 🌐 web01 — Nginx Reverse Proxy

Nginx acts as the public-facing entry point for the application.

### Responsibilities

- Receives incoming HTTP requests.
- Acts as a reverse proxy.
- Forwards requests to the Tomcat application server.
- Provides a single access point for the application.

```text
Client
   │
   ▼
Nginx
web01:80
   │
   ▼
Tomcat
app01:8080
```

---

## ☕ app01 — Apache Tomcat Application Server

Apache Tomcat runs the Java web application.

### Responsibilities

- Runs the Java application.
- Processes application requests.
- Connects the application to backend services.
- Hosts the deployed `.war` application.

The application is built using **Maven** and deployed to Tomcat as a `.war` file.

---

## 🗄️ db01 — MariaDB Database Server

MariaDB provides persistent data storage for the application.

### Responsibilities

- Stores application data.
- Stores user account information.
- Handles database queries.
- Provides persistent storage.

```text
Java Application
       │
       ▼
    MariaDB
```

---

## ⚡ mc01 — Memcached Cache Server

Memcached provides an in-memory caching layer for the application.

### Responsibilities

- Stores frequently accessed data temporarily.
- Reduces repeated database queries.
- Improves application response time.
- Reduces database workload.

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

## 🐇 rmq01 — RabbitMQ Message Broker

RabbitMQ provides message queuing and asynchronous communication.

### Responsibilities

- Handles message queues.
- Enables asynchronous communication.
- Decouples application components.
- Allows messages to be processed independently.

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

# 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Vagrant | Automated VM provisioning and management |
| VirtualBox | Virtualization platform |
| Nginx | Reverse proxy and web entry point |
| Apache Tomcat | Java application server |
| Java | Application runtime environment |
| Maven | Application build and packaging |
| MariaDB | Persistent database storage |
| Memcached | In-memory caching |
| RabbitMQ | Message broker and asynchronous messaging |
| Git | Source code management |
| Linux | Server operating system |

---

# 🚀 Getting Started

## 📋 Prerequisites

Before starting the project, install:

- Vagrant
- VirtualBox
- Git

### Recommended System Requirements

- Minimum 8 GB RAM
- Internet connection
- Hardware virtualization enabled in BIOS/UEFI

---

## 🔌 Install Vagrant Host Manager

```bash
vagrant plugin install vagrant-hostmanager
```

Vagrant Host Manager helps manage hostname resolution between virtual machines.

---

# 🚀 Deployment

## 1. Clone the Repository

```bash
git clone https://github.com/yudhaafrizarevi/vprofile-project-devops.git
```

Navigate to the project directory:

```bash
cd vprofile-project-devops
```

---

## 2. Start the Infrastructure

Provision all virtual machines:

```bash
vagrant up
```

Vagrant will automatically create and configure:

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

# 🔄 Deployment Workflow

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

# 📦 Application Build & Deployment

The application deployment process includes:

1. Install and configure Java.
2. Install and configure Maven.
3. Clone the application source code.
4. Configure application properties.
5. Configure backend service connections.
6. Build the application using Maven.
7. Generate the `.war` package.
8. Deploy the application to Apache Tomcat.

Application dependencies:

```text
Java Web Application
        │
        ├──► MariaDB
        │    db01:3306
        │
        ├──► Memcached
        │    mc01:11211
        │
        └──► RabbitMQ
             rmq01:5672
```

---

# ⚙️ Service Access

## 🗄️ MariaDB

```bash
vagrant ssh db01
```

MariaDB is used as the primary database for persistent application data.

---

## ⚡ Memcached

```bash
vagrant ssh mc01
```

Memcached is used to improve application performance by caching frequently accessed data.

---

## 🐇 RabbitMQ

```bash
vagrant ssh rmq01
```

RabbitMQ provides asynchronous communication and message queuing between application components.

---

## ☕ Apache Tomcat

```bash
vagrant ssh app01
```

Tomcat hosts and runs the deployed Java web application.

---

## 🌐 Nginx

```bash
vagrant ssh web01
```

Nginx acts as a reverse proxy and forwards incoming requests to the Tomcat application server.

---

# 🌍 Application Access

After the infrastructure has been successfully provisioned and the application has been deployed, access the application through:

```text
http://192.168.56.11
```

The request flow is:

```text
Client
   │
   ▼
Nginx
web01:80
   │
   ▼
Apache Tomcat
app01:8080
   │
   ▼
Java Web Application
```

---

# 📊 Service Communication

| Service | Host | Port | Purpose |
|---|---|---:|---|
| Nginx | `web01` | `80` | HTTP reverse proxy |
| Apache Tomcat | `app01` | `8080` | Java application server |
| RabbitMQ | `rmq01` | `5672` | Message broker |
| Memcached | `mc01` | `11211` | Application caching |
| MariaDB | `db01` | `3306` | Database server |

---

# 🧰 Vagrant Commands

### Start All Virtual Machines

```bash
vagrant up
```

### Check VM Status

```bash
vagrant status
```

### Access a VM

```bash
vagrant ssh app01
```

### Re-run Provisioning

```bash
vagrant provision
```

### Reload a VM

```bash
vagrant reload app01
```

### Stop All Virtual Machines

```bash
vagrant halt
```

### Destroy All Virtual Machines

```bash
vagrant destroy -f
```

### Rebuild the Entire Environment

```bash
vagrant destroy -f
vagrant up
```

---

# 📚 Project Structure

```text
.
├── Vagrantfile
├── scripts/
│   ├── mysql.sh
│   ├── memcache.sh
│   ├── rabbitmq.sh
│   ├── tomcat.sh
│   └── nginx.sh
├── docs/
│   ├── 01-mariadb.md
│   ├── 02-memcached.md
│   ├── 03-rabbitmq.md
│   ├── 04-tomcat.md
│   └── 05-nginx.md
└── README.md
```

---

# 🎯 Skills Demonstrated

## Infrastructure & DevOps

- Multi-VM infrastructure provisioning
- Infrastructure as Code concepts
- Virtual machine management
- Reproducible environment deployment
- Multi-tier architecture design

## Linux System Administration

- Package management
- Service management using `systemctl`
- Linux networking
- Log analysis
- Process and port troubleshooting

## Networking

- Private network configuration
- Inter-server communication
- Hostname resolution
- Reverse proxy architecture
- TCP connectivity testing

## Application Deployment

- Java application deployment
- Apache Tomcat configuration
- Maven build process
- WAR application deployment
- Backend service integration

---

# 📌 Project Highlights

This project demonstrates practical experience with:

- Linux System Administration
- Infrastructure as Code
- Vagrant Automation
- VirtualBox Virtualization
- Multi-Tier Architecture
- Private Networking
- Nginx Reverse Proxy
- Apache Tomcat
- Java Application Deployment
- Maven Build Automation
- MariaDB
- Memcached
- RabbitMQ
- Distributed Application Infrastructure

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

This architecture demonstrates how web, application, database, caching, and messaging components work together to support a distributed Java web application.

---

# 👨‍💻 Author

## Yudha Afriza Revi

DevOps and Cloud Infrastructure enthusiast focused on:

- Linux System Administration
- Infrastructure as Code
- Automation
- Virtualization
- Networking
- Application Deployment

GitHub:

https://github.com/yudhaafrizarevi

---

## 📄 License

This project is intended for educational and portfolio purposes.
