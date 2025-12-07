# Exercise 3: HTML + JavaScript Interactive Page

## Overview
Build a complete interactive web page combining HTML, CSS, and JavaScript to create a simple task list application.

## Prerequisites
- Completed Exercises 1 and 2
- Basic understanding of HTML and JavaScript
- Familiarity with DOM manipulation

## Learning Objectives
- Build a complete interactive web application
- Learn CSS for styling
- Practice event handling
- Work with arrays and DOM manipulation
- Understand local storage (optional)

## Instructions

### Step 1: Create a New Repository
```bash
# Create repository on GitHub named: interactive-todo-app
git clone https://github.com/YOUR-USERNAME/interactive-todo-app.git
cd interactive-todo-app
```

### Step 2: Create the HTML Structure
Create `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My To-Do List</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>My To-Do List</h1>
        
        <div class="input-section">
            <input type="text" id="taskInput" placeholder="Enter a new task...">
            <button onclick="addTask()">Add Task</button>
        </div>
        
        <ul id="taskList"></ul>
        
        <div class="stats">
            <p>Total Tasks: <span id="totalTasks">0</span></p>
            <p>Completed: <span id="completedTasks">0</span></p>
        </div>
    </div>
    
    <script src="app.js"></script>
</body>
</html>
```

### Step 3: Create the CSS Styling
Create `styles.css`:

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

.container {
    background: white;
    border-radius: 10px;
    padding: 30px;
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
    max-width: 500px;
    width: 100%;
}

h1 {
    color: #333;
    margin-bottom: 20px;
    text-align: center;
}

.input-section {
    display: flex;
    gap: 10px;
    margin-bottom: 20px;
}

input[type="text"] {
    flex: 1;
    padding: 12px;
    border: 2px solid #ddd;
    border-radius: 5px;
    font-size: 16px;
}

input[type="text"]:focus {
    outline: none;
    border-color: #667eea;
}

button {
    padding: 12px 24px;
    background: #667eea;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.3s;
}

button:hover {
    background: #5568d3;
}

#taskList {
    list-style: none;
    margin-bottom: 20px;
}

.task-item {
    background: #f8f9fa;
    padding: 15px;
    margin-bottom: 10px;
    border-radius: 5px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    transition: all 0.3s;
}

.task-item:hover {
    background: #e9ecef;
}

.task-item.completed {
    opacity: 0.6;
    text-decoration: line-through;
}

.task-text {
    flex: 1;
    cursor: pointer;
}

.delete-btn {
    background: #dc3545;
    padding: 8px 16px;
    font-size: 14px;
}

.delete-btn:hover {
    background: #c82333;
}

.stats {
    text-align: center;
    color: #666;
    padding-top: 20px;
    border-top: 2px solid #eee;
}

.stats p {
    margin: 5px 0;
}

.stats span {
    font-weight: bold;
    color: #667eea;
}
```

### Step 4: Create the JavaScript Logic
Create `app.js`:

```javascript
// Array to store tasks
let tasks = [];

// Add a new task
function addTask() {
    const input = document.getElementById('taskInput');
    const taskText = input.value.trim();
    
    if (taskText === '') {
        alert('Please enter a task!');
        return;
    }
    
    // Create task object
    const task = {
        id: Date.now(),
        text: taskText,
        completed: false
    };
    
    tasks.push(task);
    input.value = '';
    renderTasks();
}

// Render all tasks
function renderTasks() {
    const taskList = document.getElementById('taskList');
    taskList.innerHTML = '';
    
    tasks.forEach(task => {
        const li = document.createElement('li');
        li.className = 'task-item' + (task.completed ? ' completed' : '');
        
        li.innerHTML = `
            <span class="task-text" onclick="toggleTask(${task.id})">
                ${task.text}
            </span>
            <button class="delete-btn" onclick="deleteTask(${task.id})">Delete</button>
        `;
        
        taskList.appendChild(li);
    });
    
    updateStats();
}

// Toggle task completion
function toggleTask(id) {
    const task = tasks.find(t => t.id === id);
    if (task) {
        task.completed = !task.completed;
        renderTasks();
    }
}

// Delete a task
function deleteTask(id) {
    tasks = tasks.filter(t => t.id !== id);
    renderTasks();
}

// Update statistics
function updateStats() {
    const total = tasks.length;
    const completed = tasks.filter(t => t.completed).length;
    
    document.getElementById('totalTasks').textContent = total;
    document.getElementById('completedTasks').textContent = completed;
}

// Allow Enter key to add task
document.addEventListener('DOMContentLoaded', () => {
    const input = document.getElementById('taskInput');
    input.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            addTask();
        }
    });
});
```

### Step 5: Test Your Application
1. Open `index.html` in your browser
2. Try adding tasks
3. Click tasks to mark them as complete
4. Delete tasks
5. Check that statistics update correctly

### Step 6: Commit Your Changes
```bash
git add index.html styles.css app.js
git commit -m "Create interactive to-do list application"
git push origin main
```

## Challenge Tasks

### 1. Add Local Storage
Save tasks to browser storage so they persist after page refresh:

```javascript
// Save to local storage
function saveTasks() {
    localStorage.setItem('tasks', JSON.stringify(tasks));
}

// Load from local storage
function loadTasks() {
    const saved = localStorage.getItem('tasks');
    if (saved) {
        tasks = JSON.parse(saved);
        renderTasks();
    }
}

// Call loadTasks on page load
document.addEventListener('DOMContentLoaded', loadTasks);

// Update saveTasks() calls in addTask, toggleTask, and deleteTask
```

### 2. Add Task Editing
Allow users to edit existing tasks.

### 3. Add Categories or Tags
Let users categorize tasks (work, personal, etc.).

### 4. Add a Clear Completed Button
Remove all completed tasks at once.

### 5. Add Task Priority
Color-code tasks by priority (high, medium, low).

## Expected Output
A fully functional to-do list application with:
- Ability to add new tasks
- Click tasks to mark complete/incomplete
- Delete individual tasks
- Real-time statistics
- Responsive and attractive design

## Key Concepts Learned
- **Event Handling**: onclick, keypress events
- **DOM Manipulation**: createElement, innerHTML, appendChild
- **Array Methods**: push, filter, find, forEach
- **CSS Flexbox**: Layout and alignment
- **JavaScript Functions**: Reusable code blocks
- **Data Structures**: Working with objects and arrays

## Troubleshooting
- **Tasks not displaying**: Check console for JavaScript errors
- **Styling not working**: Verify CSS file is linked correctly
- **Button not working**: Ensure function names match in HTML and JS

## Next Steps
Continue to [Exercise 4: Git/VCS Basics](04-git-basics.md) to learn version control!

## Resources
- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web)
- [CSS Tricks](https://css-tricks.com/)
- [JavaScript Array Methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
