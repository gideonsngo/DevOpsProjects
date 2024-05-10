# 102 - Creating AWS account, EC2 Instance and establishing SSH connection
Created an AWS Account after which a new EC2 instance of the t2.micro family within the free tier was created with an Ubuntu Server Installed. A new key pair was created and downloaded to the local machine.

The permissions on the private key file (.pem) was then changed using the following command: `sudo chmod 0400 private-key-name.pem`
In this case, my private file was named `ubuntu.pem` and so the command executed was: `sudo chmod 0400 ubuntu.pem`

To establish the SSH connection, Open GitBash and navigate to the folder where the private key file was saved. In my case it was saved to the Downloads folder. I ran the command `cd Downloads`
While in the Downloads folder, the command: `ssh -i private-key-name.pem ubuntu@Public-IP-address` where in my own case, the command was `ssh -i ubuntu.pem ubuntu@54.91.134.193`
