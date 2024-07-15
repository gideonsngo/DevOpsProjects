# 103 - Ansible Configuration Management (Automate Project 7 to 10).
## Step 3: Begin Ansible Development
- **Create a new branch in your GitHub repository for development.**
  
  ```sh
  #Check existing branches
  git branch

  #Create new branch
  git branch <branch-name>
  ```

  - Checkout the new branch on your local machine.
    
    ```sh
    git checkout <new-branch>

    git branch  ---> #To confirm you switched branch
    ```
   
- **Create a directory structure:**

    - `playbooks` directory to store playbooks.
    - `inventory` directory to organize hosts.
      
      ```sh
      mkdir playbooks inventory
      ```

- **Create your first playbook:**

    - `playbooks/common.yml`
      
      ```sh
      cd playbooks && touch common.yml
      ```

- **Create inventory files for each environment:**

    - `inventory/dev`, `inventory/staging`, `inventory/uat`, and `inventory/prod`.
      
      ```sh
      cd inventory && touch dev.yml staging.yml uat.yml prod.yml
      ```

  <img width="805" alt="image" src="https://github.com/user-attachments/assets/45c46beb-7426-4bc8-af82-3bddf694829b">

  - Add the following configuration:
    ```ini
    [defaults]
    host_key_checking = False
    ```

## Step 4: Set Up an Ansible Inventory

Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host - for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent:

- **SSH into your Jenkins-Ansible instance Using the VSC `SSH-Agent`.**
  -  Configure SSH keys for Ansible:
        ```sh
        eval `ssh-agent -s`
        ssh-add <path-to-private-key>
        ssh-add -l
        ```
     <img width="617" alt="image" src="https://github.com/user-attachments/assets/4af9556c-c6ff-4985-ac58-488d6079baaf">

  - SSH into your Jenkins-Ansible server using `ssh-agent`:
    ```sh
    ssh -A ubuntu@<public-ip>
    ```
    <img width="600" alt="image" src="https://github.com/user-attachments/assets/9f193476-a9cf-4072-9309-cc49ff1a18a5">

- **Update your inventory/dev file:**
    ```sh
    [nfs]
    <nfs-server-private-ip> ansible_ssh_user=ec2-user

    [webservers]
    <web-server1-private-ip> ansible_ssh_user=ec2-user
    <web-server2-private-ip> ansible_ssh_user=ec2-user

    [db]
    <database-private-ip> ansible_ssh_user=ubuntu

    [lb]
    <lb-private-ip> ansible_ssh_user=ubuntu
    ```
