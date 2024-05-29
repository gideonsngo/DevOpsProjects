# 105 - Access the routes with AngularJS
AngularJS provides a web framework for creating dynamic views in our web applications. We use AngularJS to connect our web page with Express and perform actions on our book register.  
Change directory to `Books`  
```bash
cd ../..
```
Create a folder named `public`  
```bash
mkdir public && cd public/
```

Create a file named `scrit.js` 
```bash
vi script.js
```
Write the following code into the `script.js` file
```bash
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
    // Fetch books from the server
    $http({
        method: 'GET',
        url: '/book'
    }).then(function successCallback(response) {
        $scope.books = response.data;
    }, function errorCallback(response) {
        console.log('Error:' + response);
    });

    // Delete a book
    $scope.del_book = function(book) {
        $http({
            method: 'DELETE',
            url: '/book/' + book.isbn // Use book.isbn directly in the URL
        }).then(function successCallback(response) {
            console.log(response);
            // Optionally, refresh the book list or remove the deleted book from $scope.books
        }, function errorCallback(response) {
            console.log('Error:' + response);
        });
    };

    // Add a new book
    $scope.add_book = function() {
        var body = {
            name: $scope.Name,
            isbn: $scope.Isbn,
            author: $scope.Author,
            pages: $scope.Pages
        };

        $http({
            method: 'POST', // Changed to POST for adding a book
            url: '/book',
            data: body,
            headers: {
                'Content-Type': 'application/json' // Set the content type to JSON
            }
        }).then(function successCallback(response) {
            console.log(response);
            // Optionally, refresh the book list or add the new book to $scope.books
        }, function errorCallback(response) {
            console.log('Error:' + response);
        });
    };
});
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0f927048-420f-41b1-a067-789d85deccd3)

In the `public` folder create an `index.html` file  
```bash
vi index.html
```
Write the follosing code below into the `index.html` file 
```bash
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
<head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
</head>    
    <body>
        <div>
            <table>
                <tr>
                    <td>Name:</td>
                    <td><input type="text" ng-model="Name"></td>
                </tr>
                <tr>
                    <td>Isbn:</td>
                    <td><input type="text" ng-model="Isbn"></td>
                </tr>
                <tr>
                    <td>Author:</td>
                    <td><input type="text" ng-model="Author"></td>
                </tr>
                <tr>
                    <td>Pages:</td>
                    <td><input type="number" ng-model="Pages"></td>
                </tr>
            </table>
            <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
            <table>
                <tr>
                    <th>Name</th>
                    <th>Isbn</th>
                    <th>Author</th>
                    <th>Pages</th>
                </tr>

                <tr ng-repeat="book in books">
                    <td>{{book.name}}</td>
                    <td>{{book.isbn}}</td>
                    <td>{{book.author}}</td>
                    <td>{{book.pages}}</td>

                    <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
                </tr>
            </table>
        </div>
    </body>
</html>
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d994fc0a-c184-4216-a61d-718d4cd5fe0f)

Change the directory back to `Books` 
```bash
cd ..
```

Start the server by running the command
```bash
node server.js
```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/9967d792-0e97-429b-b5c5-7c9c6c613b6c)

Connect to the Server using the ```http://Public_IP:3000``` 
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c06878cc-d87c-4832-a2f3-ebfc251f1cee)

