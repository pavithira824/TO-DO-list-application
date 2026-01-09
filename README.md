# TO-DO-list-application
A minimalistic and responsive To-Do List App showcasing fundamental web development skills. Includes task management features and persistent data storage using local Storage.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List App</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f6f7fb;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }

        .container {
            background: white;
            width: 350px;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }

        h1 {
            text-align: center;
            margin-bottom: 20px;
        }

        .input-section {
            display: flex;
            gap: 10px;
        }

        input {
            flex: 1;
            padding: 8px;
            border-radius: 8px;
            border: 1px solid #bbb;
        }

        button {
            padding: 8px 12px;
            background: #0088ff;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
        }

        button:hover {
            background: #006fd6;
        }

        ul {
            margin-top: 20px;
            padding: 0;
        }

        li {
            list-style: none;
            background: #e8f1ff;
            margin-bottom: 10px;
            padding: 10px;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            cursor: pointer;
        }

        .completed {
            text-decoration: line-through;
            opacity: 0.6;
        }
    </style>
</head>

<body>

    <div class="container">
        <h1>To-Do List</h1>

        <div class="input-section">
            <input type="text" id="taskInput" placeholder="Enter a task">
            <button id="addBtn">Add</button>
        </div>

        <ul id="taskList"></ul>
    </div>

    <script>
        const taskInput = document.getElementById("taskInput");
        const addBtn = document.getElementById("addBtn");
        const taskList = document.getElementById("taskList");

        window.onload = () => {
            const savedTasks = JSON.parse(localStorage.getItem("tasks")) || [];
            savedTasks.forEach(task => addTask(task.text, task.completed));
        };

        addBtn.addEventListener("click", () => {
            if (taskInput.value.trim() !== "") {
                addTask(taskInput.value, false);
                saveTasks();
                taskInput.value = "";
            }
        });

        function addTask(text, completed) {
            const li = document.createElement("li");
            li.textContent = text;

            if (completed) li.classList.add("completed");

            li.addEventListener("click", () => {
                li.classList.toggle("completed");
                saveTasks();
            });

            const del = document.createElement("span");
            del.textContent = "❌";
            del.style.cursor = "pointer";
            del.addEventListener("click", (e) => {
                e.stopPropagation();
                li.remove();
                saveTasks();
            });

            li.appendChild(del);
            taskList.appendChild(li);
        }

        function saveTasks() {
            const tasks = [];
            document.querySelectorAll("li").forEach(li => {
                tasks.push({
                    text: li.firstChild.textContent,
                    completed: li.classList.contains("completed")
                });
            });
            localStorage.setItem("tasks", JSON.stringify(tasks));
        }
    </script>

</body>
</html>
