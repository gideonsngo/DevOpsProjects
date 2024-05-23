# 105 - MongoDB Database
A database is required for storing our data and we will make use of mLab which provides MongoDB database as a service. Sign up to mLab and deloy a shared cluster free account, choose AWS cloud provider and a region near you.  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7cd51e5f-15e8-40e9-a6f4-86216f1e0094)  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6d0b4d21-b427-4b09-a8f1-d166df0da14d)  

Allow access to the Database from anywhere and change the time of deleting the entry from 6 Hours to 1 week:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8e36ae8c-fe08-431c-92f5-881065f2ecee)
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f0326ba7-925d-4ff3-b439-ac1517b8c307)  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4c4d8213-d4e3-4775-87ac-dc3e8fdda194)

Create a database user ```ubuntu``` and password ```PassWord.1```  

Create a MongoDB database and collection inside mLab:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/dc433604-6c52-4f78-a99f-d68d6ee3706b)  

Create a file in your ```Todo``` directory and name it ```.env``` by running the code ```touch.env``` and ```vi .env```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0ff13db1-de64-46a5-96c8-6bc605864714)  

Add connection string to access the database into the newly created ```.env``` file while adding your <username>, <password>, <network-address> and <database> accordingly: 
```DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/f8ff175a-f9d9-4de2-a209-fb1d5e444ba8)  

Update the ```index.js``` to reflect the use of ```.env``` so that Node.js can connect to the database by editing the content of the file to the following code:  
````
const express = require('express');
const bodyParser = require('body-parser');
const mongoose= require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we override it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
        res.header("Access-Control_Allow-Origin", "\*");
        res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
        console.log(err);
        next();
});

app.listen(port, () => {
        console.log(`Server running on port ${port}`)
});
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a527e4e4-6090-49e2-ae8a-97a8eeba91bd)  

Using environmental variables store information is considered a secure best practice instead of entering that configuration information directly into the ```index.js``` file. Start the server using ```node index.js```:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0ecf3193-0341-436f-9752-51aa5c826651)  












