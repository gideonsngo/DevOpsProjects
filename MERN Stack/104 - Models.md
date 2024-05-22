# 104 - Models
Models are at the heart of JavaScript applications which makes it interractive. Models are also used to define the database schema which essentially defines the fields stored in each Mongodb document.
To create a schema and a model, install ```mongoose``` which is a Nodejs package that makes working with mongodb easier.

Change working directory to Todo folder and install Mongoose: ```npm install mongoose```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7ae1c209-ce1f-4cdc-a2e8-4fc892ebd3e0)  

Create a new folder using the ``mkdir models``` command and change the working directory to the newly created ```models``` folder:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/da2a3fed-21a3-4134-887d-c4ae076e875b)  

Inside the ```models``` folder create a file ```todo.js``` by running the command ```touch todo.js```  and open the file in the editor by running the command ```vim todo.js```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/151c53d3-fd3c-4e05-881d-198d4e47c6db)

Write the code in the file:  
````
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
        action: {
                type: String,
                required: [true, 'The todo text field is required']
        }
})

//create model for todo
        const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/b3a8fba8-abe8-42bf-a675-8acb5f959856)

In routes directory, open ```api.js``` in the text editor and replace the code with the following:  
````
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action) {
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});
router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data = > res.json(data))
.catch(next)
})

module.exports = router;
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/03276478-3d8d-4ae1-9777-bc92e52e68ed)  





