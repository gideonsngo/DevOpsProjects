# 104 - Install MySQL
After installing the apache web server, It is important to install the MySQL database.
Installed MySQL Server using ubuntu's package manager **apt** with the following command:
Run MySQL package installation: `sudo apt install mysql-server`

After successful installation, verify that the mysql service is running on the OS, the following command was run: `sudo systemctl status mysql`
If the service is running, it should show service enabled and active (running) in green.

Log unto mysql using the command: `sudo mysql` to connect as the admin database **root** user

Change the root user password for mysql with the following command: `ALTERUSER'root'@'localhost IDENTIFIED WITH mysql_native_password BY'PassWord.1';` where `PassWord.1` was the password set for the account.

Run a security script that removes insecure default settings allowing for database root password to access the database: `sudo mysql_secure_installation`

After running script, use the following command to login to mysql which now requires the password used after changing the root user password: `sudo mysql -p`


> [!NOTE]
> MySQL has now been installed and configured.

