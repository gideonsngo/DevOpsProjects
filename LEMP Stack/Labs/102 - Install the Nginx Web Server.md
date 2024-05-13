# 102 - Install the Nginx Web Server
After creating a new EC2 Instance with Ubuntu OS and t2 micro, an SSH connection was established.     
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/b94f1fdc-1f00-41d8-a1d6-6a11064c2ecc)

While connection has been established to the Ubuntu Server, It is important to install the nginx web server on the host machine.

Before Installing Nginx Server using ubuntu's package manager **apt** with the following command:
Update a list of packages in the package manager: ```sudo apt update```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/b4edd417-41ad-49a4-aa66-94e40fcf983b)

The above command could show you that you have upgradable packages. 
Run the following command to upgrade the upgradable packages to the latest versions: ```sudo apt upgrade```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/85cc07e4-00a4-4907-b6a3-a382e09fae89)

Run Nginx package installation: ```sudo apt install nginx``` when prompted answer using y  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/3593d162-b31e-4b98-a003-1b58b7a90d06)

After successful installation, verify that the nginx service is running on the OS, the following command was run: ```sudo systemctl status nginx```
If the service is running, it should show service enabled and active (running)  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/82e77104-3166-4595-baff-7f2aab1b5799)

Open inbound traffic through port 80 by adding the rule through the AWS interface or by running the command ```sudo ufw allow 80/tcp```

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2e3c9727-b586-45da-a6eb-8d1010d85835)  

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/5677b6f3-6a90-4e17-886f-fed8d63067e3)

Test to check if the server can be accessed through the Ubuntu shell, by running the command: 
```curl http://localchost:80```   
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/5d21c961-06e8-409f-94e7-27f9c07c3567)  

You can also view it by opening the public IP in the browser:
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/de2ac2d3-a33b-4c32-bb56-0d35b1914491)

