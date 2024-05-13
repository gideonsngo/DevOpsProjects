# 106 - Creating a Virtual Host
After installing LAMP stack, It is important to create a virtual host. Virtual hosting allows for one server to be used in hosting multiple domain names.

Create a directory called **projectlamp** which was used to point as the directory for the virtual host to be created using the **mkdir** command: ```sudo mkdir /var/www/projectlamp```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/1b15a71c-e814-4611-b4a3-9ab2e15b60e0)

Ownership of the newly created directory was changed to the current system user using the environment variable $USER: ```sudo chown -R $USER:$USER /var/www/projectlamp```

A new configuration file is then created and edited using the VIM editor in the sites-available folder of Apache: ```sudo vi /etc/apache2/sites-available/projectlamp.conf```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a8024a78-f093-4198-aa3e-e80fbe7687ee)


In the newly created file opened in the VIM editor, add the following code to create the virtual host:
>     <VirtualHost *:80>
>     		ServerName projectlamp
>     		ServerAlias www.projectlamp
>     		ServerAdmin webmaster@localhost
>     		DocumentRoot /var/www/projectlamp
>     		ErrorLog ${APACHE_LOG_DIR}/error.log
>     		CustomLog ${APACHE_LOG_DIR}/access.log combined
>     </VirtualHost>

Enable the new virtual host by running the command: ```sudo a2ensite projectlamp```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ec16f13b-1556-4cb7-adc2-0c4b60c6c652)

Disable the default apache website by running the command: ```sudo a2dissite 000-default```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/742430eb-f098-4f8a-b886-e9a724f73480)

Restart the Apache Service for changes to take effect: ```sudo systemctl reload apache2```

Create index.html in the new /var/www/projectlamp directory: ```sudo vi /var/www/projectlamp/index.html```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/086334ae-f45c-4b22-9f66-b78c04380976)

> [!NOTE]
> vi and vim are used interchangeably to open the vim editor.  
> Virtual Host has now been created.


