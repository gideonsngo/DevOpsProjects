# 108 - Creating your React Components
For our Todo app, we will make use of components which are reusable and also makes code modular. Two stateful components and one stateless compnent will be used.
Navigate to ```cd Todo/client/src```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8c3fc1d0-481d-4029-9b2e-0cc87305c433)

Create a folder called ```components``` in the ```src``` folder using the command ```mkdir components```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/a8453ca5-ecdb-40be-b543-8391354bb4e3)

Move into the ```components``` folder using ```cd components``` and Create three files ```Input.js, ListTodo.js and todo.js``` using the command ```touch Input.js, ListTodo.js and todo.js```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/11ceb69e-6d26-42d1-94e9-7d464fcbf0c6)

Open ```Input.js``` using a text editor of your choice ```vim Input.js``` and paste the following code  

To make use of Axios, which is a promise based HTTP client, it is important to install it in the ```clients``` folder. Navigate to the ```clients``` folder and run the command ```npm install axios```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/2c6c48f4-450a-4fe6-8f07-6aa93d005333)  

Navigate to ```cd src/components``` and open the ```ListTodo.js``` using the text editor of your choice and paste the following code:  
````
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
        state = {
                action: ""
        }

        addTodo = () => {
                const task = {action: this.state.action}

                if(task.action && task.action.length > 0) {
                        axios.post('/api/todos', task)
                        .then(res => {
                                if(res.data) {
                                        this.props.getTodos();
                                        this.setState({action:""})
                                }
                        })
                        .catch(err => console.log(err))
                }else {
                        console.log('input field required')
                }
        }

        handleChange = (e) => {
                this.setState({
                        action: e.target.value
                })
        }

        render() {
                let { action } = this.state;
                return (
                        <div>
                        <input type="text" onChange={this.handleChange} value={action} />
                        <button onClick={this.addTodo}>add todo</button>
                        </div>
                )
        }
}
export default Input
````  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ef38da83-fa08-4552-a92c-8234d6d655a4)

Then in your ```Todo.js``` file write the following code:  
````
import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

        state = {
                todos: []
        }

        componentDidMount(){
                this.getTodos();
        }

        getTodos = () => {
                axios.get('/api/todos')
                .then(res => {
                        if(res.data){
                                this.setState({
                                        todos: res.data
                                })
                        }
                })
                .catch(err => console.log(err))
        }

        deleteTodo = (id) => {
                axios.delete(`/api/todos/${id}`)
                .then(res => {
                        if(res.data){
                                this.getTodos()
                        }
                })
                .catch(err => console.log(err))
        }

        render() {
                let { todos } = this.state;

                return(
                        <div>
                        <h1>My Todo(s)</h1>
                        <Input getTodos={this.getTodos}/>
                        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
                        </div>
                )
        }
}
export default Todo;
````  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ccc2c1b8-137a-41ff-8f3a-0c42dd9afe1b)

Navigate to the ```src``` folder and open the ```App.js``` file in the editor and write the following command:  
````
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
        return (
                <div className="App">
                <Todo />
                </div>
        );
}
export default App;
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/ddb522ca-4af2-4ae5-8a3c-b6890c361fd7)

Open the ```App.css``` file in the text editor and write the following code:  
````
.App {
  text-align: center;
  font-size: calc(10px + 2vmin);
  width: 60%;
  margin-left: auto;
  margin-right: auto;
}

input {
        height: 40px;
        width: 50%;
        border: none;
        border-bottom: 2px #101113 solid;
        background: none;
        font-size: 1.5rem;
        color: #787a80;
}

input:focus {
        outline: none;
}

button {
        width: 25%;
        height: 45px;
        border: none;
        margin-left: 10px;
        font-size: 25px;
        background: #101113;
        border-radius: 5px;
        color: #787a80;
        cursor: pointer;
}

button:focus {
        outline: none;
}

ul {
        list-style: none;
        text-align: left;
        padding: 15px;
        background: #171a1f;
        border-radius: 5px;
}

li {
        padding: 15px;
        font-size: 1.5rem;
        margin-bottom: 15px;
        background: #282c34;
        border-radius: 5px;
        overflow-wrap: break-word;
        cursor: pointer;
}

@media only screen and (min-width: 300px) {
 .App {
                width: 80%;
        }

        input {
                width: 100%;
        }

        button {
                width: 100%;
                margin-top: 15px;
                margin-left: 0;
        }
}

@media only screen and (min-width:640px) {
        .App {
                width: 60%;
        }

        input {
                width: 50%;
        }

        button {
                width: 30%;
                margin-left: 10px;
                margin-top: 0;
        }
}
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/0d2b54b2-ff80-41e7-ac8a-db3b9d213c71)

In the same ```src``` directory open the ```index.css``` with the file editor and write the following command:  
````
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}
````
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/9914d588-0000-4b46-b4b1-bd7a010ee886)

Navigate back to the ```Todo``` directory and run the command ```npm run dev```  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/949224d0-9a97-4fd5-9bf6-41ed35f88377)

Navigate to the URL ```http://Public_IP:3000``` to view the new app:  
![image](https://github.com/gideonsngo/DevOpsTraining/assets/74353147/c6d62cc6-8d5a-40da-ae4f-1647e6554921)


