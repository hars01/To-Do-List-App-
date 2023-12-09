<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="app-container">
        <h1>To-Do List</h1>
        <input type="text" id="taskInput" placeholder="Add a new task...">
        <button onclick="addTask()">Add</button>
        <ul id="taskList"></ul>
    </div>

    <script src="script.js"></script>
</body>
</html>
body {
    font-family: 'Arial', sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.app-container {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: center;
}

input {
    padding: 8px;
    margin-right: 8px;
}

button {
    padding: 8px;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    background-color: #e0e0e0;
    margin: 8px 0;
    padding: 8px;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
document.addEventListener("DOMContentLoaded", function() {
    // Check for saved tasks in localStorage
    const savedTasks = JSON.parse(localStorage.getItem("tasks")) || [];
    const taskList = document.getElementById("taskList");

    // Load saved tasks
    savedTasks.forEach(task => {
        addTaskToDOM(task);
    });

    // Add task function
    window.addTask = function() {
        const taskInput = document.getElementById("taskInput");
        const taskText = taskInput.value.trim();

        if (taskText !== "") {
            const newTask = { text: taskText, completed: false };
            savedTasks.push(newTask);
            addTaskToDOM(newTask);
            saveTasks();
            taskInput.value = "";
        }
    };

    // Add task to DOM function
    function addTaskToDOM(task) {
        const taskItem = document.createElement("li");
        taskItem.innerHTML = `
            <span>${task.text}</span>
            <button onclick="removeTask(this)">Remove</button>
        `;
        taskList.appendChild(taskItem);
    }

    // Remove task function
    window.removeTask = function(button) {
        const taskText = button.parentNode.firstChild.innerText;
        const taskIndex = savedTasks.findIndex(task => task.text === taskText);

        if (taskIndex !== -1) {
            savedTasks.splice(taskIndex, 1);
            saveTasks();
            button.parentNode.remove();
        }
    };

    // Save tasks to localStorage
    function saveTasks() {
        localStorage.setItem("tasks", JSON.stringify(savedTasks));
    }
});
