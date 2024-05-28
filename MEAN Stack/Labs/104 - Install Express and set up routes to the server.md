# 104 - Install Express and set up routes to the server
Express will be installed which is a web application framework that provides features for web and mobile applications. It will handle the passing of book information to and from our MongoDB database.  `Mongoose` will also be installed to establish a schema model for the application database to store our book register.  

Install `Express` and `Mongoose`  
```bash
sudo npm install express mongoose
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/776df4ca-cba1-4423-8bba-b89f98518a06)  

In `Books` folder create a folder named `apps`  
```bash
mkdir apps && cd apps
```
Create a file named `route.js` and open with the vim editor. Write the following code in the file  
```bash
var Book = require('./models/book');
module.exports = function(app){
    app.get('/book', function(req,res){
        Book.find({}, function(err, result){
            if(err) throw err;
            res.json(result);
        });
    });
    app.post('/book', function(req, res){
        var book = new Book({
            name:req.body.name,
            isbn:req.body.isbn,
            author:req.body.author,
            pages:req.body.pages
        });
        book.save(function(err, result){
            if(err)throw err;
            res.json({
                message:"Successfully added book",
                book:result
            });
        });
    });
    app.delete("/book/:isbn", function(req, res){
        Book.findOneAndRemove(req.query, function(err,result){
            if(err) throw err;
            res.json({
                message: "Successfully deleted the book",
                book: result
            });
        });
    });
    var path = require('path');
    app.get('*', function(req,res){
        res.sendfile(path.json(__dirname + '/public', 'index.html'));
    });
};
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/fde696dd-9ac5-463e-ae39-812ff53cc4bc)  

In the `apps` folder create a folder named `models`  
```bash
mkdir models && cd models/
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/b0ede362-7e83-4f3c-879e-3eead76638f9)  

Create a file named `book.js` and open in the text editor ```vim book.js```. Paste the following code  
```bash
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema({
    name: String,
    isbn: {type: String, index: true},
    author: String,
    pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c79bc640-aefe-45d3-988e-7e6f94d95231)  




