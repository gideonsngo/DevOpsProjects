# 103 - Install Apache
While connection has been established to the Ubuntu Server, It is important to install the apache web server on the host machine.
Installed Apache Server using ubuntu's package manager **apt** with the following command:
Update a list of packages in the package manager: `sudo apt update`
Run apache package installation: `sudo apt install apache2`

After successful installation, verify that the apache2 service is running on the OS, the following command was run: `sudo systemctl status update`
If the service is running, it should show service <span style="color:green;font-weight:bold;">enabled</span> and <span style="color:green;font-weight:bold;">active (running)</span>

Open inbound traffic through port 80 by adding the rule through the AWS interface or by running the command `sudo ufw allow 80/tcp`

