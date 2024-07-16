# 104 - Ansible Refactoring & Static Assignments (Imports and Roles)
## Step 4 - Reference the `webserver` Role

### Create `uat-webservers.yml` Assignment
- **Inside `static-assignments`, create a new playbook `uat-webservers.yml` to reference the `webserver` role:**
    
    ```yaml
    ---
    - hosts: uat-webservers
      roles:
        - webserver
    ```

### Update `site.yml` to Include `uat-webservers.yml`
- **Modify `playbooks/site.yml` to include the `uat-webservers.yml` playbook:**
    
    ```yaml
    ---
    - hosts: all
    - import_playbook: ../static-assignments/common-del.yml

    - hosts: uat-webservers
    - import_playbook: ../static-assignments/uat-webservers.yml
    ```

---

## Step 5 - Commit & Test

### Commit Your Changes
- **Commit and push your changes to the repository:**
    
    ```sh
    git add .
    git commit -m "Refactored Ansible code and added roles for UAT webservers"
    git push origin refactor
    ```

- **Create a Pull Request and merge changes to the `main` branch.**
  
  - Create PR:  
  - Merge Changes:
    <img width="719" alt="image" src="https://github.com/user-attachments/assets/6e46b9ed-57e8-490b-bad4-65525b786281">

### Verify Jenkins Jobs
- **Ensure the webhook triggers two consecutive Jenkins jobs.**
- **Confirm that files are copied to `/home/ubuntu/ansible-config-artifact/`.**

### Run the Playbook Against the `uat` Inventory
- **Execute the playbook:**
    
    ```sh
    cd /home/ubuntu/ansible-config-artifact
    ansible-playbook -i inventory/uat.yml playbooks/site.yml
    ```
    <img width="550" alt="image" src="https://github.com/user-attachments/assets/35b55f1a-c2d1-45ee-b438-2f3d166d1f8f">

### Verify Web Server Configuration
- **Access the UAT web servers from your browser:**
    - `http://<Web1-UAT-Server-Public-IP-or-Public-DNS-Name>/index.php`
     <img width="913" alt="image" src="https://github.com/user-attachments/assets/4ba94cb9-5d38-454b-bde6-7e7742df84dd">

    - `http://<Web2-UAT-Server-Public-IP-or-Public-DNS-Name>/index.php`
     <img width="910" alt="image" src="https://github.com/user-attachments/assets/50956e6d-b409-46bc-872a-a2e9a93bf700">

Our Ansible architecture now looks like this:
<img width="753" alt="image-53" src="https://github.com/user-attachments/assets/585e676a-274f-4ea5-9dfb-43f52abb7c98">


### Conclusion
Congratulations! You have successfully deployed and configured UAT Web Servers using Ansible imports and roles.

---

### Blocker
- I encountered a blocker while doing a test run to save a copy of artifact/files on the jenkins server into `/home/ubuntu/ansible-config-artifact` directory. I noticed the `save-artifatc` job build failed, i got `AccessDeniedException` error:
 
 **Resolution**:

 ```sh
 sudo usermod -a -G jenkins ubuntu
 sudo chmod 777 /home/ubuntu/ansible-config-artifact
 sudo chown ubuntu:jenkins /ansible-config-artifact
 ```

---
