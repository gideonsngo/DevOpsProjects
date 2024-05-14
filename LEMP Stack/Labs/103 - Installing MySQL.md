# 103 Installing MySQL
After installing the Nginx web server, It is important to install the MySQL database.
Instal MySQL Server using ubuntu's package manager **apt** with the following command:
Run MySQL package installation: ```sudo apt install mysql-server```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c50513cd-8993-4e3e-a0de-7f7b3b97708c)

After successful installation, verify that the mysql service is running on the OS, the following command was run: ```sudo systemctl status mysql```
If the service is running, it should show service enabled and active (running) in green.  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2b8f8a8d-36f7-4d3c-850f-d460e7400213)

Log unto mysql using the command: ```sudo mysql``` to connect as the admin database **root** user  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2f42fd48-d8dc-4d32-b274-ae1e1a18efd6)

As a security measure, it is recommended to change the root user password for mysql with the following command: ```ALTER USER 'root'@'localhost IDENTIFIED WITH mysql_native_password BY 'PassWord.1';``` where ```PassWord.1``` was the password set for the account.  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/48f28008-6ead-4fe7-b130-5ec10d5b1266)

Exit MySQL by running the command: ```exit```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/77983fc5-98d7-4b4e-81c7-5774f20c8a94)

Run a security script that removes insecure default settings allowing for database root password to access the database: ```sudo mysql_secure_installation```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/e55d5fdd-d1e7-4fe1-805a-dd39d987e7ba)

Enabling this will activate the VALIDATE PASSWORD PLUGIN which essentially sets up security policies for passwords in mysql. Answer Yes or Y for anything else to continue. You will be asked to select a level of password validation and the complexity of each level will be displayed.  

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7af00ea1-f627-40b5-a191-872bebd25d94)  

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2ee67b9d-862a-494d-b051-c80f646750ef)


After running script, use the following command to login to mysql which now requires the password used after changing the root user password: ```sudo mysql -p```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c5fe09d5-9745-4837-8cbc-6e4a3fc5cbfc)


> [!NOTE]
> MySQL has now been installed and configured.
