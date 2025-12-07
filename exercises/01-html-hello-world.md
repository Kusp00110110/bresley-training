# Exercise 1: HTML Hello World

## Overview
In this exercise, you'll create your first HTML page that displays "Hello, World!" in a web browser. This is the foundation of web development!

## Prerequisites
- A text editor (VS Code, Notepad++, or any code editor)
- A web browser (Chrome, Firefox, Edge, or Safari)

## Learning Objectives
- Understand the basic structure of an HTML document
- Learn about essential HTML tags
- Create and open an HTML file in a browser

## Instructions

### Step 1: Create a New Repository
1. Go to GitHub.com and sign in
2. Click the "+" icon in the top right and select "New repository"
3. Name it `html-hello-world`
4. Add a description: "My first HTML page"
5. Select "Public"
6. Check "Add a README file"
7. Click "Create repository"

### Step 2: Clone the Repository
```bash
git clone https://github.com/YOUR-USERNAME/html-hello-world.git
cd html-hello-world
```

### Step 3: Create Your HTML File
Create a new file called `index.html` with the following content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hello World</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>Welcome to my first HTML page!</p>
</body>
</html>
```

### Step 4: Test Your Page
1. Open `index.html` in your web browser (double-click the file or right-click and choose "Open with")
2. You should see "Hello, World!" as a heading and a welcome message

### Step 5: Commit and Push Your Changes
```bash
git add index.html
git commit -m "Add Hello World HTML page"
git push origin main
```

## Understanding the Code

- `<!DOCTYPE html>` - Declares this is an HTML5 document
- `<html>` - The root element of the HTML page
- `<head>` - Contains meta information about the document
- `<meta charset="UTF-8">` - Specifies the character encoding
- `<title>` - Sets the page title (shown in browser tab)
- `<body>` - Contains the visible page content
- `<h1>` - Creates a level 1 heading (largest)
- `<p>` - Creates a paragraph

## Challenge Tasks
Once you've completed the basic exercise, try these challenges:

1. Add more content:
   - Add another paragraph with your name
   - Add a level 2 heading (`<h2>`)
   - Add a list of your favorite hobbies using `<ul>` and `<li>` tags

2. Add an image:
   - Find a free image online
   - Use the `<img>` tag to display it
   - Example: `<img src="https://example.com/image.jpg" alt="Description">`

3. Add a link:
   - Use the `<a>` tag to link to another website
   - Example: `<a href="https://github.com">GitHub</a>`

## Expected Output
Your webpage should display:
- A large "Hello, World!" heading
- A welcome message paragraph
- Any additional content you added in the challenges

## Troubleshooting
- **Page doesn't display correctly**: Make sure all tags are properly closed
- **File won't open**: Ensure the file extension is `.html`, not `.txt`
- **Changes don't show**: Refresh your browser (F5 or Ctrl+R)

## Next Steps
Once you've completed this exercise, move on to [Exercise 2: JavaScript Hello World](02-javascript-hello-world.md) to add interactivity to your webpage!

## Resources
- [MDN HTML Basics](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/HTML_basics)
- [W3Schools HTML Tutorial](https://www.w3schools.com/html/)
