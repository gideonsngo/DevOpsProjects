# 104 - Install PHP
After installing the MySQL server, It is important to install PHP.  

Installing PHP handles the code to display dynamic content to the user. Other modules needed to be installed alongside PHP are ```php-fpm``` (PHP fastCGI process manager) which tells Nginx to pass PHP requests to the software for processing acting as a bridge and ```php-mysql``` that allows PHP to communicate with MySQL-based databases. 
   
Install PHP and the two additional modules automatically using ubuntu's package manager **apt** by running the command: ```sudo apt install php-fpm php-mysql```    
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c903b7a4-1803-4fee-9a59-8c39d235d172)

After installation is complete, confirm by checking the php version installed. Run the command : `php -v`  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f2fcdc33-09cc-4cb0-b092-69b9b180c33b)


> [!NOTE]
> PHP has now been installed and configured. LEMP is completely installed. There is a need to configure Nginx
