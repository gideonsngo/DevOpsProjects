# 102 - Ansible Configuration Management
This document is a continuation of the `Tooling Website Solution Project`, it provides a step-by-step guide to setting up and using Ansible for configuration management on an EC2 instance, including `installing and configuring Ansible`, `creating playbooks`, and `automating server configuration tasks`.

## Step 1: Install and Configure Ansible on EC2 Instance

- **Update the `Name` tag on your `Jenkins` EC2 instance to `Jenkins-Ansible`.**
   - This server will be used to run playbooks
<img width="764" alt="image" src="https://github.com/user-attachments/assets/a8068daf-ddc9-4e72-ab48-c56c43753bc6">

- **Create a new GitHub repository named `ansible-config-mgt`.**
   <img width="872" alt="image" src="https://github.com/user-attachments/assets/e5f5ecc4-ee10-4208-a15b-2cf7f41c77d2">

- **Install Ansible:**
   ```sh
   sudo apt update && sudo apt upgrade -y
   sudo apt install ansible -y
   ```
- Verify Ansible installation:
    ```sh
    ansible --version
    ```
<img width="525" alt="image" src="https://github.com/user-attachments/assets/b2d42fbb-ebf3-468d-893d-b0ed8c1d3959">

- **Configure Jenkins build job:**
  - Create a new freestyle project `ansible` in Jenkins and point it to your `ansible-config-mgt` repository.
  <img width="812" alt="image" src="https://github.com/user-attachments/assets/0738d02e-c214-43b8-9a3d-8e2fb8d97c33">

- Configure a webhook in GitHub to trigger the `ansible` build.
  <img width="829" alt="image" src="https://github.com/user-attachments/assets/a4b54907-bd63-4bce-9670-12da135ae3f7">

- Configure SCM and Build Triggers
 <img width="855" alt="image" src="https://github.com/user-attachments/assets/ac30fc5e-e376-4ec7-ab37-1d7a941e3a8c">  
 <img width="893" alt="image" src="https://github.com/user-attachments/assets/5b23f718-96ce-42e4-ac66-a27487dd115c">

- Configure a post-build job to save all files.
 <img width="894" alt="image" src="https://github.com/user-attachments/assets/f42554b7-7068-4307-9f0b-397e1e0731cf">

- **Test the setup:**
  - Make changes to the `README.md` file in the `main` branch.
  <img width="863" alt="image" src="https://github.com/user-attachments/assets/db659859-de23-4fa0-8812-080295822b56">

  - Ensure builds start automatically and Jenkins saves the files in:
  <img width="903" alt="image" src="https://github.com/user-attachments/assets/d27fb12d-4a49-45bf-96ee-a42e2bae6cd5">

    ```sh
    ls /var/lib/jenkins/jobs/ansible/builds/1/archive/
    ```
    <img width="447" alt="image" src="https://github.com/user-attachments/assets/f7fd79ea-de13-4cfc-8eb8-e520ec5979fe">

    Now our setup architecture looks like this:
    <img width="592" alt="image" src="https://github.com/user-attachments/assets/a0264c06-a9b8-4d9b-b2d8-632046a3c234">  
**Tip**: Every time you stop/start your Jenkins-Ansible server - you have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an Elastic IP to your Jenkins-Ansible server (you have done it before to your LB server in Project 10). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

## Step 2: Prepare Your Development Environment Using Visual Studio Code
- Install Visual Studio Code (VSC) On the Local Machine. To do so click this [link](https://code.visualstudio.com/learn/get-started/basics).

- Configure VSC to connect to your GitHub repository.
    ```sh
    git config --global user.email "gideonsango@gmail.com"
    git config --global user.name "gideonsngo"
    ```

- Clone the ansible-config-mgt repository to your Jenkins-Ansible instance:
    ```sh
    git clone <ansible-config-mgt-repo-link>
    ```
  <img width="805" alt="image" src="https://github.com/user-attachments/assets/c0282405-7e31-4ff4-8dc6-b93ba2297b9c">
