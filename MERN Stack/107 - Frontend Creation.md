# 107 - Frontend Creation
Afte completing the functionality of the backend and API, it is time to create the UI frontend of the To-do app. Use ```create-react-app``` command to scaffold our app:  
Run the command ```npx create-react-app client``` in the Todo directory:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/cf74e640-afa9-4e53-be2b-8024be0db4e3)  

This will run the installer and create a new folder in the directory called ```client``` where the react code will be added. 
**NB** If the installation gets stuck it could be as a result of the resources on the t2 micro. Upgrade to t2 small and continue installation.  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8f95fb72-97d6-40d5-9ce8-4ca0cccc0639)

## Running a React App
Some dependencies need to be installe before testing the react app:  
1. Install **concurrently** which runs more than one command simultaneously from the same terminal window. Run the command: ```npm install concurrently --save-dev```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c833f842-1221-42ab-a278-8e103b5b5952)

2. Install **nodemon** which runs and monitors the servere for changes to the server code, restarting it automatically and loading new changes: ```npm install nodemon --save-dev```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/12ae4e53-c8d8-475c-9985-530995765f5a)

3. In the ```Todo``` folder, open the ```package.json``` file, change the highlighted part of the below screenshot and replace with the code below.:
````
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/13d91684-374c-4b86-a7a0-d92c257804e9)  

## Configure Proxy in ```package.json```
1. Change directory to ```client```: ```cd client```

2. Open the ```package.json```: ```vi package.json```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/9323483d-f953-4eb6-8804-9feb56c884b7)

4. Add the key value pair in the ```package.json``` file ```"proxy": "http://localhost:5000"``` to make it possible to access the application directly from the browser by simply calling the server url like ```http://localhost:5000``` rather than always including the entire path like ```http://localhost:5000/api/todos```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ec428261-7463-4f54-9f1e-c7bc1c811c43)

5. While in the ```Todo``` directory, run: ```npm run dev```
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/4d3201e4-2645-4637-b2e1-e5babbeb88d1)

Your app should open and start running on ```localhost:3000```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/6d67340f-c066-4f78-a562-5845dd5f5d98)  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8199a765-2a52-4d56-a384-1f026c04bd51)  

7. Allow TCP port 3000 on the EC2 instance
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/bba8e1de-1c0e-4dff-ad4a-409f856199f2)

