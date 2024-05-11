# 106 - Creating a Virtual Host
After installing LAMP stack, It is important to create a virtual host. Virtual hosting allows for one server to be used in hosting multiple domain names.

Create a directory called **projectlamp** which was used to point as the directory for the virtual host to be created using the **mkdir** command: `sudo mkdir /var/www/projectlamp`

Ownership of the newly created directory was changed to the current system user using the environment variable $USER: `sudo chown -R $USER:$USER /var/www/projectlamp`

A new configuration file is then created and edited using the VIM editor in the sites-available folder of Apache: `sudo vi /etc/apache2/sites-available/projectlamp.conf`

In the newly created file opened in the VIM editor, add the following code to create the virtual host:

    <VirtualHost *:80>
    		ServerName projectlamp
    		ServerAlias www.projectlamp
    		ServerAdmin webmaster@localhost
    		DocumentRoot /var/www/projectlamp
    		ErrorLog ${APACHE_LOG_DIR}/error.log
    		CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost> 

Enable the new virtual host by running the command: `sudo a2ensite projectlamp`

Disable the default apache website by running the command: `sudo a2dissite 000-default`

Create index.html in the new /var/www/projectlamp directory: `sudo vi /var/www/projectlamp/index.html`

> [!NOTE]
> vi and vim are used interchangeably to open the vim editor.  
> Virtual Host has now been created.


