# 05 - Nginx Setup

## 1. Login to the Nginx VM

```bash
vagrant ssh web01
```

Switch to root user:

```bash
sudo -i
```

## 2. Verify Hosts Entry

Check the hosts file:

```bash
cat /etc/hosts
```

Make sure the required VM hostnames are available, especially:

```text
app01
```

## 3. Update the Operating System

Update the package list:

```bash
apt update
```

Upgrade installed packages:

```bash
apt upgrade -y
```

## 4. Install Nginx

```bash
apt install nginx -y
```

Check the Nginx service:

```bash
systemctl status nginx
```

## 5. Create the Nginx Configuration

Create a new configuration file:

```bash
vi /etc/nginx/sites-available/vproapp
```

Add the following configuration:

```nginx
upstream vproapp {
    server app01:8080;
}

server {
    listen 80;

    location / {
        proxy_pass http://vproapp;
    }
}
```

This configuration forwards incoming traffic from Nginx to the Tomcat server running on `app01` port `8080`.

## 6. Remove the Default Nginx Configuration

```bash
rm -rf /etc/nginx/sites-enabled/default
```

## 7. Enable the VProfile Application

Create a symbolic link:

```bash
ln -s /etc/nginx/sites-available/vproapp \
/etc/nginx/sites-enabled/vproapp
```

## 8. Test the Nginx Configuration

Before restarting Nginx, check the configuration:

```bash
nginx -t
```

Expected result:

```text
syntax is ok
test is successful
```

## 9. Restart Nginx

```bash
systemctl restart nginx
```

Enable Nginx at boot:

```bash
systemctl enable nginx
```

## 10. Verify the Application

From your host machine, open:

```text
http://192.168.56.11
```

The request flow is:

```text
Client
  │
  │ HTTP :80
  ▼
web01 (Nginx)
  │
  │ Proxy Pass
  ▼
app01 (Tomcat :8080)
  │
  ├── db01  → MariaDB
  ├── mc01  → Memcached
  └── rmq01 → RabbitMQ
```
