# 03 - RabbitMQ Setup

## 1. Login to the RabbitMQ VM

```bash
vagrant ssh rmq01
```

## 2. Verify Hosts Entry

Check the hosts file:

```bash
cat /etc/hosts
```

Make sure the required VM hostnames are available.

## 3. Update the Operating System

```bash
sudo dnf update -y
```

## 4. Install EPEL Repository

```bash
sudo dnf install epel-release -y
```

## 5. Install Dependencies

```bash
sudo dnf install wget -y
```

## 6. Install RabbitMQ Repository

```bash
sudo dnf -y install centos-release-rabbitmq-38
```

## 7. Install RabbitMQ Server

```bash
sudo dnf --enablerepo=centos-rabbitmq-38 -y install rabbitmq-server
```

## 8. Enable and Start RabbitMQ

```bash
sudo systemctl enable --now rabbitmq-server
```

Check the status:

```bash
sudo systemctl status rabbitmq-server
```

## 9. Configure RabbitMQ User

Allow access to RabbitMQ:

```bash
sudo sh -c 'echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config'
```

Create the RabbitMQ user:

```bash
sudo rabbitmqctl add_user test test
```

Set the user as administrator:

```bash
sudo rabbitmqctl set_user_tags test administrator
```

Set permissions:

```bash
sudo rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
```

Restart RabbitMQ:

```bash
sudo systemctl restart rabbitmq-server
```

## 10. Configure the Firewall

Start and enable firewalld:

```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

Allow RabbitMQ port `5672`:

```bash
sudo firewall-cmd --permanent --add-port=5672/tcp
sudo firewall-cmd --reload
```

## 11. Enable RabbitMQ at Boot

```bash
sudo systemctl start rabbitmq-server
sudo systemctl enable rabbitmq-server
```

## 12. Verify RabbitMQ

```bash
sudo systemctl status rabbitmq-server
```

Check that RabbitMQ is listening on port `5672`:

```bash
sudo ss -tulpn | grep 5672
```

## 13. Test Connectivity

From `app01`:

```bash
nc -vzw 3 rmq01 5672
```

If the connection is successful, `app01` can communicate with RabbitMQ.
