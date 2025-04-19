# Spring PetClinic Deployment on Ubuntu 24.04

This guide demonstrates the manual setup and Docker-based deployment of the Spring PetClinic (SPC) application on an Ubuntu 24.04 server (t2.medium instance).

---

## Manual Setup

### 1. Clone the Spring PetClinic Repository

```bash
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
```

### 2. Install Java 17

```bash
sudo apt update 
sudo apt install openjdk-17-jdk
```

### 3. Install Maven

```bash
wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
tar xvzf apache-maven-3.9.9-bin.tar.gz
sudo mkdir /opt/maven
sudo mv apache-maven-3.9.9 /opt/maven
```

### 4. Set Up Maven Environment Variables

Add Maven to the system `PATH`. Open the environment configuration file:

```bash
sudo vi /etc/environment
```

Append the Maven binary path:

```
:/opt/maven/apache-maven-3.9.9/bin
```

Save and exit. Then log out and log back in for the changes to take effect.

### 5. Verify Maven Installation

```bash
mvn --version
```

### 6. Build the Spring PetClinic Application

Navigate to the project directory and build the application:

```bash
cd spring-petclinic
mvn clean package
```

### 7. Run the Application

Navigate to the `target` directory and run the JAR file:

```bash
cd target
java -jar spring-petclinic-3.4.0-SNAPSHOT.jar
```

### 8. Access the Application in Browser

Visit the application using your EC2 public IP:

```bash
http://<your-ec2-public-ip>:8080
```

> **Note:** Ensure that the EC2 instance has inbound rules configured to allow HTTP (port 8080) and SSH access.

![Spring PetClinic Manual](images/image.png)

---

# Docker-Based Deployment

### 1. Install Docker on Ubuntu 24.04

```bash
curl -fsSL https://get.docker.com -o install-docker.sh
sh install-docker.sh
```

### 2. Add User to Docker Group

```bash
sudo usermod -aG docker ubuntu
```

Log out and log back in for the changes to take effect.

### 3. Fork and Clone the Repository

Fork the official Spring PetClinic repository into your GitHub account. Then clone your forked repository into your local folder for writing dockerfile:

```bash
git clone https://github.com/Zeeshanshaikh93/spring-petclinic.git
cd spring-petclinic
git pull
```

### 4. Create a Multistage Dockerfile

Here's an optimized multistage Dockerfile for efficient builds:

```dockerfile
# Build Stage
FROM maven:3.9-eclipse-temurin-17 AS builder
COPY . /spc
WORKDIR /spc
RUN mvn package -DskipTests

# Runtime Stage
FROM eclipse-temurin:17-jre AS runner
WORKDIR /app
COPY --from=builder /spc/target/spring-petclinic-3.4.0-SNAPSHOT.jar spring-petclinic.jar
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic.jar"]
```

### 5. Build and Run the Docker Image

```bash
docker build -t spc:1.0 .
docker run -d -p 8080:8080 spc:1.0
```

Check the running container:

```bash
docker ps
```

### 6. Access the Application in Browser

Open your browser and navigate to:

```bash
http://<your-ec2-public-ip>:8080
```

![Spring PetClinic Docker](images/image01.png)

> **Note:** Confirm that your EC2 instance's security group allows inbound traffic on port 8080.

---
