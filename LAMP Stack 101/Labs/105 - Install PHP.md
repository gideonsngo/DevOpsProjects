# 105 - Install PHP
After installing the MySQL server, It is important to install PHP.
Installing PHP handles the code to display dynamic content to the user. Other modules needed to be installed alongside PHP are ```php-mysql``` which allows PHP to communicate with MySQL-based databases and ```libapache2-mod-php``` which enables Apache to handle PHP files.  
Install PHP and the two additional modules automatically using ubuntu's package manager **apt** by running the command: ```sudo apt install php libapache2-mod-php php-mysql```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/e3319407-8597-4748-ad40-a6500695558e)


After installation is complete, confirm by checking the php version installed. Run the command : `php -v`
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/3c8ed7a8-ad6c-4002-8bb6-42a6ae5dc64d)

> [!NOTE]
> PHP has now been installed and configured. LAMP is completely installed.

