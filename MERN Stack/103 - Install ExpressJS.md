# 103 - Install ExpressJS
After installing Nodejs and NPM manager it is important to install ExpressJS which is the framework for NodeJS.

To install ExpressJS, use npm by running the command: ```npm install express```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/15436be2-48c5-4c32-a146-d806e27a2905)  

Create a file ```index.js``` with the command: ```touch index.js``` and run the command ```ls``` to confirm the file has been created in the directory:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/1f50914f-084f-4fbb-bb92-fe010861865c)  

Install ```dotenv``` module by running the command: ```npm install dotenv```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d84b2ed5-f277-4c3c-a3bc-2f371fbfd141)  

Open the ```index.js``` file with the command: ```vim index.js```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/287f5d74-fbed-494b-81db-f57f32724977)  

Type the code below into the file and save:  
````
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
        res.header("Access-Control-Allow-Origin", "\*");
        res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
        next();
});

app.use((req, res, next) => {
        res.send('Welcome to Express');
});

app.listen(port, () => {
        console.log(`Server running on port ${port}`)
});

````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/993e5968-6840-4024-b986-9a28e8831c75)  

In the same directory as your index.js file, start the server by running the command: ```node index.js```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c417f69a-b2d9-43fc-bcbf-dbb7dd5e3d1e)  If everything goes well, the message **Server running on port 5000** should display  

Create an inbound rule to open TCP port 5000 by running the command: ```sudo ufw allow 5000/tcp``` or by adding an inbound rule through the Amazon console  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ac80b192-9f40-4921-a101-2f592bd9ebfe)  

Open up your browser and access the servers Public IP or Public DNS and followed by port 5000:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d9fdcb26-f4e0-4a00-99e4-ca5a20cb30a1)  

## Routes
Three tasks will be created for the To-do app which include, creating new tasks, displaying all tasks and deleting complete tasks. This will make use of different enpoints linked to standard HTTP request methods: POST, GET, DELETE.
```routes``` are then created that will define various endpoints that the app will depend on.

Create a folder ```routes``` by running the command: ```mkdir routes```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6310b8a7-76e0-4fad-8978-286e43ee4032)  

Change Directory to the ```routes``` folder and create a file called ```api.js``` with the command: ```touch api.js``` and open the ```api.js``` file by running the command: ```vim api,js```:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7fe34f98-f069-405b-b3a6-86530cf4ef64)  

Write the below code into the file and save:   
````
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/82a36d77-1a5f-4e7b-8463-69807e11a988)














