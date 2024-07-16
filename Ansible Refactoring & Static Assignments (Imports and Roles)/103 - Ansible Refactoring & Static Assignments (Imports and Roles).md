# 103 - Ansible Refactoring & Static Assignments (Imports and Roles)
## Step 3 - Configure UAT Webservers with a Role 'Webserver'

### Launch UAT Servers
- **Launch 2 new EC2 instances using the RHEL 9 image.**
- **Name them `Web1-UAT` and `Web2-UAT`.**
 <img width="755" alt="image" src="https://github.com/user-attachments/assets/3daabea0-a88e-41d4-a4dd-f451216de778">

### Create the Role Structure
- **Create a `roles` directory:**

  - Create the folder for `roles` manually in the `static-assignments` directory
 
    ```sh
    cd static-assignments
    mkdir roles
    cd roles
    ansible-galaxy init webserver
    ```
- Alternatively, create the directory structure manually. The initial structure should be:
    
    ```
    └── webserver
        ├── README.md
        ├── defaults
        │   └── main.yml
        ├── handlers
        │   └── main.yml
        ├── meta
        │   └── main.yml
        ├── tasks
        │   └── main.yml
        └── templates
    ```
    ![Role Directory](./images/role-folder-structure.PNG)

    - Final Folder Structure:
     ![Structure](./images/final-folder-structure.PNG)

### Update UAT Inventory
- **Update the `ansible-config-mgt/inventory/uat.yml` with the IP addresses of the UAT servers:**
    
    ```ini
    [uat-webservers]
    <Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
    <Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
    ```

### Configure the `webserver` Role
- **Add `roles_path` in `ansible.cfg` file:**
    
    ```ini
    roles_path = /home/ubuntu/ansible-config-mgt/roles
    ```

- **Add tasks in `roles/webserver/tasks/main.yml`:**
    
    ```yaml
    ---
    - name: install apache
      become: true
      ansible.builtin.yum:
        name: "httpd"
        state: present

    - name: install git
      become: true
      ansible.builtin.yum:
        name: "git"
        state: present

    - name: clone a repo
      become: true
      ansible.builtin.git:
        repo: https://github.com/<your-name>/tooling.git
        dest: /var/www/html
        force: yes

    - name: copy html content to one level up
      become: true
      command: cp -r /var/www/html/html/ /var/www/

    - name: start service httpd, if not started
      become: true
      ansible.builtin.service:
        name: httpd
        state: started

    - name: recursively remove /var/www/html/html/ directory
      become: true
      ansible.builtin.file:
        path: /var/www/html/html
        state: absent
    ```

---
