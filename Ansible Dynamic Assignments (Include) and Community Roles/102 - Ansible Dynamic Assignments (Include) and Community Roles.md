# 102 - Ansible Dynamic Assignments (Include) and Community Roles
### Step 1

- **Create a New Branch and Folder:**

    First, let's start by creating a new branch named `dynamic-assignments` in your GitHub repository. Then, create a folder named `dynamic-assignments` and add a file `env-vars.yml`.

    ```sh
    # Navigate to your repository
    cd ansible-config-mgt

    # Create a new branch
    git checkout -b dynamic-assignments

    # Create the dynamic-assignments directory and the env-vars.yml file
    mkdir dynamic-assignments
    touch dynamic-assignments/env-vars.yml
    ```
    
- **Set Up Directory Layout:**

    Next, we'll create the directory structure needed for this project. Create these directory and files.

    ```sh
    # Create env-vars directory and its files
    mkdir env-vars
    touch env-vars/dev.yml env-vars/stage.yml env-vars/uat.yml env-vars/prod.yml
    ```
    - Update the `env-vars/uat.yml` file with the code below:
    
        ```yml
        ---
        load_balancer_is_required: true
        enable_nginx_lb: true
        enable_apache_lb: false
        ```


    Your GitHub repository should have the following structure:

    ```arduino
    ansible-config-mgt/
    ├── dynamic-assignments
    │   └── env-vars.yml
    ├── env-vars
    │   ├── dev.yml
    │   ├── stage.yml
    │   ├── uat.yml
    │   └── prod.yml
    ├── inventory
    │   ├── dev
    │   ├── stage
    │   ├── uat
    │   └── prod
    ├── playbooks
    │   └── site.yml
    └── static-assignments
        ├── common.yml
        └── uat-webservers.yml
    ```
    Then do a `push`, create a `pull-request` and `merge` to main branch.

    ```sh
    git add .

    git commit -m "your-commit-message"

    git push origin dynamic-assignments
    ```

    <img width="602" alt="image" src="https://github.com/user-attachments/assets/692c9594-bc93-4018-80da-425c560897e2">

    > **Note:** The above folder structure is a result of the previous project `Ansible-Refactoring-Static-Assignment`.

- **Configure Dynamic Assignment:**
    
    Let's configure the `env-vars.yml` file to dynamically include environment-specific variables.

    Open `dynamic-assignments/env-vars.yml` and add the following content:

```yml
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always
```

- **Update site.yml for Dynamic Assignments:**

    Next, update the `site.yml` file to include the dynamic assignments. Open playbooks/site.yml and update it as follows:

    ```yml
    ---
    - hosts: all
    - name: Include dynamic variables 
      tasks:
      import_playbook: ../static-assignments/common.yml 
      include: ../dynamic-assignments/env-vars.yml
      tags:
        - always
    
    - hosts: webservers
    - name: Webserver assignment
      import_playbook: ../static-assignments/webservers.yml
    ```
