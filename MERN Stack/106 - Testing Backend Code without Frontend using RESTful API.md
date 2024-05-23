# 106 - Testing Backend Code without Frontend using RESTful API
The backend part of the ```To-Do``` application has been written and a database has been configured but a frontend UI is yet to be implemented. ReactJS will be used to acheive the frontend. However, theres a need to test our code using RESTful API, in this case, we use an API development client to test our code.

Download and install postman:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/fbfff89a-6654-4681-9bb3-c270963207a1)

Next is to test the API endpoints to be sure they are working properly:

Create a POST request to the API: ```http://<PublicIP-or-PublicDNS>:5000/api/todos```. It is also important to enter the body of the request with the content of the new task to be posted into the database:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/7078a6fa-5b13-4e6b-b81f-50de6da2b190)  

Create a GET request to the API: ```http://<PublicIP-or-PublicDNS>:5000/api/todos``` which will retreive all existing records from the database.
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/88dd281e-c246-421a-bfc9-01a666b459c7)  

Create a DELETE request to the API: ```http://<PublicIP-or-PublicDNS>:5000/api/todos/<id-of-task>``` which will delete an existing record from the database using the ```id``` as an identifier of the record.  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/d4747200-b1b2-4094-9a8c-350d1a25fe84)



