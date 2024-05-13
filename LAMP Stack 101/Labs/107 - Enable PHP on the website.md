# 107 - Enable PHP on the website
Apache's default DirectoryIndex settings gives a filenamed ```index.html``` more precedence over ```index.php``` but this can be changed by editing the following: ```sudo vim /etc/apache2/mods-enabled/dir.conf```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/157d11bf-a418-4a6a-ae8e-b249661487af)  

Implement the following changes: 

    <ifModule mod_dir.c>
    		#Change this:
    		#DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
    		#To this:
    		DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
    </IfModule>

Reload apache for changes to take effect: ```sudo systemctl reload apache2```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/49999f8e-ede5-4fa3-892c-3800377cfea8)

Create a new file named index.php inside the custom root folder to test: ```sudo vim /var/www/projectlamp/index.php```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0e067aac-62af-43bd-8f25-bd970c4c62d4)

Add the following PHP code to the blank file opened in VIM editor: ```<?php  phpinfo ();``` and view the page by visiting the public IP or public DNS in the browser
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f25b54fd-f633-4a6f-9bc2-8ba8debc1b58)

Remove the index.php file leaving only the index.html by running the following command: ```sudo rm /var/www/projectlamp/index.php```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ff29e465-85f0-4e59-a3d8-adeed58cef6f)

Open the Public IP or Public DNS of the EC2 Instance to view the content of the new index.php
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f0c69494-0635-442d-834d-245c6955ce7a)

> [!NOTE]
> PHP has now been enabled on the website.
