# 04 - Apache Tomcat Setup

## 1. Login to the Tomcat VM

```bash
vagrant ssh app01
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

## 5. Install Java and Dependencies

Install Java 17:

```bash
sudo dnf -y install java-17-openjdk java-17-openjdk-devel
```

Install Git and Wget:

```bash
sudo dnf install git wget -y
```

Verify Java:

```bash
java -version
```

## 6. Download Apache Tomcat

Change to `/tmp`:

```bash
cd /tmp/
```

Download Apache Tomcat:

```bash
wget https://archive.apache.org/dist/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10.1.26.tar.gz
```

Extract the archive:

```bash
tar xzvf apache-tomcat-10.1.26.tar.gz
```

## 7. Create the Tomcat User

```bash
sudo useradd --home-dir /usr/local/tomcat --shell /sbin/nologin tomcat
```

## 8. Copy Tomcat Files

Create the Tomcat directory:

```bash
sudo mkdir -p /usr/local/tomcat
```

Copy the Tomcat files:

```bash
sudo cp -r /tmp/apache-tomcat-10.1.26/* /usr/local/tomcat/
```

Change ownership:

```bash
sudo chown -R tomcat:tomcat /usr/local/tomcat
```

## 9. Create the Tomcat Systemd Service

Create the service file:

```bash
sudo vi /etc/systemd/system/tomcat.service
```

Add the following configuration:

```ini
[Unit]
Description=Tomcat
After=network.target

[Service]
User=tomcat
Group=tomcat
WorkingDirectory=/usr/local/tomcat

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_PID=/var/tomcat/%i/run/tomcat.pid
Environment=CATALINA_HOME=/usr/local/tomcat
Environment=CATALINA_BASE=/usr/local/tomcat

ExecStart=/usr/local/tomcat/bin/catalina.sh run
ExecStop=/usr/local/tomcat/bin/shutdown.sh

RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

## 10. Reload Systemd

```bash
sudo systemctl daemon-reload
```

## 11. Start and Enable Tomcat

```bash
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

Check the status:

```bash
sudo systemctl status tomcat
```

Verify that Tomcat is listening on port `8080`:

```bash
sudo ss -tulpn | grep 8080
```

Expected:

```text
0.0.0.0:8080
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

Allow Tomcat port `8080`:

```bash
sudo firewall-cmd --permanent --add-port=8080/tcp
sudo firewall-cmd --reload
```

## 13. Test Tomcat

From `web01`:

```bash
curl http://app01:8080
```

Or:

```bash
curl http://192.168.56.12:8080
```

If Tomcat is working, the server should return a response from Apache Tomcat.
