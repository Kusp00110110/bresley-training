# Exercise 2: JavaScript Hello World

## Overview
In this exercise, you'll write your first JavaScript code to display "Hello, World!" and learn how JavaScript adds interactivity to web pages.

## Prerequisites
- Completed Exercise 1: HTML Hello World
- Understanding of basic HTML structure
- A text editor and web browser

## Learning Objectives
- Understand what JavaScript is and how it works with HTML
- Learn how to include JavaScript in an HTML page
- Use `console.log()` and `alert()` functions
- Manipulate the DOM (Document Object Model)

## Instructions

### Step 1: Create a New Repository
1. Go to GitHub.com
2. Create a new repository named `javascript-hello-world`
3. Add a description: "My first JavaScript program"
4. Initialize with a README
5. Clone it to your local machine

```bash
git clone https://github.com/YOUR-USERNAME/javascript-hello-world.git
cd javascript-hello-world
```

### Step 2: Create HTML with Embedded JavaScript
Create a file called `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Hello World</title>
</head>
<body>
    <h1>JavaScript Hello World</h1>
    <p id="message">Waiting for JavaScript...</p>
    
    <script>
        // This is a comment in JavaScript
        console.log("Hello, World! Check the browser console!");
        
        // Show an alert
        alert("Hello, World!");
        
        // Change the paragraph text
        document.getElementById("message").textContent = "Hello from JavaScript!";
    </script>
</body>
</html>
```

### Step 3: Test Your Page
1. Open `index.html` in your browser
2. You should see:
   - An alert box with "Hello, World!"
   - The page heading
   - The paragraph text changed to "Hello from JavaScript!"
3. Open the browser console (F12 or right-click → Inspect → Console tab)
4. You should see "Hello, World! Check the browser console!" in the console

### Step 4: Create an External JavaScript File
Create a new file called `script.js`:

```javascript
// Function to greet the user
function greetUser() {
    let userName = prompt("What's your name?");
    
    if (userName) {
        document.getElementById("greeting").textContent = 
            "Hello, " + userName + "! Welcome to JavaScript!";
    }
}

// Add event listener when page loads
console.log("JavaScript file loaded successfully!");
```

### Step 5: Update HTML to Use External Script
Modify `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JavaScript Hello World</title>
</head>
<body>
    <h1>JavaScript Hello World</h1>
    <p id="greeting">Click the button to say hello!</p>
    <button onclick="greetUser()">Click Me!</button>
    
    <!-- External JavaScript file -->
    <script src="script.js"></script>
</body>
</html>
```

### Step 6: Commit and Push
```bash
git add index.html script.js
git commit -m "Add JavaScript Hello World with interactive greeting"
git push origin main
```

## Understanding the Code

### JavaScript Basics
- `console.log()` - Prints messages to the browser console (for debugging)
- `alert()` - Shows a popup message box
- `prompt()` - Shows a dialog box asking for user input
- `document.getElementById()` - Finds an HTML element by its ID
- `.textContent` - Gets or sets the text content of an element
- `function` - Defines a reusable block of code
- `//` - Single-line comment
- `/* */` - Multi-line comment

### Script Placement
- `<script>` tags can be in `<head>` or `<body>`
- Scripts in `<body>` run after HTML above them is loaded
- External scripts use `<script src="filename.js"></script>`

## Challenge Tasks

1. **Add a Clock**
   - Display the current time on the page
   - Update it every second using `setInterval()`

2. **Create a Counter**
   - Add buttons to increment and decrement a number
   - Display the current count on the page

3. **Make a Simple Calculator**
   - Create input fields for two numbers
   - Add buttons for +, -, ×, ÷
   - Display the result when a button is clicked

4. **Change Colors**
   - Add a button that changes the page background color randomly
   - Use `Math.random()` and `style.backgroundColor`

## Example Challenge Solution: Counter

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Counter</title>
</head>
<body>
    <h1>Counter: <span id="count">0</span></h1>
    <button onclick="increment()">+</button>
    <button onclick="decrement()">-</button>
    <button onclick="reset()">Reset</button>
    
    <script>
        let count = 0;
        
        function increment() {
            count++;
            updateDisplay();
        }
        
        function decrement() {
            count--;
            updateDisplay();
        }
        
        function reset() {
            count = 0;
            updateDisplay();
        }
        
        function updateDisplay() {
            document.getElementById("count").textContent = count;
        }
    </script>
</body>
</html>
```

## Expected Output
- Alert box appears with "Hello, World!"
- Page displays interactive greeting
- Console shows log messages
- Button click prompts for name and displays personalized greeting

## Troubleshooting
- **Nothing happens**: Check browser console (F12) for error messages
- **Script doesn't load**: Verify the script file path and name
- **Changes don't appear**: Hard refresh (Ctrl+Shift+R or Cmd+Shift+R)
- **Syntax errors**: Make sure all brackets and quotes are matched

## Next Steps
Move on to [Exercise 3: HTML + JavaScript Interactive Page](03-interactive-page.md) to build a more complex interactive application!

## Resources
- [MDN JavaScript Basics](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics)
- [JavaScript.info](https://javascript.info/)
- [W3Schools JavaScript Tutorial](https://www.w3schools.com/js/)
