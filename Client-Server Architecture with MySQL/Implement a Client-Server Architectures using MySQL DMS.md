# Implement a Client-Server Architecture using MySQL DMS

This guide will walk you through creating and configuring two Linux-based virtual servers (EC2 instances in AWS) to implement a client-server architecture using MySQL.

### Server Setup
- **Server A name:** `mysql server`
- **Server B name:** `mysql client`

![Server Setup](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/e3215d64-5b2f-4cf1-b9df-1bc2c05ee4ec)

### Step 1: SSH Connect to Your Instances
Use the following command to SSH into your instances:

```bash
ssh -i <key_pair_name> ubuntu@<public_IP>
```

![SSH Connect](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/429fab56-4981-4416-a42d-6aaa7de05a46)

### Step 2: Update the Packages
Run this command to update the package lists on both instances:

```bash
sudo apt-get update
```

![Update Packages](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d5d56b10-d1f8-40aa-a2c5-b3287246f998)

### Step 3: Install MySQL Server
On the `mysql server` instance, install the MySQL server:

```bash 
sudo apt-get install mysql-server
```

![Install MySQL Server](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/dae78a39-a78c-4a5b-8eee-63eeb6d0ab4e)

### Step 4: Configure MySQL Server for Remote Connections
Edit the MySQL configuration file to allow remote connections by changing the `bind-address` from `127.0.0.1` to `0.0.0.0`:

```bash
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

![Edit Configuration](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2cca4a32-9cc2-4b8e-a7d9-7ea9086c88a2)

### Step 5: Configure MySQL Server Security Groups
In AWS, modify the security group for the `mysql server` instance to allow inbound TCP connections on port 3306, but only from the IP address of the `mysql client` instance.

![Security Group](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0a90a188-8a29-4c6a-b42f-4582d0c56c8b)

### Step 6: Create a New MySQL User
1. **Log in to MySQL as the Root User:**
   ```bash
   sudo mysql -u root -p
   ```

   ![Log in to MySQL](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/9e6031e0-1a2b-4a52-9354-635c369e94b3)

2. **Create a New User:**
   Replace `username` and `password` with your desired username and password.
   ```sql
   CREATE USER 'username'@'%' IDENTIFIED BY 'password';
   ```

   ![Create User](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/42c4f8dd-e18d-4d59-b6aa-d1f62ecadfe7)

3. **Grant Privileges to the New User:**
   ```sql
   GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' WITH GRANT OPTION;
   ```

   ![Grant Privileges](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f561d7f4-3107-42e8-b0a1-af9eef87b680)

4. **Flush Privileges:**
   ```sql
   FLUSH PRIVILEGES;
   ```

   ![Flush Privileges](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/523f324a-e608-47dc-8420-c4876b91ec86)

5. **Exit MySQL:**
   ```bash
   EXIT;
   ```

### Step 7: Install MySQL Client
On the `mysql client` instance, install the MySQL client:

```bash
sudo apt-get install mysql-client
```

![Install MySQL Client](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4c356059-37af-436b-a8bc-5106f695a8e8)

### Step 8: Connect to the MySQL Server from the Client
Use the newly created user to connect to the MySQL server from the `mysql client` instance:

```bash
mysql -u username -p -h server_host
```

- `username`: The username you created.
- `-p`: Prompts for the password.
- `-h server_host`: The public IP address of the `mysql server` instance.

You should now be able to connect to the MySQL server from the MySQL client using the new user account.
