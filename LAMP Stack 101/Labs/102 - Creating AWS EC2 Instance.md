# 102 - Creating AWS account, EC2 Instance and establishing SSH connection
Created an AWS Account after which a new EC2 instance of the t2.micro family within the free tier was created with an Ubuntu Server Installed. A new key pair was created and downloaded to the local machine.
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2274a7dc-e167-4f27-86e7-4a72ce1cc908)

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/71e34d18-b882-4ac6-bb69-391995d7c72f)

The permissions on the private key file (.pem) was then changed using the following command: ```sudo chmod 0400 private-key-name.pem```
In this case, my private file was named `ubuntu.pem` and so the command executed was: ```sudo chmod 0400 ubuntu.pem```

To establish the SSH connection, Open GitBash and navigate to the folder where the private key file was saved. In my case it was saved to the Downloads folder. I ran the command ```cd Downloads```
While in the Downloads folder, the command: ```ssh -i private-key-name.pem ubuntu@Public-IP-address``` where in my own case, the command was ```ssh -i ubuntu.pem ubuntu@54.91.134.193```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8c1414aa-2f1d-43ff-98ba-e5cf3a64f2de)
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4cd0da8e-7bf1-410f-89ea-4f7c6cfb1db6)
