# 103 - Load Balancer Solution With Apache

### Optional: Configure Local DNS Name Resolution

- **Edit the Hosts File on the Load Balancer Server**
    ```sh
    sudo vi /etc/hosts
    ```

- **Add Local IP Addresses and Arbitrary Names for Both Web Servers**
    ```sh
    <webserver1-private-ip> Web1
    <webserver2-private-ip> Web2
    ```
    <img width="455" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/dab0a1f3-4907-4ea1-914c-8a85bd15363b">

- **Update Load Balancer Configuration with These Names**
    ```apache
    BalancerMember http://Web1:80 loadfactor=5 timeout=1
    BalancerMember http://Web2:80 loadfactor=5 timeout=1
    ```
    <img width="404" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4f413e4b-06a7-494b-a6fd-aa9d1690aa1b">

- **Test the Configuration Locally**
    ```sh
    curl http://Web1
    curl http://Web2
    ```
    - Running `curl` command on Web Server 1:
     <img width="451" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/73915a54-f681-4454-997c-8efae6f2c578">

    - Running `curl` command on Web Server 2:
     <img width="442" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/5c7f23ed-e739-4783-979f-79bb44c1f921">

### Current Architecture
<img width="715" alt="image-23" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a1c84bdf-b8f0-469b-a1ea-d94a23457d22">

**Congratulations!**
You have just implemented a Load balancing Web Solution for your DevOps team.

     
