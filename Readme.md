# Spring-Petclinic 

* This depicts the manual approach of SPC in ubuntu-24.04 using t2.medium as a vcpu
* After creating the server clone the git repo of SPC as given below

```bash
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
```

* Then download Java -17

```bash 
sudo apt update 
sudo apt install openjdk-17-jdk
```

* Then download the maven 

```bash
wget https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz
tar xvzf apache-maven-3.9.9-bin.tar.gz
mkdir /opt/maven
sudo mv apache-maven-3.9.9 /opt/maven
Then copy this path to vi editor to setup the maven permission, makesure to keep the colon ":" after the available paths are shown, 
":/opt/maven/apache-maven-3.9.9/bin/"
Then exit and relogin
And then check the version
"mvn --version"
```

* Now "cd spring-petclinic" & apply the below command

```bash
mvn clean package
```

* After building the SPC Project, goto this folder "cd spring-petclinic/target" and then apply this command
  
```bash
java -jar spring-petclinic-3.4.0-SNAPSHOT.jar
```

* After this access with your public ip address 

```bash
http://your<publicip>:8080
```

![alt text](images/image.png)

*** Note: Make sure that in your ubuntu machine security inbound rules are open for HTTP & SSH ***