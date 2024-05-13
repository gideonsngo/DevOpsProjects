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

As a security measure, it is recommended to change the root user password for mysql with the following command: ```ALTERUSER'root'@'localhost IDENTIFIED WITH mysql_native_password BY'PassWord.1';``` where ```PassWord.1``` was the password set for the account.  


Run a security script that removes insecure default settings allowing for database root password to access the database: ```sudo mysql_secure_installation```  


After running script, use the following command to login to mysql which now requires the password used after changing the root user password: ```sudo mysql -p```  



> [!NOTE]
> MySQL has now been installed and configured.
