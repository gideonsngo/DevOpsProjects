# 103 - Web Solution With Wordpress
## Step 2: Prepare the Database Server
Launch a Second EC2 Instance that will have a role `database server`. Repeat the same steps as the configuration steps for the `web server`, but instead of `apps-lv` create `db-lv` and mount it to `/db` instead of `/var/www/html`.

### Step 3: Install WordPress on your Web Server
Update the repository:
`sudo yum -y update`

Install wget, Apache and its dependiencies:  
     ```sh
     sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
Start Apache
```
     sudo systemctl enable httpd
     sudo systemctl start httpd
     ```
To install PHP and its dependencies:

     ```sh
     sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
     sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
     sudo yum module list php
     sudo yum module reset php
     sudo yum module enable php:remi-7.4
     sudo yum install php php-opcache php-gd php-curl php-mysqlnd
     sudo systemctl start php-fpm
     sudo systemctl enable php-fpm
     sudo setsebool -P httpd_execmem 1
     ```
Restart Apache:

`sudo systemctl restart httpd`

Download wordpress and copy wordpress to /var/www/html/wordpres

     ```sh
     mkdir wordpress && cd wordpress
     sudo wget http://wordpress.org/latest.tar.gz
     sudo tar xzvf latest.tar.gz
     sudo rm -rf latest.tar.gz
     sudo cp -R wordpress/wp-config-sample.php wordpress/wp-config.php
     sudo cp -R wordpress /var/www/html/
     ```
     
Configure SELinux Policies:

     ```sh
     sudo chown -R apache:apache /var/www/html/wordpress
     sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
     sudo setsebool -P httpd_can_network_connect=1
     ```

### Step 4: Install MySQL on your DB Server EC2
Install MySQL:

     ```sh
     sudo yum update
     sudo yum install mysql-server
     ```
     
Verify that the service is up and running by using `sudo systemctl status mysqld`, if it is not running, restart the service and enable it so it willbe running even after reboot:

     ```sh
     sudo systemctl enable mysqld
     sudo systemctl restart mysqld
     ```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/020a3470-83d4-4f46-9292-2017fe456e63)

### Step 5: Configure DB to work with WordPress
```sudo mysql```

     ```sql
     CREATE DATABASE wordpress;
     CREATE USER `myuser`@`<Web-Server-private-ip-Adress>` IDENTIFIED BY 'mypass';
     GRANT ALL ON wordpress.* TO 'myuser'@'<web-server-private-ip-address>';
     FLUSH PRIVILEGES;
     SHOW DATABASES;
     exit
     ```

### Step 6: Configure WordPress to Connect to Remote Database
Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server **ONLY** from your Web Server's IP address, so in the Inbound Rule configuration soecify source as /32

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/605965e5-a726-46c7-b717-7fe41fcb2fc7)

Install MySQL Client and test that you can connect from your Web Server to your DB server by using `mysql-client`

     ```sh
     sudo yum install mysql
     sudo mysql -u username -p -h DB-server-private-ip
     ```
     
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/24751566-f980-4ed0-be1d-b57b08022bdf)

Verify if you can successfully execute `SHOW DATABASES;` command and see a list of existing databases.

Change permissions and configuration so Apache could use Wordpress 

Update `wp-config.php` with database details.

Set appropriate permissions and configurations for Apache to use WordPress. 

    ```sh
    sudo chown -R apache:apache /var/www/html/wordpress
    ```
    
Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstations IP)

Try to access from your browser the link to your WordPress `http://<Web-server-public-ip-address>/wordpress/`.
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c8a6f39d-586d-4077-a068-dcb7de14aed6)
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d66498dd-40ec-4b43-8f41-03ef155e659c)
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a7e6d030-e87c-432b-9178-52902db46961)

**CONGRATULATIONS**
You have learned how to confidure Linux storage susbystem and have also deployed a full-scale Web Solution using WordPress CMS and MySQL RDBMS!
    
  
