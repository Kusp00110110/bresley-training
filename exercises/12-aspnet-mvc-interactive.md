# Exercise 12: ASP.NET Core MVC - Interactive Web Application

## Overview
Build a complete, interactive web application using ASP.NET Core MVC with full CRUD (Create, Read, Update, Delete) functionality, forms, validation, and a database.

## Prerequisites
- Completed Exercise 11: ASP.NET MVC Setup
- .NET SDK 6.0 or later
- Basic understanding of MVC pattern
- SQL Server or SQLite (we'll use SQLite for simplicity)
- Code editor

## Learning Objectives
- Implement CRUD operations
- Work with Entity Framework Core
- Use forms with model binding
- Add data validation
- Handle form submissions
- Use databases with migrations
- Implement responsive design
- Add navigation and layouts

## What You'll Build

A **Task Management Web Application** with:
- View all tasks
- Create new tasks
- Edit existing tasks
- Delete tasks
- Mark tasks as complete
- Filter and search tasks

## Instructions

### Step 1: Create a New Repository

```bash
# Create repo on GitHub: aspnet-task-manager
git clone https://github.com/YOUR-USERNAME/aspnet-task-manager.git
cd aspnet-task-manager

# Create MVC application
dotnet new mvc -n TaskManager
cd TaskManager
```

### Step 2: Install Required Packages

```bash
# Entity Framework Core with SQLite
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Step 3: Create the Task Model

Create `Models/TaskItem.cs`:

```csharp
using System;
using System.ComponentModel.DataAnnotations;

namespace TaskManager.Models
{
    public class TaskItem
    {
        public int Id { get; set; }
        
        [Required(ErrorMessage = "Title is required")]
        [StringLength(100, MinimumLength = 3, ErrorMessage = "Title must be between 3 and 100 characters")]
        [Display(Name = "Task Title")]
        public string Title { get; set; } = string.Empty;
        
        [StringLength(500, ErrorMessage = "Description cannot exceed 500 characters")]
        public string? Description { get; set; }
        
        [Display(Name = "Due Date")]
        [DataType(DataType.Date)]
        public DateTime? DueDate { get; set; }
        
        [Display(Name = "Priority")]
        public Priority Priority { get; set; } = Priority.Medium;
        
        [Display(Name = "Completed")]
        public bool IsCompleted { get; set; }
        
        [Display(Name = "Created At")]
        public DateTime CreatedAt { get; set; } = DateTime.Now;
        
        // Computed property
        public bool IsOverdue => DueDate.HasValue && DueDate.Value < DateTime.Now && !IsCompleted;
        
        public string PriorityBadgeClass => Priority switch
        {
            Priority.High => "badge bg-danger",
            Priority.Medium => "badge bg-warning",
            Priority.Low => "badge bg-info",
            _ => "badge bg-secondary"
        };
    }
    
    public enum Priority
    {
        Low = 1,
        Medium = 2,
        High = 3
    }
}
```

### Step 4: Create the Database Context

Create `Data/ApplicationDbContext.cs`:

```csharp
using Microsoft.EntityFrameworkCore;
using TaskManager.Models;

namespace TaskManager.Data
{
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
        
        public DbSet<TaskItem> Tasks { get; set; } = null!;
        
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);
            
            // Seed some initial data
            modelBuilder.Entity<TaskItem>().HasData(
                new TaskItem
                {
                    Id = 1,
                    Title = "Complete ASP.NET Core tutorial",
                    Description = "Finish all exercises in the tutorial",
                    Priority = Priority.High,
                    DueDate = DateTime.Now.AddDays(7),
                    CreatedAt = DateTime.Now
                },
                new TaskItem
                {
                    Id = 2,
                    Title = "Learn Entity Framework",
                    Description = "Study EF Core documentation",
                    Priority = Priority.Medium,
                    DueDate = DateTime.Now.AddDays(14),
                    CreatedAt = DateTime.Now
                }
            );
        }
    }
}
```

### Step 5: Configure Services

Update `Program.cs`:

```csharp
using Microsoft.EntityFrameworkCore;
using TaskManager.Data;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllersWithViews();

// Add DbContext
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlite(builder.Configuration.GetConnectionString("DefaultConnection")));

var app = builder.Build();

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();

app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Tasks}/{action=Index}/{id?}");

app.Run();
```

### Step 6: Add Connection String

Update `appsettings.json`:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=taskmanager.db"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### Step 7: Create Database Migration

```bash
# Create initial migration
dotnet ef migrations add InitialCreate

# Apply migration to create database
dotnet ef database update
```

### Step 8: Create the Tasks Controller

Create `Controllers/TasksController.cs`:

```csharp
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using TaskManager.Data;
using TaskManager.Models;

namespace TaskManager.Controllers
{
    public class TasksController : Controller
    {
        private readonly ApplicationDbContext _context;
        
        public TasksController(ApplicationDbContext context)
        {
            _context = context;
        }
        
        // GET: Tasks
        public async Task<IActionResult> Index(string searchString, string filterStatus)
        {
            var tasks = _context.Tasks.AsQueryable();
            
            // Search filter
            if (!string.IsNullOrEmpty(searchString))
            {
                tasks = tasks.Where(t => t.Title.Contains(searchString) || 
                                        (t.Description != null && t.Description.Contains(searchString)));
            }
            
            // Status filter
            if (!string.IsNullOrEmpty(filterStatus))
            {
                tasks = filterStatus switch
                {
                    "completed" => tasks.Where(t => t.IsCompleted),
                    "active" => tasks.Where(t => !t.IsCompleted),
                    "overdue" => tasks.Where(t => t.DueDate.HasValue && t.DueDate.Value < DateTime.Now && !t.IsCompleted),
                    _ => tasks
                };
            }
            
            var taskList = await tasks.OrderByDescending(t => t.CreatedAt).ToListAsync();
            
            ViewData["SearchString"] = searchString;
            ViewData["FilterStatus"] = filterStatus;
            
            return View(taskList);
        }
        
        // GET: Tasks/Details/5
        public async Task<IActionResult> Details(int? id)
        {
            if (id == null)
            {
                return NotFound();
            }
            
            var task = await _context.Tasks.FindAsync(id);
            if (task == null)
            {
                return NotFound();
            }
            
            return View(task);
        }
        
        // GET: Tasks/Create
        public IActionResult Create()
        {
            return View();
        }
        
        // POST: Tasks/Create
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<IActionResult> Create([Bind("Title,Description,DueDate,Priority")] TaskItem task)
        {
            if (ModelState.IsValid)
            {
                task.CreatedAt = DateTime.Now;
                _context.Add(task);
                await _context.SaveChangesAsync();
                TempData["Success"] = "Task created successfully!";
                return RedirectToAction(nameof(Index));
            }
            return View(task);
        }
        
        // GET: Tasks/Edit/5
        public async Task<IActionResult> Edit(int? id)
        {
            if (id == null)
            {
                return NotFound();
            }
            
            var task = await _context.Tasks.FindAsync(id);
            if (task == null)
            {
                return NotFound();
            }
            return View(task);
        }
        
        // POST: Tasks/Edit/5
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<IActionResult> Edit(int id, [Bind("Id,Title,Description,DueDate,Priority,IsCompleted,CreatedAt")] TaskItem task)
        {
            if (id != task.Id)
            {
                return NotFound();
            }
            
            if (ModelState.IsValid)
            {
                try
                {
                    _context.Update(task);
                    await _context.SaveChangesAsync();
                    TempData["Success"] = "Task updated successfully!";
                }
                catch (DbUpdateConcurrencyException)
                {
                    if (!TaskExists(task.Id))
                    {
                        return NotFound();
                    }
                    else
                    {
                        throw;
                    }
                }
                return RedirectToAction(nameof(Index));
            }
            return View(task);
        }
        
        // GET: Tasks/Delete/5
        public async Task<IActionResult> Delete(int? id)
        {
            if (id == null)
            {
                return NotFound();
            }
            
            var task = await _context.Tasks.FindAsync(id);
            if (task == null)
            {
                return NotFound();
            }
            
            return View(task);
        }
        
        // POST: Tasks/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public async Task<IActionResult> DeleteConfirmed(int id)
        {
            var task = await _context.Tasks.FindAsync(id);
            if (task != null)
            {
                _context.Tasks.Remove(task);
                await _context.SaveChangesAsync();
                TempData["Success"] = "Task deleted successfully!";
            }
            
            return RedirectToAction(nameof(Index));
        }
        
        // POST: Tasks/ToggleComplete/5
        [HttpPost]
        [ValidateAntiForgeryToken]
        public async Task<IActionResult> ToggleComplete(int id)
        {
            var task = await _context.Tasks.FindAsync(id);
            if (task == null)
            {
                return NotFound();
            }
            
            task.IsCompleted = !task.IsCompleted;
            await _context.SaveChangesAsync();
            
            return RedirectToAction(nameof(Index));
        }
        
        private bool TaskExists(int id)
        {
            return _context.Tasks.Any(e => e.Id == id);
        }
    }
}
```

### Step 9: Create Views

Create `Views/Tasks/Index.cshtml`:

```html
@model IEnumerable<TaskManager.Models.TaskItem>

@{
    ViewData["Title"] = "Task Manager";
}

<div class="container mt-4">
    <div class="row mb-4">
        <div class="col-md-6">
            <h1><i class="bi bi-list-check"></i> My Tasks</h1>
        </div>
        <div class="col-md-6 text-end">
            <a asp-action="Create" class="btn btn-primary">
                <i class="bi bi-plus-circle"></i> New Task
            </a>
        </div>
    </div>
    
    @if (TempData["Success"] != null)
    {
        <div class="alert alert-success alert-dismissible fade show" role="alert">
            @TempData["Success"]
            <button type="button" class="btn-close" data-bs-dismiss="alert"></button>
        </div>
    }
    
    <!-- Search and Filter -->
    <div class="card mb-4">
        <div class="card-body">
            <form asp-action="Index" method="get" class="row g-3">
                <div class="col-md-6">
                    <input type="text" name="searchString" value="@ViewData["SearchString"]" 
                           class="form-control" placeholder="Search tasks..." />
                </div>
                <div class="col-md-4">
                    <select name="filterStatus" class="form-select">
                        <option value="">All Tasks</option>
                        <option value="active" selected="@(ViewData["FilterStatus"]?.ToString() == "active")">Active</option>
                        <option value="completed" selected="@(ViewData["FilterStatus"]?.ToString() == "completed")">Completed</option>
                        <option value="overdue" selected="@(ViewData["FilterStatus"]?.ToString() == "overdue")">Overdue</option>
                    </select>
                </div>
                <div class="col-md-2">
                    <button type="submit" class="btn btn-primary w-100">Filter</button>
                </div>
            </form>
        </div>
    </div>
    
    <!-- Tasks List -->
    @if (Model.Any())
    {
        <div class="row">
            @foreach (var task in Model)
            {
                <div class="col-md-6 mb-3">
                    <div class="card @(task.IsCompleted ? "border-success" : "") @(task.IsOverdue ? "border-danger" : "")">
                        <div class="card-body">
                            <h5 class="card-title">
                                @if (task.IsCompleted)
                                {
                                    <del>@task.Title</del>
                                }
                                else
                                {
                                    @task.Title
                                }
                                <span class="@task.PriorityBadgeClass float-end">@task.Priority</span>
                            </h5>
                            
                            @if (!string.IsNullOrEmpty(task.Description))
                            {
                                <p class="card-text">@task.Description</p>
                            }
                            
                            @if (task.DueDate.HasValue)
                            {
                                <p class="text-muted">
                                    <i class="bi bi-calendar"></i> 
                                    Due: @task.DueDate.Value.ToString("MMM dd, yyyy")
                                    @if (task.IsOverdue)
                                    {
                                        <span class="badge bg-danger">Overdue</span>
                                    }
                                </p>
                            }
                            
                            <div class="btn-group" role="group">
                                <form asp-action="ToggleComplete" asp-route-id="@task.Id" method="post" class="d-inline">
                                    <button type="submit" class="btn btn-sm @(task.IsCompleted ? "btn-warning" : "btn-success")">
                                        @if (task.IsCompleted)
                                        {
                                            <i class="bi bi-arrow-counterclockwise"></i> Reopen
                                        }
                                        else
                                        {
                                            <i class="bi bi-check"></i> Complete
                                        }
                                    </button>
                                </form>
                                <a asp-action="Edit" asp-route-id="@task.Id" class="btn btn-sm btn-primary">
                                    <i class="bi bi-pencil"></i> Edit
                                </a>
                                <a asp-action="Details" asp-route-id="@task.Id" class="btn btn-sm btn-info">
                                    <i class="bi bi-eye"></i> Details
                                </a>
                                <a asp-action="Delete" asp-route-id="@task.Id" class="btn btn-sm btn-danger">
                                    <i class="bi bi-trash"></i> Delete
                                </a>
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    }
    else
    {
        <div class="alert alert-info">
            <h4>No tasks found</h4>
            <p>Create your first task to get started!</p>
        </div>
    }
</div>
```

Create `Views/Tasks/Create.cshtml`:

```html
@model TaskManager.Models.TaskItem

@{
    ViewData["Title"] = "Create Task";
}

<div class="container mt-4">
    <h1>Create New Task</h1>
    
    <div class="row">
        <div class="col-md-8">
            <form asp-action="Create" method="post">
                <div asp-validation-summary="ModelOnly" class="text-danger"></div>
                
                <div class="mb-3">
                    <label asp-for="Title" class="form-label"></label>
                    <input asp-for="Title" class="form-control" />
                    <span asp-validation-for="Title" class="text-danger"></span>
                </div>
                
                <div class="mb-3">
                    <label asp-for="Description" class="form-label"></label>
                    <textarea asp-for="Description" class="form-control" rows="3"></textarea>
                    <span asp-validation-for="Description" class="text-danger"></span>
                </div>
                
                <div class="mb-3">
                    <label asp-for="DueDate" class="form-label"></label>
                    <input asp-for="DueDate" class="form-control" type="date" />
                    <span asp-validation-for="DueDate" class="text-danger"></span>
                </div>
                
                <div class="mb-3">
                    <label asp-for="Priority" class="form-label"></label>
                    <select asp-for="Priority" class="form-select" asp-items="Html.GetEnumSelectList<TaskManager.Models.Priority>()"></select>
                    <span asp-validation-for="Priority" class="text-danger"></span>
                </div>
                
                <div class="mb-3">
                    <button type="submit" class="btn btn-primary">Create Task</button>
                    <a asp-action="Index" class="btn btn-secondary">Cancel</a>
                </div>
            </form>
        </div>
    </div>
</div>

@section Scripts {
    @{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
}
```

Create similar views for `Edit.cshtml`, `Details.cshtml`, and `Delete.cshtml` (adapt the Create view).

### Step 10: Update Layout

Update `Views/Shared/_Layout.cshtml` to add Bootstrap Icons:

```html
<!-- In the <head> section, add: -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.10.0/font/bootstrap-icons.css">
```

### Step 11: Run and Test

```bash
dotnet run
```

Visit `http://localhost:5000` and test:
- Create tasks
- View task list
- Edit tasks
- Delete tasks
- Mark tasks as complete
- Search and filter

### Step 12: Commit to GitHub

```bash
cd ..  # Go to repo root
git add .
git commit -m "Add complete Task Manager web application with CRUD operations"
git push origin main
```

## Key Concepts Explained

### Entity Framework Core

**DbContext**: Manages database connections and operations
**DbSet**: Represents a table
**Migrations**: Version control for database schema

```bash
# Common EF Core commands
dotnet ef migrations add MigrationName
dotnet ef database update
dotnet ef migrations remove
dotnet ef database drop
```

### Model Binding

Automatically maps form data to model properties:

```csharp
[HttpPost]
public IActionResult Create(TaskItem task)
{
    // task object is automatically populated from form data
}
```

### Data Validation

Use data annotations:

```csharp
[Required]
[StringLength(100)]
[Range(1, 100)]
[EmailAddress]
[Display(Name = "Display Name")]
```

### TempData

Pass data between requests (survives redirects):

```csharp
TempData["Success"] = "Operation successful!";
return RedirectToAction("Index");
```

## Best Practices

✅ Use async/await for database operations
✅ Validate user input
✅ Use strongly-typed models
✅ Implement proper error handling
✅ Use TempData for flash messages
✅ Follow RESTful conventions
✅ Use Bootstrap for responsive design

## Challenge Tasks

### 1. Add Categories
- Create Category model
- Assign tasks to categories
- Filter by category

### 2. Add User Authentication
- Use ASP.NET Core Identity
- User-specific tasks
- Login/Register pages

### 3. Add File Attachments
- Upload files for tasks
- Store in wwwroot
- Download attachments

### 4. Add Comments
- Comment model
- Add comments to tasks
- Display comment history

### 5. Add Dashboard
- Task statistics
- Charts (use Chart.js)
- Completion rates
- Upcoming deadlines

## Expected Output
- Full CRUD web application
- Database integration
- Form validation
- Professional UI with Bootstrap

## Next Steps
You've completed all exercises! Continue learning:
- ASP.NET Core Identity (authentication)
- Web APIs
- Blazor (for interactive UIs)
- Deployment to Azure/AWS

## Resources
- [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/)
- [ASP.NET Core MVC](https://docs.microsoft.com/en-us/aspnet/core/mvc/)
- [Bootstrap Documentation](https://getbootstrap.com/)
- [Data Validation](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/validation)
