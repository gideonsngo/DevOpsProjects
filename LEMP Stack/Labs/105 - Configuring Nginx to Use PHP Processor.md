# 105 - Configuring Nginx to Use PHP Processor
After creating the LEMP stack it is important to configure server blocks which are similar to Apache's virtual hosts which was used in LAMP stack deployment.
This enables the server to host more than one domain name,

The default Nginx server block directory is ```/var/www/html``` but we create a directory called **projectLEMP** which is used to point as the directory for the server block to be created using the **mkdir** command: ```sudo mkdir /var/www/projectLEMP```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a670f0aa-d174-47d2-83cc-ca960eed0a9a)

Ownership of the newly created directory is changed to the current system user using the environment variable $USER: ```sudo chown -R $USER:$USER /var/www/projectLEMP```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8699fed9-90f8-49c5-8b17-e4d6cba46c58)

A new configuration file is then created and edited using the VIM/Nano editor in the ```sites-available``` folder of Nginx: ```sudo nano /etc/nginx/sites-available/projectLEMP```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f7907b68-aca8-495e-bb81-ce43b4696b22)


In the newly created file opened in the VIM/Nano editor, add the following bare-bones configuration:
> #/etc/nginx/sites-available/projectLEMP
> 
> server {
>         listen 80;
>         server_name projectLEMP www.projectLEMP;
>         root /var/www/projectLEMP;
> 
>         index index.html index.htm index.php;
> 
>         location / {
>                 try_files $uri $uri/ =404;
>         }
> 
>         location ~ \.php$ {
>                 include snippets/fastcgi-php.conf;
>                 fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
>         }
> 
>         location ~ /\.ht {
>                 deny all;
>         }
> 
> }

### Glossary of directives and location blocks:  
 - ```listen```: This defines the port Nginx will listen on.   
 - ```root```: The document root where the files served by this website are stored
 - ```index```: Order of priority for Nginx index files for this website   
 - ```server_name```: The domain name/IP addresses this server block should respond for 
 - ```location /```: This includes a ```try_files``` directive, which checks for the existence of files or directories matching a URI request which displays a 404 error if appropriate resources are not find.   
 - ```location ~ \.php$```: This location block handles the actual PHP processing by pointing Nginx to the ```fastcgi-php.conf``` file and the ```php7.4-fpm.sock``` file, which declares what socket is associated with ```php-fpm``` 
 - ```location ~/\.ht```: Deals with ```.htaccess``` files which Nginx does not process. Adding the deny all directive, it will not display any ```.htaccess``` files that find their way into the folder.

Enable the new configuration by linking to the config file from Nginx's ```sites-enabled``` directory which will be used when Nginx is reloaded by running the command: ```sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a2580201-d627-4040-8f28-cfab80f4b13d)

Test Configuration  for syntax errors by running: ```sudo nginx -t```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/09173f98-3dc3-423a-b20d-6a9db918aaa8)

Disable the default Nginx host currently on port 80 by running the command: ```sudo unlink /etc/nginx/sites-enabled/default```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/25149ff0-ead1-4cc5-a059-854b40c3aa6c)

Reload Nginx for changes to take effect: ```sudo systemctl reload nginx```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/52aac632-0194-4fc8-8666-7224454057f7)

Create index.html in the new /var/www/projectLEMP directory: ```sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c3a6c7d6-ddcd-4d7d-958d-6af1370e7cc2)

When setup correctly, the following should be the result when the public IP or Public DNS is viewed in a browser:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6d5cc91b-e73f-400f-a863-e58a7afe7352)


> [!NOTE]
> Nginx Server blocks have now been configured
