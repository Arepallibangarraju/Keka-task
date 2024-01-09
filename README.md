#html code
<<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="task-container">
        <h1>To-Do List</h1>
        <form id="task-form">
            <label for="title">Title:</label>
            <input type="text" id="title" required>
            <label for="description">Description:</label>
            <textarea id="description" rows="3" required></textarea>
            <label for="due-date">Due Date:</label>
            <input type="date" id="due-date" required>
            <label for="priority">Priority:</label>
            <select id="priority">
                <option value="Low">Low</option>
                <option value="Medium">Medium</option>
                <option value="High">High</option>
            </select>
            <label for="category">Category:</label>
            <input type="text" id="category">
            <button type="button" onclick="addTask()">Add Task</button>
        </form>
        <ul id="task-list"></ul>
    </div>

    <script src="script.js"></script>
</body>
</html>

#css
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f0f0f0;
    margin: 0;
    padding: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100vh;
}

#task-container {
    max-width: 400px;
    width: 100%;
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

#task-container h1 {
    color: #00adb5;
    text-align: center;
}

form {
    display: flex;
    flex-direction: column;
}

label {
    margin-top: 10px;
    font-size: 14px;
    color: #333;
}

input, textarea, select {
    margin-bottom: 10px;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    background-color: #f8f8f8;
    color: #333;
}

button {
    background-color: #00adb5;
    color: #fff;
    border: none;
    padding: 10px;
    border-radius: 4px;
    cursor: pointer;
}

button:hover {
    background-color: #00787c;
}

#task-list {
    list-style: none;
    padding: 0;
}

.task {
    margin: 10px 0;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #f8f8f8;
}

.task input[type="checkbox"] {
    margin-right: 10px;
}

.task .task-details {
    flex: 1;
    color: #333;
}

.task button {
    background-color: #ff5a5f;
    color: #fff;
    border: none;
    padding: 6px 12px;
    border-radius: 4px;
    cursor: pointer;
}

.task button:hover {
    background-color: #e44d52;
}


#javascript
document.addEventListener("DOMContentLoaded", function () {
    loadTasks();
});

function addTask() {
    const title = document.getElementById("title").value;
    const description = document.getElementById("description").value;
    const dueDate = document.getElementById("due-date").value;
    const priority = document.getElementById("priority").value;
    const category = document.getElementById("category").value;

    const task = {
        title: title,
        description: description,
        dueDate: dueDate,
        priority: priority,
        category: category,
        status: "New"
    };

    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks.push(task);
    localStorage.setItem("tasks", JSON.stringify(tasks));

    loadTasks();

    document.getElementById("title").value = "";
    document.getElementById("description").value = "";
    document.getElementById("due-date").value = "";
    document.getElementById("priority").value = "Low";
    document.getElementById("category").value = "";
}

function updateTaskStatus(index, newStatus) {
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks[index].status = newStatus;
    localStorage.setItem("tasks", JSON.stringify(tasks));

    loadTasks();
}

function deleteTask(index) {
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks.splice(index, 1);
    localStorage.setItem("tasks", JSON.stringify(tasks));

    loadTasks();
}

function loadTasks() {
    const taskList = document.getElementById("task-list");
    taskList.innerHTML = "";

    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

    tasks.forEach((task, index) => {
        const taskItem = document.createElement("li");
        taskItem.className = "task";

        const checkbox = document.createElement("input");
        checkbox.type = "checkbox";
        checkbox.checked = task.status === "Completed";
        checkbox.addEventListener("change", () => {
            updateTaskStatus(index, checkbox.checked ? "Completed" : "In Progress");
        });

        const taskDetails = document.createElement("div");
        taskDetails.className = "task-details";
        taskDetails.innerHTML = `<strong>${task.title}</strong><br>${task.description}<br>Due Date:
