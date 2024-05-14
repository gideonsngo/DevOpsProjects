# 106 Testing PHP with Nginx
After successfully setting up the LEMP stack, it is important to test to validate that Nginx handles ```.php``` correctly.

Create a new file called ```info.php``` within the document root in the command: ```nano /var/www/projectLEMP/info.php```   
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/147dcdc5-1aa0-48da-8512-dda6e16fa7dd)  

Paste the code in the new file: ```<?php  phpinfo();>```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6e164718-b131-4183-8666-2e92a4f6ea10)

Open the page through the web browser by opening ```http:\\`server_domain_or_IP`/info.php```:
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/71f51661-7bd1-4148-9e9f-7e740135fafc)

After successfully viewing you can remove the file by running the command: ```sudo rm /var/www/your_domain/info.php```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/92e9e281-afaf-4fb7-92a4-18ceb0fbba0d)


