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
    ![Arbitrary Names](./images/arbitrary-names-web-servers.PNG)

- **Update Load Balancer Configuration with These Names**
    ```apache
    BalancerMember http://Web1:80 loadfactor=5 timeout=1
    BalancerMember http://Web2:80 loadfactor=5 timeout=1
    ```

- **Test the Configuration Locally**
    ```sh
    curl http://Web1
    curl http://Web2
    ```
    - Running `curl` command on Web Server 1:
     ![Curl WebServer1](./images/curl-web1.PNG)

    - Running `curl` command on Web Server 2:
     ![Curl WebServer2](./images/curl-web2.PNG)
