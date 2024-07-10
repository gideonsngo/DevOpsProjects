# 103 - Tooling Website Deployment Automation with Continuous Integration Using Jenkins
## Step 2: Configure Jenkins to Retrieve Source Codes from GitHub using Webhooks

- **Enable webhooks in the GitHub repository settings**
  - Go to your GitHub repository, navigate to `Settings` > `Webhooks` > `Add webhook`.
  
  - Set the payload URL to `http://<jenkins-server-public-ip>:8080/github-webhook/` and choose `application/json` as the content type.
  
  - Select `Just the push event` and add the webhook.
  <img width="942" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a261fad9-a0e3-4479-a41e-4804b62fca15">
 
- **Create a Freestyle project in Jenkins**

  - In the Jenkins dashboard, click `New Item`, name it `Tooling_GitHub`, and select `Freestyle project`.
  <img width="926" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a51dc5eb-37b2-418c-83c2-6ee1d5845d11">
  
  - In the project configuration, under `Source Code Management`, choose `Git`.
  
  - Provide the Git repository URL and add credentials if required.
   <img width="913" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/bbd574c6-c94c-43af-966a-bfc41cdc6591">

  - Run the Build manually on Jenkins Console to see if the GitHub is connceted.
   <img width="938" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/b523fccb-e436-4dc6-a953-3ae4e5bfd826">  
   <img width="812" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7cb443b6-ad61-45cf-b7c5-bde07cf0fced">  

- **Configure build triggers and post-build actions**

  - In the project configuration, under `Build Triggers`, select `GitHub hook trigger for GITScm polling`.
   <img width="941" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8e1ddc4c-24ad-4fb4-a6fe-5eaeedb0abfd">

  - Under `Post-build Actions`, select `Archive the artifacts` and specify `**` to archive all files.
   <img width="511" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f83dba5b-9b68-43d9-ac21-eb97c94b8765">
   <img width="593" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/672f7639-17c2-4594-89d5-e53825cb653a">  

- **Test the configuration**

  - Make a change in your GitHub repository (e.g., update `README.md`) and push it to the master branch.

  - Jenkins should automatically trigger a build and archive the files.
   <img width="815" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/72399877-2923-4584-95f9-bc21d5f9a37d">

  - Verify that the artifact is stored on Jenkins Server locally
    ```sh
    ls /var/lib/jenkins/jobs/tooling_github/builds/<build-number>/archive/
    ```
    <img width="496" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c6553250-2060-4f5a-b8f0-3a68ac7f9151">
