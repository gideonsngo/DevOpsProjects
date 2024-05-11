# 107 - Enable PHP on the website
Apache's default DirectoryIndex settings gives a filenamed index.html more precedence over index.php but this can be changed by editing the following: `sudo vim /etc/apache2/mods-enabled/dir.conf`

Implement the following changes: 

    <ifModule mod_dir.c>
    		#Change this:
    		#DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
    		#To this:
    		DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
    </IfModule>

Reload apache for changes to take effect: `sudo systemctl reload apache2`

Create a new file named index.php inside the custom root folder to test: `sudo vim /var/www/projectlamp/index.php`

Add the following PHP code to the blank file opened in VIM editor: `<?php  phpinfo ();`

Open the Public IP or Public DNS of the EC2 Instance to view the content of the new index.php

> [!NOTE]
> PHP has now been enabled on the website.
