# 103 - Install Apache
While connection has been established to the Ubuntu Server, It is important to install the apache web server on the host machine.
Installed Apache Server using ubuntu's package manager **apt** with the following command:
Update a list of packages in the package manager: ```sudo apt update```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/df536295-6cf4-4af7-b701-24342802e432)

Run apache package installation: ```sudo apt install apache2```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/b8c2ab56-9489-4fe5-ac8c-138e5e1df05a)

After successful installation, verify that the apache2 service is running on the OS, the following command was run: ```sudo systemctl status update```
If the service is running, it should show service enabled and active (running)  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/195f6971-8242-45c3-bbcc-5f7f9199027a)

Open inbound traffic through port 80 by adding the rule through the AWS interface or by running the command ```sudo ufw allow 80/tcp```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7168c323-a523-4625-9b94-27dfd9b9b8d2)

Test the Connection running the command: ```curl http://localhost:80``` or ```curl http://127.0.0.1:80```
The output should look like this:
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/fc08e1de-fc49-4184-90e3-6572f516f65f)


> [!NOTE]
> Apache web server has now been installed!!
