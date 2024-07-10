# 102 - Tooling Website Deployment Automation with Continuous Integration Using Jenkins

## Overview

<img width="542" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d9f174b3-73c6-47f5-a9c6-6accbbe40cc9">

This guide will walk you through setting up a Jenkins server for continuous integration and deployment, automating the deployment of source code changes from GitHub to an NFS server. A continuation of the Tooling Website Solution from [Project 7](../DevOps Tooling Website Solution/) & [Project 8](../Load-Balancer Solution With Apache/).

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
   <img width="735" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/b0af7e9a-b3ca-4b2a-9210-331c3c7e1648">
   <img width="724" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/02140bb9-c2a0-4c53-9e7c-74f315b70ebf">

- **Update the system, Install JDK and Verify JDK**
    ```sh
    sudo apt update && sudo apt upgrade -y
    sudo apt install openjdk-11-jdk -y
    java -version
    ```
    <img width="459" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/3ff4c717-e114-4958-a172-3be3b3c6da17">

- **Install Jenkins**
   ```bash
    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add - 
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
    ```
   <img width="473" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/12944169-b945-49bb-a67f-45bfc51bdad8">
   
    ```bash
    sudo apt update
    sudo apt install jenkins -y
    ```
   <img width="353" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d8bb0525-6c4d-4e8e-8ea0-50f00ac06a7e">

- **Start Jenkins service and verify its status**
    ```bash
    sudo systemctl start jenkins
    sudo systemctl enable jenkins
    sudo systemctl status jenkins
    ```
   <img width="518" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4c4098e0-52b0-4164-9486-8da241513293">

- **Perform initial Jenkins setup**

  - Access Jenkins through your browser using `http://<jenkins-server-public-ip>:8080`.
  <img width="848" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6c852fe5-6395-45dc-bfd0-cdf5020b444d">
  
  - Retrieve the initial admin password from the Jenkins server:
    ```sh
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

  - Follow the setup wizard and install the suggested plugins.
   <img width="833" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/94254e9a-66a8-4333-8f97-6b5ce34af597">

    - Create Jenkins First Console User:

    - Jenkins Set-Up Completed:
     <img width="832" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/166871a0-f7f0-4368-a7a5-64f09191e62a">

