# 103 - Install MongoDB
## Step 1: 
MongoDB will be used to store data in JSON-like documents which will be used in our example application for adding book records which contains book name, isbn number, author and number of pages.

Install MongoDB  
```bash
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/69d4cdde-cc83-431e-bef2-4c703bf8c5d0)  

Run the following code to update dependencies   
```bash
sudo apt-get update
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/e83f87ba-3ea8-41dc-b073-b4e327f5f103)  

Install the MongoDB by running the command  
```bash
sudo apt-get install -y mongodb-org
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/db1de398-afc0-4672-9354-9aad7812159a)  

Start the Server by running the following code:  
```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/bd896926-35d2-43c2-8790-6e738ba84644)  

Verify that the service is up and running  
```bash
sudo systemctl status mongod
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4487f601-90e1-4766-8d45-58f24302a3b3)  

Verify that `npm` has been installed  
```bash
npm -v
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4caf8b63-30f2-479a-b3d1-13c4830f6a40)

If its not installed, install `npm`  
```bash
sudo apt install -y npm
```

Install `body-parser` package which helps to process JSON files passed in requests to the server.  
```bash
sudo npm install body-parser
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f88ed459-6708-4be4-a105-ab35d052d299)  

Create a folder named `Books`  
```bash
mkdir Books && cd Books
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/1e0aad93-ffc4-4df1-8236-b13bab9640d5)  

In the Books directory, Initialize npm project  
```bash
npm init
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/57ca77f6-da18-41b5-90f8-c5485467fbc3)  

Add a file to the directory called `server.js`  
```bash
vim server.js
```

Write the following code into the `server.js` file  
```bash
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3000);
app.listen(app.get('port'), function(){
    console.log('Server up: http://localhost:' + app.get('port'));
});
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2501a7ca-935d-49df-be08-b716bc359291)










