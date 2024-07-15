# 105 - Ansible Configuration Management (Automate Project 7 to 10)
## Step 7: Run the First Ansible Test
- **SSH into your Jenkins-Ansible instance Using the VSC `SSH-Agent`.**
  You have already Configure SSH keys for Ansible in an earlier step.  
  - SSH into your Jenkins-Ansible server using `ssh-agent`:
    ```sh
    ssh -A ubuntu@<public-ip>
    ```
    <img width="523" alt="image" src="https://github.com/user-attachments/assets/ebf5b058-3beb-42df-905a-18fe4265ea28">
 
- **Run your playbook:**
    ```sh
    #navigate to ansible build files
    cd /var/lib/jenkins/jobs/ansible/builds/1/archive/
    
    #run the playbook command
    ansible-playbook -i inventory/dev.yml playbooks/common.yml
    ```
    <img width="662" alt="image" src="https://github.com/user-attachments/assets/4c061089-0ee9-454d-ad39-f1d1770a75b6">

  - Verify that `wireshark` is installed on each of the servers by running:
    ```sh
    which wireshark
    wireshark --version
    ```
    - Output Web Server
     <img width="443" alt="image" src="https://github.com/user-attachments/assets/32981f2d-08b3-495e-9823-102179179d83">

    - Output Database Srver
     <img width="440" alt="image" src="https://github.com/user-attachments/assets/62549fa7-5ef9-4681-b384-a1e709407acf">

    - Output Load-Balancer
     <img width="443" alt="image" src="https://github.com/user-attachments/assets/b1555897-3344-45bc-b5a9-bf55ca2fa634">

    - Output NFS Server
     <img width="443" alt="image" src="https://github.com/user-attachments/assets/57775897-6936-4f33-bb06-584223df0998">

## Optional Step: Repeat the Process
- Update your Ansible playbook with new tasks and repeat the full cycle of:
    > Checkout -> Change codes -> Commit -> PR -> Merge -> Build -> ansible-playbook

Your update architecture with Ansible now looks like this:  
<img width="644" alt="image" src="https://github.com/user-attachments/assets/05a6cb67-ccaa-4f6d-9361-656a4e16c307">

## Conclusion
Congratulations! You have successfully automated routine tasks using Ansible. This guide has walked you through the process of setting up Ansible, creating and running playbooks, and automating server configurations.

---
