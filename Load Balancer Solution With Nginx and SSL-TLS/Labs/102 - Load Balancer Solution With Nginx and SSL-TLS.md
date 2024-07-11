# 102 - Load Balancer Solution With Nginx and SSL-TLS
<img width="575" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6d86c351-2cc0-4586-91e4-4c707220a5ac">

This project will guide you through configuring `Nginx` as a load balancer and securing your website with `SSL/TLS` certificates using `LetEncrypt`. We'll achieve this in two parts:

1. Configuring Nginx as a Load Balancer.
2. Registering a domain name and configuring a secured connection using SSL/TLS certificates.

## Part 1: Configure Nginx as a Load Balancer

### Step 1: Create an EC2 Instance
- **Launch an EC2 instance** on AWS based on Ubuntu 24.04 LTS and name it `Nginx LB`.

- **Security Group Configuration**:
  - Open TCP port 80 for HTTP.
  - Open TCP port 443 for HTTPS.
   <img width="780" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/cbdcc37c-7ea7-446d-8823-ae02ad5863d4">

### Step 2: Update `/etc/hosts` File
- Connect to your EC2 instance via SSH.
    ```sh
    ssh -i keypair.pem ubuntu@server-public-ip
    ```

- Update the `/etc/hosts` file with the names and IP addresses of your web servers:
    ```sh
    sudo vi /etc/hosts
    ```

    - Add the following lines:
        ```
        <web-server-private-ip> Web1
        <web-server-private-ip> Web2
        ```
    <img width="353" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/38679f7e-fbf6-4fe2-9fd3-c60c43e03fb9">

### Step 3: Install and Configure Nginx
- Update and upgrade the system:
    ```
    sudo apt update && sudo apt upgrade -y
    ```

- Install Nginx:
    ```
    sudo apt install nginx -y
    ```

### Step 4: Configure Nginx as a Load Balancer
- Open the Nginx configuration file:
    ```
    sudo vi /etc/nginx/nginx.conf
    ```

- Insert the following configuration into the `http` section:
    ```nginx
    upstream myproject {
        server Web1 weight=5;
        server Web2 weight=5;
    }

    server {
        listen 80;
        server_name www.domain.com;
        location / {
            proxy_pass http://myproject;
        }
    }

    # Comment out this line
    # include /etc/nginx/sites-enabled/*;
    ```
    <img width="476" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f35cd9b0-b7cc-4a09-a90c-d2781ca11028">

- Restart Nginx and ensure the service is running:
    ```sh
    sudo nginx -t
    sudo systemctl restart nginx
    sudo systemctl status nginx
    ```
    <img width="472" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/97904f71-73d0-480a-9b04-cdd4ac401118">
