## 14. Maven Setup

Change to `/tmp`:

```bash
cd /tmp/
```

Download Maven:

```bash
wget https://archive.apache.org/dist/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.zip
```

Extract Maven:

```bash
unzip apache-maven-3.9.9-bin.zip
```

Copy Maven to `/usr/local`:

```bash
sudo cp -r apache-maven-3.9.9 /usr/local/maven3.9
```

Set Maven memory options:

```bash
export MAVEN_OPTS="-Xmx512m"
```

## 15. Download the Source Code

```bash
git clone -b local https://github.com/hkhcoder/vprofile-project.git
```

Enter the project directory:

```bash
cd vprofile-project
```

## 16. Update Application Configuration

Edit the application configuration:

```bash
vim src/main/resources/application.properties
```

Configure the backend services according to the infrastructure:

```properties
jdbc.url=jdbc:mysql://db01:3306/accounts
jdbc.username=admin
jdbc.password=admin123
```

The application should also be configured to connect to:

```text
MariaDB    → db01
Memcached  → mc01
RabbitMQ   → rmq01
```

## 17. Build the Application

Run the following command inside the `vprofile-project` repository:

```bash
/usr/local/maven3.9/bin/mvn install
```

After a successful build, the WAR file will be available inside:

```text
target/
```

## 18. Deploy the Application to Tomcat

Stop Tomcat:

```bash
sudo systemctl stop tomcat
```

Remove the existing ROOT application:

```bash
sudo rm -rf /usr/local/tomcat/webapps/ROOT*
```

Copy the generated WAR file:

```bash
sudo cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
```

Start Tomcat:

```bash
sudo systemctl start tomcat
```

Set the correct ownership:

```bash
sudo chown -R tomcat:tomcat /usr/local/tomcat/webapps
```

Restart Tomcat:

```bash
sudo systemctl restart tomcat
```

Verify the Tomcat service:

```bash
sudo systemctl status tomcat
```

## 19. Verify Application Deployment

Check that Tomcat is listening on port `8080`:

```bash
sudo ss -tulpn | grep 8080
```

From `web01`, test the application:

```bash
curl http://app01:8080
```

Or:

```bash
curl http://192.168.56.12:8080
```

If the application is deployed successfully, Tomcat should return the application response.
