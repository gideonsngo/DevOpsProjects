# 104 - Install MySQL
After installing the apache web server, It is important to install the MySQL database.
Installed MySQL Server using ubuntu's package manager **apt** with the following command:
Run MySQL package installation: ```sudo apt install mysql-server```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f5de93f9-d872-4cd6-b6f3-43946e4a61fe)


After successful installation, verify that the mysql service is running on the OS, the following command was run: ```sudo systemctl status mysql```
If the service is running, it should show service enabled and active (running) in green.  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8bbc8233-7d58-40d6-b075-3cfd274bbd24)


Log unto mysql using the command: ```sudo mysql``` to connect as the admin database **root** user  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/1e153a60-4d9a-42ef-ab19-dac1b6892d82)


Change the root user password for mysql with the following command: ```ALTERUSER'root'@'localhost IDENTIFIED WITH mysql_native_password BY'PassWord.1';``` where ```PassWord.1``` was the password set for the account.  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f0212d21-a41b-427a-9e8f-9c5fbd4b2c34)


Run a security script that removes insecure default settings allowing for database root password to access the database: ```sudo mysql_secure_installation```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/bf86c009-d7de-41d7-8f3d-e531c9d1f240)


After running script, use the following command to login to mysql which now requires the password used after changing the root user password: ```sudo mysql -p```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f26d044e-c88a-44a9-ba27-287b15c23d69)


> [!NOTE]
> MySQL has now been installed and configured.

