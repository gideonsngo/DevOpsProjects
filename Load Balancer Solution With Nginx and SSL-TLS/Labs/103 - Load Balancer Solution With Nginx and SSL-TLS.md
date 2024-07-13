# 103 - Load Balancer Solution With Nginx and SSL-TLS

## Part 2: Register a Domain and Configure SSL/TLS

### Step 1: Register a Domain Name
- Register a new domain name using any domain registrar (e.g., GoDaddy, Domain.com, Bluehost), but for this implementation I registered a domain name called `devopscalvary.com.ng` from Web4Africa.

### Step 2: Create & Assign Elastic IP and Update DNS
- To create an `Elastic IP` navigate to `VPC` services on the AWS management console and on the left side of the management console under `Virtual Private Cloud` click `Elastic IP` > `Allocate Elastic IP address`.
- Assign the Elastic IP to your `Nginx LB` EC2 instance.
    - Associate the `Elastic IP` to Nginx EC2-Instance:
    <img width="908" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/15b67f17-eb73-4ae9-986a-32460ddec1ca">

- Update the `A-Record` in your domain registrar to point to the Elastic IP of your `Nginx LB` instance.
 <img width="784" alt="image" src="https://github.com/user-attachments/assets/1b0b4f9a-feb5-4985-b064-2f88a197e7ff">

### Step 3: Configure Nginx for Your Domain
- Update the `nginx.conf` file to recognize your new domain name:
    ```sh
    sudo vi /etc/nginx/nginx.conf
    ```
    Replace `server_name www.domain.com` with `server_name www.<your-domain-name.com>`.

    <img width="427" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/93aa35d1-f7bc-46be-9f2b-8569ffb47553">

### Step 4: Install Certbot and Obtain SSL Certificate
- Ensure `snapd` service is active:
    ```sh
    sudo systemctl status snapd
    ```
    <img width="467" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/eefb5ba0-b40f-48fe-9edc-0fa0dc4b4314">

- Install Certbot:
    ```sh
    sudo snap install --classic certbot
    ```
    <img width="360" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/94ab8c65-ff71-4279-be35-038aba3e90c7">

- Create a symlink for Certbot:
    ```sh
    sudo ln -s /snap/bin/certbot /usr/bin/certbot
    ```

- Obtain and install the SSL certificate:
    ```sh
    sudo certbot --nginx
    ```
    <img width="470" alt="image" src="https://github.com/user-attachments/assets/9278a245-cc9a-4b4f-90fb-c0f277ed3be0">

### Step 5: Test Secure Access
- Verify that you can access your website securely by navigating to `www.<your-domain-name.com>` in your browser.

### Step 6: Set Up Certificate Renewal
- Test renewal in dry-run mode:
    ```sh
    sudo certbot renew --dry-run
    ```
    <img width="464" alt="image" src="https://github.com/user-attachments/assets/96b8d34a-e992-45fe-8bdd-252e433517cf">

- Configure a cron job to renew the certificate periodically:
    ```sh
    crontab -e
    ```
    <img width="350" alt="image" src="https://github.com/user-attachments/assets/8672e364-c575-403a-a17f-ee85ac8c00c5">

    > N.B: Select an editor to modify the file (e.g Input 1-4, where `1` is nano).

- Add the following line to the crontab file:
    ```javascript
    * */12 * * * root /usr/bin/certbot renew > /dev/null 2>&1
    ```
    <img width="413" alt="image" src="https://github.com/user-attachments/assets/e7ce876e-af36-43dc-abc7-8a9f2b8c560d">

- Verify the `Cronjob` configuration was successful
    ```
    crontab -l
    ```
    <img width="404" alt="image" src="https://github.com/user-attachments/assets/b60e5486-d383-4be6-b198-df7caefa2f86">

- Accessing the Tooling Website using the domain `www.devopscalvary.com.ng`.

    <img width="821" alt="image" src="https://github.com/user-attachments/assets/f1c8a600-9140-40cf-bc14-912278216013">  
    <img width="868" alt="image" src="https://github.com/user-attachments/assets/35ad5c77-7c25-4b43-8f4c-6792e7fbf3a3">  
    <img width="865" alt="image" src="https://github.com/user-attachments/assets/a57f3e1f-2c2e-4749-ad90-911961f81d2b">

### Conclusion

Congratulations! You have successfully configured an `Nginx load balancer with SSL/TLS` secured connection and set up automatic certificate renewal. Your web solution is now load-balanced and secured with HTTPS.

---
