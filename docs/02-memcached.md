# 02 - Memcached Setup

## 1. Login to the Memcached VM

```bash
vagrant ssh mc01
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

## 4. Install Memcached

Install EPEL:

```bash
sudo dnf install epel-release -y
```

Install Memcached:

```bash
sudo dnf install memcached -y
```

## 5. Start and Enable Memcached

```bash
sudo systemctl start memcached
sudo systemctl enable memcached
```

Check the status:

```bash
sudo systemctl status memcached
```

## 6. Configure Memcached to Listen on All Interfaces

By default, Memcached may listen only on `127.0.0.1`.

Change it to listen on all network interfaces:

```bash
sudo sed -i 's/127.0.0.1/0.0.0.0/g' /etc/sysconfig/memcached
```

Restart Memcached:

```bash
sudo systemctl restart memcached
```

Verify that Memcached is listening on port `11211`:

```bash
sudo ss -tulpn | grep 11211
```

Expected:

```text
0.0.0.0:11211
```

## 7. Configure the Firewall

Start and enable firewalld:

```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

Allow Memcached TCP port:

```bash
sudo firewall-cmd --permanent --add-port=11211/tcp
sudo firewall-cmd --reload
```

## 8. Optional UDP Configuration

If UDP access is required:

```bash
sudo firewall-cmd --permanent --add-port=11111/udp
sudo firewall-cmd --reload
```

## 9. Test Memcached Connectivity

From `app01`, test the connection:

```bash
nc -vzw 3 mc01 11211
```

If the connection is successful, the application server can communicate with Memcached.

## 10. Verify Memcached Service

```bash
sudo systemctl status memcached
```
