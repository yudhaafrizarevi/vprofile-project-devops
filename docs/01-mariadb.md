# 01 - MariaDB Setup

## 1. Login to the DB VM

```bash
vagrant ssh db01
```

## 2. Verify Hosts Entry

Check the hosts file:

```bash
cat /etc/hosts
```

Make sure the required VM hostnames are available, such as:

```text
192.168.56.15 db01
192.168.56.14 mc01
192.168.56.13 rmq01
192.168.56.12 app01
192.168.56.11 web01
```

## 3. Update the Operating System

```bash
sudo dnf update -y
```

## 4. Install EPEL Repository

```bash
sudo dnf install epel-release -y
```

## 5. Install MariaDB

```bash
sudo dnf install git mariadb-server -y
```

## 6. Start and Enable MariaDB

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

Check the status:

```bash
sudo systemctl status mariadb
```

## 7. Secure MariaDB Installation

Run:

```bash
sudo mysql_secure_installation
```

Recommended answers:

```text
Set root password? Y
Remove anonymous users? Y
Disallow root login remotely? N
Remove test database and access to it? Y
Reload privilege tables now? Y
```

The database root password used in this project is:

```text
admin123
```

## 8. Create the Database and User

Login to MariaDB:

```bash
mysql -u root -p
```

Then execute:

```sql
CREATE DATABASE accounts;

GRANT ALL PRIVILEGES ON accounts.* 
TO 'admin'@'localhost' 
IDENTIFIED BY 'admin123';

GRANT ALL PRIVILEGES ON accounts.* 
TO 'admin'@'%' 
IDENTIFIED BY 'admin123';

FLUSH PRIVILEGES;

EXIT;
```

## 9. Download the Source Code

```bash
cd /tmp

git clone -b local https://github.com/hkhcoder/vprofile-project.git

cd vprofile-project
```

## 10. Import the Database

```bash
mysql -u root -p accounts < src/main/resources/db_backup.sql
```

Verify the tables:

```bash
mysql -u root -p accounts
```

Inside MariaDB:

```sql
SHOW TABLES;
EXIT;
```

## 11. Restart MariaDB

```bash
sudo systemctl restart mariadb
```

## 12. Configure the Firewall

Start and enable firewalld:

```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

Check active zones:

```bash
sudo firewall-cmd --get-active-zones
```

Allow MariaDB port 3306:

```bash
sudo firewall-cmd --permanent --add-port=3306/tcp
sudo firewall-cmd --reload
```

Restart MariaDB:

```bash
sudo systemctl restart mariadb
```

## 13. Test Database Connectivity

From `app01`, test the connection to `db01`:

```bash
nc -vzw 3 db01 3306
```

If the connection is successful, the application server can communicate with MariaDB.
