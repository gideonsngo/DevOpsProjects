# 102 - Tooling Website Deployment Automation with Continuous Integration Using Jenkins

## Overview

![Architectural Diagram](./images/Architecture-Diagram.PNG)

This guide will walk you through setting up a Jenkins server for continuous integration and deployment, automating the deployment of source code changes from GitHub to an NFS server. A continuation of the Tooling Website Solution from [Project 7](../DevOps-Tooling-Website-Solution/) & [Project 8](../Load-Balancer-Solution-With-Apache/).

---

## Prerequisites

Before starting, ensure you have the following:
- An AWS account.
- Access to Tooling Website GitHub repository.
- NFS server setup and accessible from the previous projects.

> **Note**: I won't be provisioning the `Web Servers`, `Database Server` along with `Load Balancer` in this project since the task requirements focuses on the `NFS Server` and `Jenkins Server`.

## Step 1: Install Jenkins Server

- **Create an AWS EC2 instance**
   - Launch an EC2 instance using Ubuntu Server 24.02 LTS AMI and name it `Jenkins`.
   - Ensure the security group allows inbound TCP traffic on port 8080.

- **Update the system, Install JDK and Verify JDK**
    ```sh
    sudo apt update && sudo apt upgrade -y
    sudo apt install openjdk-11-jdk -y
    java -version
    ```
    ![Java Version](./images/java-version.PNG)

- **Install Jenkins**
   ```bash
    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add - 
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
    ```
    ![Add Jenkins GPG Keys](./images/add-jenkins-repo-gpg-keys.PNG)

    ```bash
    sudo apt update
    sudo apt install jenkins -y
    ```
    ![Package Manager Update](./images/sudo-apt-update.PNG)

- **Start Jenkins service and verify its status**
    ```bash
    sudo systemctl start jenkins
    sudo systemctl enable jenkins
    sudo systemctl status jenkins
    ```
    ![Jenkins Service Status](./images/jenkins-service-status.PNG)

- **Open port 8080**

  - Modify the security group to add a new inbound rule allowing TCP traffic on port 8080.
   ![Jenkins Security Group](./images/jenkins-sg.PNG)

- **Perform initial Jenkins setup**

  - Access Jenkins through your browser using `http://<jenkins-server-public-ip>:8080`.
   ![Jenkins Admin Password](./images/admin-password-jenkins.PNG)
  
  - Retrieve the initial admin password from the Jenkins server:
    ```sh
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

  - Follow the setup wizard and install the suggested plugins.
   ![Install Plugins](./images/customize-jenkins.PNG)

    - Create Jenkins First Console User:
     ![Create User](./images/jenkins-create-new-user.PNG)

    - Jenkins Set-Up Completed:
     ![Jenkins Ready](./images/jenkins-ready.PNG)  

