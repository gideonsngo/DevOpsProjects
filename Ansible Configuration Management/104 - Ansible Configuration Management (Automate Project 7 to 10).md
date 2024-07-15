# 104 - Ansible Configuration Management (Automate Project 7 to 10)
## Step 5: Create a Common Playbook
- **Edit `playbooks/common.yml` to include the following tasks:**
    ```yml
    -  name: update web and nfs servers
       hosts: webservers, nfs
       become: yes
       tasks:
	  - name: ensure wireshark is at the latest version
	    yum:
	      name: wireshark
	      state: latest

    -  name: update LB and db servers
       hosts: lb, db
       become: yes
       tasks:
	  - name: Update apt repo
	    apt:
	      update_cache: yes

	  - name: ensure wireshark is at the latest version
	    apt:
	      name: wireshark
	      state: latest
    ```
Feel free to add additional tasks such as `creating a directory`, `changing the timezone`, or `running shell scripts`.

- **Here is the full `common.yml` playbook with all the additional tasks included:**
    ```yml
    - name: update web and nfs servers
      hosts: webservers, nfs
      become: yes
      tasks:
        - name: ensure wireshark is at the latest version
          yum:
            name: wireshark
            state: latest

        - name: create a directory
          file:
            path: /path/to/directory
            state: directory
            mode: '0755'

        - name: add a file into the directory
          copy:
            content: "This is a sample file content"
            dest: /path/to/directory/samplefile.txt
            mode: '0644'

        - name: set timezone to UTC
          timezone:
            name: UTC

        - name: run a shell script
          shell: |
            #!/bin/bash
            echo "This is a shell script"
            echo "Executed on $(date)" >> /path/to/directory/script_output.txt

    - name: update LB and db servers
      hosts: lb, db
      become: yes
      tasks:
        - name: Update apt repo
          apt:
            update_cache: yes

        - name: ensure wireshark is at the latest version
          apt:
            name: wireshark
            state: latest

        - name: create a directory
          file:
            path: /path/to/directory
            state: directory
            mode: '0755'

        - name: add a file into the directory
          copy:
            content: "This is a sample file content"
            dest: /path/to/directory/samplefile.txt
            mode: '0644'

        - name: set timezone to UTC
          timezone:
            name: UTC

        - name: run a shell script
          shell: |
            #!/bin/bash
            echo "This is a shell script"
            echo "Executed on $(date)" >> /path/to/directory/script_output.txt
    ```

## Step 6: Update GIT with the Latest Code
- **Commit your code chnages to GitHub:**
    ```sh
    git status
    git add .
    git commit -m "commit message"
    git push origin <branch-name>
    ```
  <img width="536" alt="image" src="https://github.com/user-attachments/assets/81c225b0-dc88-4693-a7ff-852c3bc796ea">
  <img width="530" alt="image" src="https://github.com/user-attachments/assets/00734928-da9c-470f-837c-5b7c7c0c6dab">

- **Create a pull request and review the changes.**
  - Open your GitHub repository.
   <img width="928" alt="image" src="https://github.com/user-attachments/assets/7cfb3414-9611-4293-a96e-302c63fd396c">

  - Create a pull request for the `feature` branch.
  <img width="932" alt="image" src="https://github.com/user-attachments/assets/030ec9cc-8bd2-4398-99f1-68ceec43091a">

  - Act as a reviewer and merge the code if everything is satisfactory.
   <img width="902" alt="image" src="https://github.com/user-attachments/assets/f97f6d4b-b71d-4a60-a9ba-70956673f6db">

  - Merge the pull request to the `master` branch.
   <img width="872" alt="image" src="https://github.com/user-attachments/assets/77b73034-4c7d-42f5-91be-164ba6622fd3">

- **Pull the Latest Changes On Local Repository:**
    - Checkout from the feature branch into the master branch.
        ```sh
        git checkout main
        git pull origin main
        ```
        <img width="644" alt="image" src="https://github.com/user-attachments/assets/5bcd7683-b56f-4af9-b7f2-fc077c1104e2">
