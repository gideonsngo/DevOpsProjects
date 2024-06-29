# 103 - DevOps Tooling Website Solution
## Step 2: Configure the Database Server
- **SSH into the instance**
  
    ```
    ssh -i keypair.pem ubuntu@public-ip
    ```

- **Install MySQL Server on Ubuntu 24.04**:
   - Update and install MySQL server:
     ```sh
     sudo apt update && sudo apt upgrade -y
     sudo apt install mysql-server -y
     ```

- **Create a Database and User**:
   - Log in to MySQL and create a database and user:
     ```sh
     sudo mysql
     ```
     ```sql
     CREATE DATABASE tooling;
     CREATE USER 'myuser'@'172.31.32.0/20' IDENTIFIED BY 'mypass1';
     GRANT ALL PRIVILEGES ON tooling.* TO 'myuser'@'<subnet-cidr>';
     FLUSH PRIVILEGES;
     EXIT;
     ```
     <img width="422" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/67da117c-e2de-4810-a2f7-e8b164f85fb1">

    - Edit the MySQL configuration file to bind it to all IP addresses, (0.0.0.0) Open the MySQL configuration file, which is located at `/etc/mysql/mysql.conf.d/mysqld.cnf`
        ```sh
        sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
        ```
        <img width="392" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d88d77a0-c99c-475f-811a-676a0c5f6645">

    - Start and enable MySQL service:
        ```sh
        sudo systemctl restart mysql
        sudo systemctl enable mysql
        sudo systemctl status mysql
        ```
        <img width="440" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/24d0516f-5f3c-4cc4-9011-4f756e99d05c">

## Step 3: Prepare the Web Servers
- **SSH into the instance**
  
    ```
    ssh -i keypair.pem ec2-user@public-ip
    ```

- **Configure NFS Client on All Web Servers**:
   - Launch (3) new EC2 instances with RHEL 8 OS.
   - Install NFS client:
     ```sh
     sudo yum update -y
     sudo yum install nfs-utils nfs4-acl-tools -y
     ```
   - Mount NFS Web shares `/var/www`:
     ```sh
     sudo mkdir -p /var/www
     sudo mount -t nfs -o rw,nosuid 172.31.16.182:/mnt/apps /var/www
     ```
   - Add the following lines to `/etc/fstab` to ensure mounts persist after reboot:
     ```sh
     sudo vi /etc/fstab
     ```
     Add:
     ```
     172.31.16.182:/mnt/apps /var/www nfs defaults 0 0
     ```
     ![Mount Web Point](./images/mount-nfs-share-webserver.PNG)

- **Install Apache and PHP**:
   - Install Remi's repository, Apache, and PHP:
     ```sh
     sudo yum install httpd -y
     sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
     sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
     sudo dnf module reset php -y
     sudo dnf module enable php:remi-7.4 -y
     sudo dnf install php php-opcache php-gd php-curl php-mysqlnd -y
     sudo systemctl start php-fpm
     sudo systemctl enable php-fpm
     sudo setsebool -P httpd_execmem 1
     ```
   - Start and enable Apache:
     ```sh
     sudo systemctl start httpd
     sudo systemctl enable httpd
     ```
   - Mount NFS Logs shares `/var/log/httpd`:
     ```sh
     sudo mkdir -p /var/log/httpd
     sudo mount -t nfs -o rw,nosuid <NFS-Server-private-ip>:/mnt/logs /var/log/httpd
     ```
   - Add the following lines to `/etc/fstab` to ensure mounts persist after reboot:
     ```sh
     sudo vi /etc/fstab
     ```
     Add:
     ```
     <NFS-Server-private-ip>:/mnt/logs /var/log/httpd nfs defaults 0 0
     ```
     ![Mount Logs Point](./images/1.PNG)

- **Deploy Tooling Application**:
   - Fork the tooling source code from `Darey.io GitHub` account to your GitHub account.
    ![Forked Repository](./images/forked-repo.PNG)
   
   - Then `clone` the source code repository to the Web server.
        ```sh
        sudo yum install git -y
        git clone https://github.com/alagbaski/tooling.git
        ```

   - Deploy the code to the web server and ensure the `html` folder from the repository is deployed to `/var/www/html`.
        ```sh
        cd tooling/html
        sudo mv * /var/www/html/
        ```

   - Verify that Apache files and directories are available on the web server in `/var/www` and also on the NFS server `/mnt/apps`.

   - If you see the same files, it means NFS is mounted correctly. Create a new file `touch test.txt` from one server and check if it is accessible from other web servers.
    ![Create Test.txt](./images/testing-txt-file-1.PNG)
    - Verify the NFS Mounted Correctly
    ![Verify Test.txt](./images/testing-txt-file-2.PNG)

- **Configure Database Connection for the Website**:
   - Update the website's configuration to connect to the database (in `/var/www/html/functions.php` file).
        ```sh
        sudo vi /var/www/html/functions.php
        ```
      - Add the `db-private-ip` as `host`, also include `username`, `password` and `db-name` into the file.

   - Apply the `tooling-db.sql` script to your database:
     - Fist make sure MySQL client is installed on the Web servers.
      ```sh
      sudo yum install mysql -y
      ```
      ```sh
      cd tooling
      mysql -h <database-private-ip> -u <db-username> -p <db-name> < tooling-db.sql
      ```
   - Database Security Group:
    ![DB SG](./images/DB-sg.PNG)

- **Create Admin User in MySQL**:
   - First, we access the Database remotely from the web server with `webaccess` user credentials.
     ```sh
     sudo mysql -u webaccess -p -h <db-server-private-ip>
     ```
   - Create a new admin user with username `myuser` and password `password`:
     ```sql
     USE tooling;
     INSERT INTO users (id, username, password, email, user_type, status) VALUES (2, 'myuser', '5f4dcc3b5aa765d61d8327deb882cf99', 'user@mail.com', 'admin', '1');
     EXIT
     ```

- **Open Port 80 & Required Ports**:
   - Ensure TCP port 80 is open on the web server to allow HTTP traffic.
    ![Web SG](./images/web-sg.PNG)

- **If Encountering 403 Errors & Apache Fails to Restart**:
  - Then you will need to allow apache to use NFS mounted directories with SElinux enabled
   
     ```sh
     sudo setsebool -P httpd_use_nfs 1
     ```
  - Allow apache to make outside connection requests
     ```sh
     sudo setsebool -P httpd_can_network_connect=1
     ```
  - Then restart the Apache service again
     ```sh
     sudo systemctl restart httpd
     ```
     ![Restartin Apache](./images/setsebool-httpd-nfs.PNG)

  - To disbale selinux and make the change permanent, open selinux file and set selinux to disable.
     ```sh
     sudo setenforce 0
     
     sudo vi /etc/sysconfig/selinux

     SELINUX=disabled

     sudo systemctl restart httpd
     ```

- **Access the Website**:
   - Open a browser and navigate to `http://<web-server-public-ip>/index.php`.
    ![Tooling Web Page](./images/accessing-on-the%20website-index-php.PNG)

   - Log in with the `myuser` user.
    ![Login Page](./images/accessing-on-the%20website-index-php-2.PNG)

### Conclusion
Congratulations! You have successfully implemented a web solution for a DevOps team using a LAMP stack with remote database and NFS servers.
