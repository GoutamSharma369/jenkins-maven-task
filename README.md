Jenkins Task 8: Simple Java Maven Build

This repository contains a basic "Hello World" Java application built using Maven. The primary goal of this project was to set up a CI/CD pipeline using Jenkins to automatically build the project.

Jenkins Job: Freestyle Project

Build Tool: Maven

Environment: Jenkins running as a Docker container on an AWS EC2 Ubuntu instance.

Final Result: Build Success! 🚀

Here is the console output from the successful Jenkins build, showing that Jenkins successfully cloned the repository, invoked Maven, and compiled the Java code.

Running as SYSTEM
Building in workspace /var/jenkins_home/workspace/Hello-World-Build
...
Cloning repository [https://github.com/GoutamSharma369/jenkins-maven-task.git](https://github.com/GoutamSharma369/jenkins-maven-task.git)
 > git checkout -f 6f762d03bf443e0da9b0c77b9fc9911a10b895a4
...
[Hello-World-Build] $ /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/My-Maven-3.9/bin/mvn clean package
[INFO] Scanning for projects...
[INFO] 
[INFO] -------------------------< com.example:hello >--------------------------
[INFO] Building hello 1.0
...
[INFO] --- clean:3.2.0:clean (default-clean) @ hello ---
...
[INFO] --- compiler:3.8.1:compile (default-compile) @ hello ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /var/jenkins_home/workspace/Hello-World-Build/target/classes
...
[INFO] --- jar:3.3.0:jar (default-jar) @ hello ---
[INFO] Building jar: /var/jenkins_home/workspace/Hello-World-Build/target/hello-1.0.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  ...
[INFO] Finished at: ...
[INFO] ------------------------------------------------------------------------
Finished: SUCCESS


Steps & Commands to Replicate This Project

Phase 1: Create the Java Project & Push to GitHub

Create the project directory:

mkdir hello-java-maven
cd hello-java-maven


Create pom.xml:

vim pom.xml <<EOF
<project>
<modelVersion>4.0.0</modelVersion>
<groupId>com.example</groupId>
<artifactId>hello</artifactId>
<version>1.0</version>
<build>
<plugins>
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-compiler-plugin</artifactId>
<version>3.8.1</version>
<configuration>
<source>1.8</source>
<target>1.8</target>
</configuration>
</plugin>
</plugins>
</build>
</project>
EOF


Create the Java source directory:

mkdir -p src/main/java


Create HelloWorld.java:

vim src/main/java/HelloWorld.java <<EOF
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Jenkins + Maven!");
    }
}
EOF


Push to GitHub:

git init
git add .
git commit -m "Initial Hello World commit"
git branch -M main
git remote add origin [https://github.com/GoutamSharma369/jenkins-maven-task.git](https://github.com/GoutamSharma369/jenkins-maven-task.git)
git push -u origin main


Phase 2: Set Up the Jenkins Server (AWS Ubuntu)

Install Docker:

sudo apt update
sudo apt install docker.io -y


Add user to the Docker group (to run docker without sudo):

sudo usermod -aG docker ${USER}
# IMPORTANT: Log out and log back in for this change to take effect!
exit


Open AWS Security Group:

In the AWS EC2 Console, navigate to your instance.

Go to Security > Security Groups.

Click Edit inbound rules and add a rule:

Type: Custom TCP

Port range: 8080

Source: Anywhere - IPv4 (0.0.0.0/0) or My IP

Start the Jenkins Docker Container:

# This creates a persistent volume 'jenkins_home' to save your data
docker run -d -p 8080:8080 -v jenkins_home:/var/jenkins_home --name my-jenkins jenkins/jenkins:lts


Get the Initial Admin Password:

# Wait ~30-60 seconds for Jenkins to start
docker logs my-jenkins
