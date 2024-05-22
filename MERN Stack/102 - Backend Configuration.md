# 102 - Backend Configuration
Create an EC istance with Ubuntu OS and t2 micro configuration and SSH to the machine  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/863eaef7-61f1-4d17-b46e-3b219b28dd22)  

Update ubuntu and check for upgrades using the apt manager by running the following command ```sudo apt update```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/55f1231e-41a7-4b20-a395-359bfd5bc7a4)  

Upgrade ubuntu ```sudo apt upgrade```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0c5aa8c2-98c5-4d42-978b-6021423b0586)

Get the location of Node.js software from ubuntu repos by running the command ```curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d2c50129-e289-48df-b8b4-22d5abdd025d)  

Install Node.js on the server by running the command: ```sudo apt-get install -y nodejs```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/86aa5d13-0cdc-49eb-9183-4d5e24586c52)  

This installs both ```nodejs``` and ```npm``` (which is a package manager for node used to install modules and packages and manage depencendies)  

Verify the node installation with the command: ```node -v```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/45ec100c-10cb-41b1-baa2-57ec38e915ef)  

Verify installation npm installation with the command ```npm -v```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/9df3a8ea-b9fe-4e19-af06-3f6aad713cd7)  

Create a new directory for your To-Do project: ```mkdir Todo``` and run the command ```ls``` to confirm the creation of the directory  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6a606430-f4c3-49f4-af3e-148e90dda1e9)  

Change the current directory to the project directory by running the command: ```cd Todo```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/65015305-c568-4c07-b9d3-58776964f201)  

Run the command ```npm init``` to initiaize the project which creates a new file ```package.json``` that contains information about the application and dependencies it needs. Press ```enter``` several times to accept the default values then type ```yes``` to accept to write out the ```package.json``` file  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ffbad31b-4c2d-428e-a97a-61e0a26e5b06)  

Run the command ```ls``` to confirm the file has been created in the directory  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/02eedbab-0f2b-4b4c-ac4d-e87fa6054306)










