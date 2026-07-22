# 🚀 VProfile Project - Multi-VM DevOps Infrastructure

A hands-on DevOps project demonstrating the deployment of a multi-tier web application using **Vagrant, VirtualBox, Nginx, Apache Tomcat, MariaDB, Memcached, and RabbitMQ**.

This project simulates a real-world infrastructure architecture where each major service runs on its own virtual machine.

---

## 📌 Project Overview

This project deploys a Java-based web application using a distributed multi-VM architecture.

The infrastructure is divided into several layers:

- **Web Layer** → Nginx
- **Application Layer** → Apache Tomcat
- **Database Layer** → MariaDB
- **Caching Layer** → Memcached
- **Messaging Layer** → RabbitMQ

All virtual machines are provisioned and managed using **Vagrant** and **VirtualBox**.

---

## 🏗️ Architecture

![Project Architecture](screenshots/architecture.png)

### Application Request Flow

```text
                 ┌─────────────┐
                 │    Users    │
                 └──────┬──────┘
                        │
                        ▼
              ┌──────────────────┐
              │      Nginx       │
              │    web01          │
              │ 192.168.56.11    │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │   Apache Tomcat  │
              │      app01        │
              │ 192.168.56.12    │
              └──────┬─────┬─────┘
                     │     │
          ┌──────────┘     └──────────┐
          ▼                           ▼
 ┌─────────────────┐         ┌─────────────────┐
 │     MariaDB     │         │    Memcached    │
 │      db01        │         │      mc01       │
 │ 192.168.56.15   │         │ 192.168.56.14  │
 └─────────────────┘         └─────────────────┘
                     │
                     ▼
              ┌─────────────────┐
              │    RabbitMQ     │
              │      rmq01       │
              │ 192.168.56.13  │
              └─────────────────┘
