# 🚀 VProfile Multi-VM DevOps Project

A multi-VM DevOps project built using **Vagrant and VirtualBox** to simulate a production-like application infrastructure.

This project demonstrates the deployment and integration of a Java web application with multiple backend services running on separate virtual machines.

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

## 🖥️ Virtual Machines

| VM | Role | IP Address | Operating System |
|---|---|---|---|
| `web01` | Nginx Reverse Proxy | `192.168.56.11` | Ubuntu 22.04 |
| `app01` | Apache Tomcat Application Server | `192.168.56.12` | CentOS Stream 9 |
| `rmq01` | RabbitMQ Message Broker | `192.168.56.13` | CentOS Stream 9 |
| `mc01` | Memcached Cache Server | `192.168.56.14` | CentOS Stream 9 |
| `db01` | MariaDB Database Server | `192.168.56.15` | CentOS Stream 9 |

## 🛠️ Technologies

- Vagrant
- VirtualBox
- Nginx
- Apache Tomcat
- Java 17
- Maven
- MariaDB
- Memcached
- RabbitMQ
- Git

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

## 🚀 Deployment Steps

Follow these steps in order.

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/yudhaafrizarevi/vprofile-project-devops.git
cd vprofile-project-devops
```

### 2️⃣ Start the Virtual Machines

Provision all five virtual machines:

```bash
vagrant up
```

Check the VM status:

```bash
vagrant status
```

The following VMs should be running:

```text
web01
app01
rmq01
mc01
db01
```

### 3️⃣ Configure the Database

Login to the database server:

```bash
vagrant ssh db01
```

Follow the detailed instructions:

```text
docs/01-mariadb.md
```

MariaDB will be configured as the main database for the application.

### 4️⃣ Configure Memcached

Login to the Memcached server:

```bash
vagrant ssh mc01
```

Follow:

```text
docs/02-memcached.md
```

Memcached will be used as the application caching service.

### 5️⃣ Configure RabbitMQ

Login to the RabbitMQ server:

```bash
vagrant ssh rmq01
```

Follow:

```text
docs/03-rabbitmq.md
```

RabbitMQ will be used as the message broker.

### 6️⃣ Configure Apache Tomcat

Login to the application server:

```bash
vagrant ssh app01
```

Follow:

```text
docs/04-tomcat.md
```

Tomcat will be used to run the Java web application.

### 7️⃣ Build and Deploy the Application

Inside the `app01` VM:

1. Install and configure Maven.
2. Clone the application source code.
3. Update the application configuration with the backend server details.
4. Build the application using Maven.
5. Deploy the generated WAR file to Tomcat.

The application configuration should point to:

```text
MariaDB   → db01
Memcached → mc01
RabbitMQ  → rmq01
```

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

## 🌐 Access the Application

After completing all configuration and deployment steps, open:

```text
http://192.168.56.11
```

The request flow is:

```text
Client
  ↓
Nginx (web01)
  ↓
Tomcat (app01)
  ↓
MariaDB / Memcached / RabbitMQ
```

## 📸 Project Result

### Users List

![Users List](screenshots/users-list.png)

### User Details

![User Details](screenshots/user-details.png)

### RabbitMQ

![RabbitMQ](screenshots/rabbitmq.png)

## 📚 Documentation

Detailed setup instructions for each service are available in the `docs/` directory:

- [MariaDB Setup](docs/01-mariadb.md)
- [Memcached Setup](docs/02-memcached.md)
- [RabbitMQ Setup](docs/03-rabbitmq.md)
- [Tomcat Setup](docs/04-tomcat.md)
- [Nginx Setup](docs/05-nginx.md)

## 🛑 Stop the Virtual Machines

To stop all virtual machines:

```bash
vagrant halt
```

To destroy all virtual machines:

```bash
vagrant destroy -f
```

## 🎯 Project Objective

This project was created to demonstrate practical knowledge of:

- Multi-VM infrastructure provisioning
- Linux server administration
- Network configuration
- Reverse proxy configuration
- Application server deployment
- Database and caching integration
- Message broker configuration
- Basic DevOps infrastructure architecture

---

⭐ This project is part of my DevOps learning portfolio.
