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
We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case - NFS Server and MySQL database. You already know that one DB can be accessed for reads and writes by multiple clients. For storing shared files that our Web Servers will use - we will utilize NFS and mount previously created Logical Volume lv-apps to the folder where Apache stores files to be served to the users (/var/www).

This approach will make our Web Servers stateless, which means we will be able to add new ones or remove them whenever we need, and the integrity of the data (in the database and on NFS) will be preserved.

During the next steps we will do following:

- Configure NFS client (this step must be done on all three servers)
- Deploy a Tooling application to our Web Servers into a shared NFS folder
- Configure the Web Servers to work with a single MySQL database

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
     sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
     ```
   - Add the following lines to `/etc/fstab` to ensure mounts persist after reboot:
     ```sh
     sudo vi /etc/fstab
     ```
     Add:
     ```
     <NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0
     ```
     <img width="445" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a9e4651f-635e-44f2-a399-c395d3db9586">

- **Install Apache and PHP**:
   - Install Remi's repository, Apache, and PHP:
     ```sh
     sudo yum install httpd -y
     sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y
     sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm -y
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
     <img width="351" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0aef6293-a919-44d4-9a98-c2021a1a0e44">
     
- **Deploy Tooling Application**:
   - Fork the tooling source code from `Steghub GitHub` account to your GitHub account.
    ![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/cc4796bb-6e7f-4ca0-abc6-0dd33145300b)
   
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
    <img width="340" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/64103b6f-d412-48e8-8ace-d02b67412bba">

    - Verify the NFS Mounted Correctly
    <img width="394" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f6ba8a19-c5d7-4596-9c82-d3d1cef54ad0">

- **Configure Database Connection for the Website**:
   - Update the website's configuration to connect to the database (in `/var/www/html/functions.php` file).
        ```sh
        sudo vi /var/www/html/functions.php
        ```
      - Add the `db-private-ip` as `host`, also include `username`, `password` and `db_name` into the file.

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
  <img width="680" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0e989cb1-07c5-4844-9f7c-cef1281e5eec">
  
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
     <img width="444" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ad811f6a-6ca5-4eb3-8d8b-647cb6d70a3c">

- **Open Port 80 & Required Ports**:
   - Ensure TCP port 80 is open on the web server to allow HTTP traffic.

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

  - To disbale selinux and make the change permanent, open selinux file and set selinux to disable.
     ```sh
     sudo setenforce 0
     
     sudo vi /etc/sysconfig/selinux

     SELINUX=disabled

     sudo systemctl restart httpd
     ```
     ![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f322112d-b0ca-4a0b-8e94-910a9ecf4f84)


- **Access the Website**:
   - Open a browser and navigate to `http://<web-server-public-ip>/index.php`.
     ![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/3081cf6f-cea8-4bda-9816-d2e99de4407d)

   - Log in with the `myuser` user.
    

### Conclusion
Congratulations! You have successfully implemented a web solution for a DevOps team using a LAMP stack with remote database and NFS servers.
