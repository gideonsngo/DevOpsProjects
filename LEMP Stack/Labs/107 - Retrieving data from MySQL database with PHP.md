# 107 - Retrieving data from MySQL database with PHP
It is important to configure access to the database for Nginx to be able to query it and retreive data to be displayed.

Connect to MySQL by running the following command: ```sudo mysql -p```:
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7303ac6e-62fe-4d92-911e-a6411d829129)

Create a database named **example_database** by running the command: ```CREATE DATABASE `example_database`;```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/16c5d932-f2f7-4f25-af54-1ea2ffd09213)  

Create a new user named **example_user** using ```mysql_native_password``` as default authentication method. Run the command: ```CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.2';```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c4949ca9-e7a4-43f9-b6d9-da35b5adca96)  

There's a need to grant user permissions over the ```example_database``` by running the command:  ```GRANT ALL ON example_database.* TO 'example_user'@'%';```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a55e6621-bf2e-40e9-bef8-45935c38f496)  

Exit MySQL by running the command: ```exit```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7a91cdf7-ff03-4fc1-90fe-44a2642617bb)  

Test if the new user has the proper permissions by loggging into MySQL with the user's credentials. Run the command ```mysql -u example_user -p``` which will prompt for password because of the ```-p``` attribute in the command  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/3053662e-6d35-4b54-8700-8c659927aa33)  

Run the Command ```SHOW DATABASES;``` to display all the databases that the user has access to:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/845c92b4-0822-448b-8c57-b6a1fb0b9954)  

Create a test table named **todo_list** by running the statements:  
> CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,
> content VARCHAR(255), PRIMARY KEY(item_id));

![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/422e3dc0-2f92-4522-9367-8d9e8b99bf9f)








