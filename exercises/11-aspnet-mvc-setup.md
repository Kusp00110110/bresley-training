# Exercise 11: ASP.NET Core MVC - Basic Setup

## Overview
Learn to build web applications using ASP.NET Core MVC (Model-View-Controller). Create your first web application with dynamic pages and server-side logic.

## Prerequisites
- Completed Exercise 10: C# Interactive Program
- .NET SDK installed (version 6.0 or later)
- Basic understanding of C# and OOP
- Web browser
- Code editor (VS Code recommended with C# extension)

## Learning Objectives
- Understand the MVC pattern
- Set up an ASP.NET Core MVC project
- Create controllers and views
- Work with routing
- Pass data from controllers to views
- Use Razor syntax
- Understand the request/response cycle

## What is ASP.NET Core MVC?

**ASP.NET Core MVC** is a framework for building web applications using the Model-View-Controller pattern:

- **Model**: Data and business logic
- **View**: User interface (HTML)
- **Controller**: Handles requests and coordinates model and view

**Benefits:**
- Separation of concerns
- Testable code
- Reusable components
- Powerful routing system
- Cross-platform (Windows, Mac, Linux)

## Instructions

### Step 1: Create a New Repository

```bash
# Create repo on GitHub: aspnet-mvc-hello-world
git clone https://github.com/YOUR-USERNAME/aspnet-mvc-hello-world.git
cd aspnet-mvc-hello-world
```

### Step 2: Create an MVC Project

```bash
# Create new MVC application
dotnet new mvc -n HelloWorldMvc
cd HelloWorldMvc

# Run the application
dotnet run
```

You should see:
```
Now listening on: http://localhost:5000
```

Open your browser to `http://localhost:5000`

### Step 3: Explore the Project Structure

```
HelloWorldMvc/
├── Controllers/          # Handle requests
│   └── HomeController.cs
├── Views/               # UI templates
│   ├── Home/
│   │   ├── Index.cshtml
│   │   └── Privacy.cshtml
│   └── Shared/
│       ├── _Layout.cshtml
│       └── Error.cshtml
├── Models/              # Data and logic
├── wwwroot/             # Static files (CSS, JS, images)
│   ├── css/
│   ├── js/
│   └── lib/
├── appsettings.json     # Configuration
└── Program.cs           # Entry point
```

### Step 4: Understand the Default Controller

Open `Controllers/HomeController.cs`:

```csharp
using Microsoft.AspNetCore.Mvc;
using HelloWorldMvc.Models;
using System.Diagnostics;

namespace HelloWorldMvc.Controllers
{
    public class HomeController : Controller
    {
        private readonly ILogger<HomeController> _logger;

        public HomeController(ILogger<HomeController> logger)
        {
            _logger = logger;
        }

        // GET: /Home/Index or just /
        public IActionResult Index()
        {
            return View();
        }

        // GET: /Home/Privacy
        public IActionResult Privacy()
        {
            return View();
        }

        [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
        public IActionResult Error()
        {
            return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
        }
    }
}
```

### Step 5: Create Your First Custom Action

Add a new action to `HomeController.cs`:

```csharp
// Add this method to HomeController
public IActionResult Welcome()
{
    ViewData["Message"] = "Welcome to ASP.NET Core MVC!";
    ViewData["CurrentTime"] = DateTime.Now.ToString("F");
    
    return View();
}

public IActionResult Greet(string name)
{
    ViewData["UserName"] = name ?? "Guest";
    return View();
}
```

### Step 6: Create Views for Your Actions

Create `Views/Home/Welcome.cshtml`:

```html
@{
    ViewData["Title"] = "Welcome";
}

<div class="text-center">
    <h1 class="display-4">@ViewData["Title"]</h1>
    <p class="lead">@ViewData["Message"]</p>
    
    <div class="alert alert-info mt-4">
        <strong>Current Time:</strong> @ViewData["CurrentTime"]
    </div>
    
    <div class="mt-4">
        <a asp-action="Index" class="btn btn-primary">Back to Home</a>
    </div>
</div>
```

Create `Views/Home/Greet.cshtml`:

```html
@{
    ViewData["Title"] = "Greetings";
}

<div class="text-center">
    <h1 class="display-4">Hello, @ViewData["UserName"]!</h1>
    <p class="lead">Welcome to our ASP.NET Core MVC application.</p>
    
    <div class="card mt-4 mx-auto" style="max-width: 500px;">
        <div class="card-body">
            <h5 class="card-title">Quick Facts</h5>
            <ul class="list-group list-group-flush">
                <li class="list-group-item">Framework: ASP.NET Core</li>
                <li class="list-group-item">Pattern: MVC</li>
                <li class="list-group-item">Language: C#</li>
            </ul>
        </div>
    </div>
    
    <div class="mt-4">
        <a asp-action="Welcome" class="btn btn-secondary">See Welcome</a>
        <a asp-action="Index" class="btn btn-primary">Go Home</a>
    </div>
</div>
```

### Step 7: Update the Layout Navigation

Edit `Views/Shared/_Layout.cshtml` to add links to your new pages.

Find the `<ul class="navbar-nav flex-grow-1">` section and add:

```html
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Welcome">Welcome</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Greet" asp-route-name="Developer">Greet</a>
</li>
```

### Step 8: Create a Model

Create `Models/Person.cs`:

```csharp
namespace HelloWorldMvc.Models
{
    public class Person
    {
        public string FirstName { get; set; } = string.Empty;
        public string LastName { get; set; } = string.Empty;
        public string Email { get; set; } = string.Empty;
        public int Age { get; set; }
        
        public string FullName => $"{FirstName} {LastName}";
        
        public string AgeGroup
        {
            get
            {
                if (Age < 18) return "Minor";
                if (Age < 65) return "Adult";
                return "Senior";
            }
        }
    }
}
```

### Step 9: Create a Controller Action with a Model

Add to `HomeController.cs`:

```csharp
public IActionResult Person()
{
    var person = new Person
    {
        FirstName = "John",
        LastName = "Doe",
        Email = "john.doe@example.com",
        Age = 30
    };
    
    return View(person);
}
```

Create `Views/Home/Person.cshtml`:

```html
@model HelloWorldMvc.Models.Person

@{
    ViewData["Title"] = "Person Details";
}

<div class="container mt-4">
    <h1 class="display-4">Person Information</h1>
    
    <div class="card mt-4">
        <div class="card-header bg-primary text-white">
            <h3>@Model.FullName</h3>
        </div>
        <div class="card-body">
            <table class="table">
                <tr>
                    <th>First Name:</th>
                    <td>@Model.FirstName</td>
                </tr>
                <tr>
                    <th>Last Name:</th>
                    <td>@Model.LastName</td>
                </tr>
                <tr>
                    <th>Email:</th>
                    <td><a href="mailto:@Model.Email">@Model.Email</a></td>
                </tr>
                <tr>
                    <th>Age:</th>
                    <td>@Model.Age years old</td>
                </tr>
                <tr>
                    <th>Category:</th>
                    <td>
                        <span class="badge bg-info">@Model.AgeGroup</span>
                    </td>
                </tr>
            </table>
        </div>
    </div>
    
    <div class="mt-3">
        <a asp-action="Index" class="btn btn-primary">Back to Home</a>
    </div>
</div>
```

### Step 10: Test Your Application

```bash
dotnet run
```

Visit these URLs:
- `http://localhost:5000/` - Home page
- `http://localhost:5000/Home/Welcome` - Welcome page
- `http://localhost:5000/Home/Greet?name=YourName` - Greeting page
- `http://localhost:5000/Home/Person` - Person details

### Step 11: Add .gitignore and Commit

```bash
cd ..  # Go to repo root

cat > .gitignore << 'EOF'
## Ignore Visual Studio temporary files, build results, and
## files generated by popular Visual Studio add-ons.

# User-specific files
*.suo
*.user
*.userosscache
*.sln.docstates

# Build results
[Dd]ebug/
[Rr]elease/
[Bb]in/
[Oo]bj/

# Visual Studio
.vs/

# VS Code
.vscode/

# User-specific files (MonoDevelop/Xamarin Studio)
*.userprefs

# Build artifacts
*.dll
*.exe
*.pdb

# NuGet
*.nupkg
packages/

# Application specific
appsettings.Development.json
EOF

git add .
git commit -m "Add ASP.NET Core MVC Hello World application"
git push origin main
```

## Understanding MVC Components

### Controllers

Controllers handle HTTP requests and return responses:

```csharp
public class ProductController : Controller
{
    // GET: /Product/Index
    public IActionResult Index()
    {
        return View();
    }
    
    // GET: /Product/Details/5
    public IActionResult Details(int id)
    {
        // Fetch product with id
        return View(product);
    }
    
    // POST: /Product/Create
    [HttpPost]
    public IActionResult Create(Product product)
    {
        // Save product
        return RedirectToAction("Index");
    }
}
```

### Views (Razor Syntax)

Razor allows mixing C# and HTML:

```html
@* This is a Razor comment *@

@* Display variable *@
<p>@ViewData["Message"]</p>

@* C# code block *@
@{
    var name = "John";
    var age = 30;
}

@* Conditional rendering *@
@if (age >= 18)
{
    <p>Adult</p>
}
else
{
    <p>Minor</p>
}

@* Loop *@
<ul>
@for (int i = 0; i < 5; i++)
{
    <li>Item @i</li>
}
</ul>

@* Strongly-typed model *@
@model MyApp.Models.Person
<h1>@Model.FullName</h1>
```

### Routing

Default routing pattern: `{controller=Home}/{action=Index}/{id?}`

Examples:
- `/` → HomeController.Index()
- `/Home/Privacy` → HomeController.Privacy()
- `/Product/Details/5` → ProductController.Details(5)

Custom routes in `Program.cs`:
```csharp
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

### Passing Data to Views

**Method 1: ViewData (loosely typed)**
```csharp
// Controller
ViewData["Message"] = "Hello";

// View
<p>@ViewData["Message"]</p>
```

**Method 2: ViewBag (dynamic)**
```csharp
// Controller
ViewBag.Message = "Hello";

// View
<p>@ViewBag.Message</p>
```

**Method 3: Strongly-typed Model (preferred)**
```csharp
// Controller
var model = new Person { Name = "John" };
return View(model);

// View
@model MyApp.Models.Person
<p>@Model.Name</p>
```

## Razor Tag Helpers

Tag helpers make Razor more readable:

```html
@* Link to action *@
<a asp-controller="Home" asp-action="Index">Home</a>

@* Form *@
<form asp-action="Create" method="post">
    <input asp-for="Name" />
    <button type="submit">Submit</button>
</form>

@* Image *@
<img asp-append-version="true" src="~/images/logo.png" />

@* Script with cache busting *@
<script asp-append-version="true" src="~/js/site.js"></script>
```

## Practice Exercise: Personal Portfolio

Create a personal portfolio website:

```csharp
// Models/Project.cs
public class Project
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public string TechnologyUsed { get; set; }
    public string GitHubUrl { get; set; }
}

// Controllers/PortfolioController.cs
public class PortfolioController : Controller
{
    public IActionResult Index()
    {
        var projects = new List<Project>
        {
            new Project { 
                Id = 1, 
                Name = "Todo App", 
                Description = "A task management application",
                TechnologyUsed = "JavaScript, HTML, CSS",
                GitHubUrl = "https://github.com/user/todo-app"
            },
            new Project { 
                Id = 2, 
                Name = "Weather App", 
                Description = "Shows weather information",
                TechnologyUsed = "C#, ASP.NET Core",
                GitHubUrl = "https://github.com/user/weather-app"
            }
        };
        
        return View(projects);
    }
    
    public IActionResult About()
    {
        return View();
    }
    
    public IActionResult Contact()
    {
        return View();
    }
}
```

## Challenge Tasks

### 1. Create an About Me Page
- Add personal information
- Education history
- Skills list
- Social media links

### 2. Build a Blog
- List of blog posts
- Individual post pages
- Categories
- Date formatting

### 3. Product Catalog
- Display products
- Product details page
- Search functionality
- Categories

### 4. Calculator Web App
- Form with two numbers
- Operation selector
- Display result
- Calculation history

### 5. Contact Form
- Name, email, message fields
- Form validation
- Display submitted data
- Success message

## Development Tools

### Hot Reload

Run with hot reload (auto-refresh on changes):
```bash
dotnet watch run
```

### Debugging in VS Code

`.vscode/launch.json`:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": ".NET Core Launch (web)",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            "program": "${workspaceFolder}/bin/Debug/net6.0/HelloWorldMvc.dll",
            "args": [],
            "cwd": "${workspaceFolder}",
            "stopAtEntry": false,
            "serverReadyAction": {
                "action": "openExternally",
                "pattern": "\\bNow listening on:\\s+(https?://\\S+)"
            },
            "env": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    ]
}
```

## Best Practices

✅ Use strongly-typed models
✅ Keep controllers thin
✅ Use async/await for I/O operations
✅ Follow naming conventions
✅ Use layout pages for common structure
✅ Separate business logic from controllers
✅ Use dependency injection
✅ Handle errors gracefully

## Expected Output
- Working ASP.NET Core MVC application
- Multiple pages with navigation
- Data passed from controller to view
- Understanding of MVC pattern

## Troubleshooting

**Problem**: Port already in use
**Solution**: Change port in `appsettings.json` or kill the process

**Problem**: View not found
**Solution**: Check view location: `Views/{Controller}/{Action}.cshtml`

**Problem**: CSS not loading
**Solution**: Ensure files are in `wwwroot` and use `~/` prefix

## Next Steps
Continue to [Exercise 12: ASP.NET Core MVC - Interactive Web App](12-aspnet-mvc-interactive.md) to build a complete CRUD application!

## Resources
- [ASP.NET Core Documentation](https://docs.microsoft.com/en-us/aspnet/core/)
- [MVC Tutorial](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/)
- [Razor Syntax Reference](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/razor)
- [Tag Helpers](https://docs.microsoft.com/en-us/aspnet/core/mvc/views/tag-helpers/intro)
