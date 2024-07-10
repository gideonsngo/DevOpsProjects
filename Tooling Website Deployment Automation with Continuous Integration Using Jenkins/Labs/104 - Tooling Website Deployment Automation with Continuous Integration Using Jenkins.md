# 104 - Tooling Website Deployment Automation with Continuous Integration Using Jenkins
## Step 3: Configure Jenkins to Copy Files to NFS Server via SSH

- **Install Publish Over SSH Plugin in Jenkins**

  - Go to `Manage Jenkins` > `Manage Plugins` > `Available`, search for `Publish Over SSH,` and install it.
   <img width="943" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7f4b6286-3a0f-4b10-b72e-a0a8b98993f3">
   <img width="691" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/49daf9d3-cac1-4e07-9574-fcab9c3fb730">
   
- **Configure SSH in Jenkins**

  - Go to `Manage Jenkins` > `Configure System`.
  
  - Scroll down to `Publish over SSH`, add the SSH server details (hostname, username, remote directory `/mnt/apps`, and private key for authentication).
   <img width="769" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7a149d1e-835a-49b3-a097-2c4d816d9433">
  
  - Test the configuration to ensure it connects successfully.
   <img width="844" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/88e41e6a-5a07-4994-bf59-94a4e7cf9f8c">

- **Add post-build action to copy artifacts to NFS server**

  - In the project configuration, under `Post-build Actions`, select `Send build artifacts over SSH`.
   <img width="590" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/e0db806a-e3e5-4760-9356-bcbb572b3812">
  
  - Choose the configured SSH server, specify the source files (`**`), and the remote directory.
   <img width="905" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/3d67802e-0c0c-4075-bd7b-8158000e6ba8">

- **Test the full pipeline**

  - Make another change in your GitHub repository (e.g., update `README.md`) and push it to the master branch.
  
  - Jenkins should trigger a new build, archive the artifacts, and transfer them to the NFS server.
   <img width="802" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/846b3ed7-3812-477a-8e62-121e75264a41">
  
  - Verify the update on the NFS server by checking the contents of `/mnt/apps/README.md`.
    ```sh
    sudo cat /mnt/apps/README.md
    ```
    <img width="456" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/090f9ce1-c51a-4f48-b54d-893ccea41ce3">


**Blocker:**
- After configuring the Jenkins Server to connect to the NFS server and getting a success message after the test configuration returned a success the message, commit operation from the git repository failed to deploy the artifact to the NFS server folder `/mnt/apps/`:
  <img width="896" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/846b9278-7a31-4a5d-a0c3-f22c5f35848c">

- I was able to resolve the issue after modifiying the permissions on the `mnt/apps` directory of the NFS server
    ```sh
    sudo chown -R ec2-user:ec2-user /mnt/apps
    sudo chmod -R 777 /mnt/apps
    
    ```
    
### Conclusion

Congratulations! You have successfully implemented a continuous integration solution using Jenkins CI. This setup ensures that any change in your GitHub repository is automatically deployed to your NFS server, streamlining your development and deployment process.

