# 102 - Ansible Refactoring & Static Assignments (Imports and Roles)

## Task Overview
In this project, you will continue working with the `ansible-config-mgt` repository to improve and refactor your Ansible code. You will create assignments and learn to use the imports functionality to organize and reuse tasks efficiently. Imports allow you to include previously created playbooks into new playbooks.

---

## Step 1 - Jenkins Job Enhancement

### Create a New Directory for Artifacts
- **Log in to your Jenkins-Ansible server via SSH.**
    
    ```sh
    ssh -i keypair.pem ubuntu@public-ip
    ```

- **Create a new directory to store artifacts after each build:**
    
    ```sh
    sudo mkdir /home/ubuntu/ansible-config-artifact
    ```
- **Change permissions to allow Jenkins to `save`, `write` and `execute` files:**
    
    ```sh
    sudo chmod -R 777 /home/ubuntu/ansible-config-artifact
    ```
- **Add `Jenkins` to `Ubuntu` group:**

    ```sh
    sudo usermod -a -G jenkins ubuntu

    #Considering 'ubuntu' is a group name with admin rights
    ```

### Install the Copy Artifact Plugin
- **Go to Jenkins web console.**

- **Navigate to `Manage Jenkins` > `Manage Plugins`.**
 <img width="890" alt="image" src="https://github.com/user-attachments/assets/a5baa516-51ef-4535-a14c-c6d448339a16">

- **In the `Available` tab, search for `Copy Artifact` and install it without restarting Jenkins.**
 <img width="825" alt="image" src="https://github.com/user-attachments/assets/b591e370-1e41-4182-85d2-8ade44fbd76b">
 <img width="752" alt="image" src="https://github.com/user-attachments/assets/14dfc3f7-a649-4961-9bdb-368e497c739b">

### Create a New Jenkins Freestyle Project
- **Create a new Freestyle project named `save_artifacts`.**
 <img width="830" alt="image" src="https://github.com/user-attachments/assets/7e2e02cf-9b69-4599-9cbb-0aa647e44c5c">

- **Configure it to be triggered by the completion of your existing `ansible` project.**
 <img width="689" alt="image" src="https://github.com/user-attachments/assets/61a87772-76d3-4add-b092-0423415c1e88">

- **Configure Job to Discard Old Builds for clean dev pipeline:**
 <img width="905" alt="image" src="https://github.com/user-attachments/assets/cffe4f0a-e496-4760-96ea-5f5159180f6f">
   
    - Add a `Build` step and choose `Copy artifacts from other project`.

    - Set `ansible` as the source project.
    
    - Set `/home/ubuntu/ansible-config-artifact` as the target directory.
    <img width="885" alt="image" src="https://github.com/user-attachments/assets/86fdb0ce-6cce-46e0-b813-75da6a4cdd99">

### Test the Setup
- **Make a change in the `README.md` file in the `main` branch of the `ansible-config-mgt` repository.**
  <img width="846" alt="image" src="https://github.com/user-attachments/assets/160cb4f7-6586-4830-80c3-25cdf320326f">

- **Verify that both Jenkins jobs complete and that files are updated in `/home/ubuntu/ansible-config-artifact` on the Jenkins server.**
    ```sh
    cd /home/ubuntu/ansible-config-artifact
    ls
    ```
    ![Verify Job Run](./images/verify-jenkins-job-copy-files.PNG)

---

## Step 2 - Refactor Ansible Code by Importing Other Playbooks into `site.yml`

### Setup the Repository
- **Pull the latest code from the `main` branch.**

- **Create a new branch named `refactor`:**
    
    ```sh
    git checkout -b refactor
    ```
    ![Create Branch](./images/create-new-branch-refactor.PNG)

### Create the `site.yml` File
- **Within the `playbooks` folder, create a new file named `site.yml` and add the code below into the file:**
    
    ```yaml
    ---
    - hosts: all
    - import_playbook: ../static-assignments/common.yml
    ```

### Organize Playbooks
- **Create a new folder named `static-assignments` at the root of the repository.**

- **Move `common.yml` to the `static-assignments` folder.**

### Folder Structure
**Your folder structure should look like this:**
    
```
    ├── static-assignments
	│   └── common.yml
	├── inventory
    		└── dev
    		└── stage
    		└── uat
    		└── prod
	└── playbooks
    		└── site.yml
```
![Folder Structure](./images/folder-structure-01.PNG)


### Create `common-del.yml` Playbook
- **Create a new playbook named `common-del.yml` in `static-assignments` to delete `wireshark`:**
    
    ```yaml
    ---
    - name: update web, nfs, and db servers
      hosts: webservers, nfs
      remote_user: ec2-user
      become: yes
      become_user: root
      tasks:
      - name: delete wireshark
        yum:
          name: wireshark
          state: removed

    - name: update LB server
      hosts: lb, db
      remote_user: ubuntu
      become: yes
      become_user: root
      tasks:
      - name: delete wireshark
        apt:
          name: wireshark
          state: absent
          autoremove: yes
          purge: yes
          autoclean: yes
    ```

### Update `site.yml`
- **Replace the import of `common.yml` with `common-del.yml`:**
    
    ```yaml
    ---
    - hosts: all
    - import_playbook: ../static-assignments/common-del.yml
    ```

### Run the Playbook
- **Run the playbook against the `dev` environment using the `ssh-agent` on VSC:**
    
    ```sh
    eval `ssh-agent -s`
    ssh-add <path-to-key>
    ssh -A ubuntu@public-ip


    cd /home/ubuntu/ansible-config-mgt/
    ansible-playbook -i inventory/dev.yml playbooks/site.yml
    ```
    ![Uninstall Wireshark](./images/wireshark-uninstalled.PNG)

- Ensure `wireshark` is deleted by running `wireshark --version` on the servers.
  
  - Output Webserver:
   ![Wireshark1](./images/nfs-uninstalled-wireshark.PNG)

  - Output LBserver:
   ![Wireshark2](./images/db-uninstalled-wireshark.PNG)
  

---
