# 101 - Load Balancer Solution With Apache
## Prerequisites
Make sure that you have the following servers installed and configured within the previous project:
- Two RHEL8 Web Servers
- One MySQL DB Server (based on Ubuntu 20.04)
- One RHEL8 NFS server
<img width="584" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/23438a3a-58e9-4096-9cf1-44d110a7c99a">

## Launch & Configure Apache as a Load Balancer

### Create an Ubuntu Server EC2 Instance On AWS
- SSH into the instance
  
    ```
    ssh -i keypair.pem ubuntu@public-ip
    ```

- Create an EC2 instance and name it `Project-8-apache-lb`.
  <img width="742" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/89183700-c94f-40ad-bb35-6b9a2db31551">
  
- Open TCP port 80 on the `Project-8-apache-lb` server.
  <img width="620" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/5017e601-aa30-4781-9bd5-f62d7def5ef6">

### Install and Configure Apache

- **Update and Install Apache**
    ```sh
    sudo apt update && sudo apt upgrade -y
    sudo apt install apache2 -y
    sudo apt-get install libxml2-dev
    ```

- **Enable Required Apache Modules**
    ```sh
    sudo a2enmod rewrite
    sudo a2enmod proxy
    sudo a2enmod proxy_balancer
    sudo a2enmod proxy_http
    sudo a2enmod headers
    sudo a2enmod lbmethod_byrequests
    ```

- **Restart Apache Service**
    ```sh
    sudo systemctl restart apache2
    ```

- **Verify Apache Service**
    ```sh
    sudo systemctl status apache2
    ```

### Configure Load Balancing

- **Edit the Default Apache Configuration**
    ```sh
    sudo vi /etc/apache2/sites-available/000-default.conf
    ```

- **Add the Following Configuration to the `<VirtualHost *:80>` Section**
    ```apache
    <Proxy "balancer://mycluster">
        BalancerMember http://<webserver1-private-ip>:80 loadfactor=5 timeout=1
        BalancerMember http://<webserver2-private-ip>:80 loadfactor=5 timeout=1
        ProxySet lbmethod=byrequests
    </Proxy>

    ProxyPreserveHost On
    ProxyPass / balancer://mycluster/
    ProxyPassReverse / balancer://mycluster/
    ```
    <img width="447" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ffa54085-815e-4772-8341-aaf1e734afe3">

    **Note:** `bytraffic` balancing method will distribute incoming load between your Web Servers according to current traffic load. We can control in which proportion the traffic must be distributed by `loadfactor` parameter.
  You can also study and try other methods, like: `bybusyness`, `byrequests`, `heartbeat`

- **Restart Apache Server**
    ```sh
    sudo systemctl restart apache2
    ```

- **If Apache Does Not Run Successfully Follow the below step**:
   - Check for any syntax errors in the Apache configuration files
        ```sh
        sudo apachectl configtest
        ```

### Verify Configuration

- Access the Load Balancer using its public IP address or public DNS name from a browser:
    ```
    http://<Load-Balancer-Public-IP-or-Public-DNS-Name>/index.php
    ```
  - Accessing the `Web Servers` using the `LB Server` Public-IP address on the broswer:
   <img width="730" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0f54e2a3-9129-48f8-a752-c2595bd28a34">

  - Login with `myuser` credentials:
   <img width="779" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/64a01694-6152-4e5d-91d0-12ebe998183e">

### Monitoring Access Logs

- Open two SSH consoles for both web servers and run the following command to monitor access logs:
    ```sh
    sudo tail -f /var/log/httpd/access_log
    ```
    - Web Server 1 `access_log`:
     <img width="452" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0562aa49-7aed-4d13-bf42-6ffde00d49ac">

    - Web Server 2 `access_log`:
     <img width="456" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/164bc52c-b4a6-4556-aed0-27a7be8c398b"> 

**Note:** If in the previous project you mounted `/var/log/httpd/` from your Web Server to the NFS server, unmount them and ensure each Web Server has its own log directory.
