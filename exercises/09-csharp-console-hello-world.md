# Exercise 9: C# Console App - Hello World

## Overview
Learn the basics of C# programming by creating your first console application. C# is a powerful, modern programming language used for everything from games to web applications.

## Prerequisites
- .NET SDK installed (download from [dotnet.microsoft.com](https://dotnet.microsoft.com/download))
- A code editor (Visual Studio, VS Code, or Rider)
- Basic programming concepts
- GitHub account

## Learning Objectives
- Understand C# basics and syntax
- Create a console application with .NET
- Learn about variables, data types, and methods
- Work with user input and output
- Use the .NET CLI
- Set up a C# project in Git

## What is C#?

C# (pronounced "C Sharp") is a modern, object-oriented programming language created by Microsoft. It's used for:
- Desktop applications
- Web applications (ASP.NET)
- Mobile apps (Xamarin)
- Game development (Unity)
- Cloud services

## Instructions

### Step 1: Verify .NET Installation

Check if .NET is installed:
```bash
dotnet --version
```

If not installed, download the .NET SDK from Microsoft's website.

List available templates:
```bash
dotnet new list
```

### Step 2: Create a New Repository

```bash
# Create repo on GitHub: csharp-hello-world
git clone https://github.com/YOUR-USERNAME/csharp-hello-world.git
cd csharp-hello-world
```

### Step 3: Create Your First Console App

```bash
# Create a new console application
dotnet new console -n HelloWorld
cd HelloWorld
```

This creates:
```
HelloWorld/
  ├── Program.cs          # Main program file
  ├── HelloWorld.csproj   # Project file
  └── obj/                # Build artifacts
```

### Step 4: Examine the Default Code

Open `Program.cs`:
```csharp
// See https://aka.ms/new-console-template for more information
Console.WriteLine("Hello, World!");
```

This is the new "top-level statements" syntax in C# 9+.

### Step 5: Run Your Program

```bash
dotnet run
```

You should see:
```
Hello, World!
```

### Step 6: Expand Your Program

Replace the contents of `Program.cs`:
```csharp
// Hello World Console Application
Console.WriteLine("=================================");
Console.WriteLine("    Welcome to C# Programming    ");
Console.WriteLine("=================================");
Console.WriteLine();

// Get user's name
Console.Write("What is your name? ");
string? name = Console.ReadLine();

// Greet the user
Console.WriteLine($"\nHello, {name}! Nice to meet you!");

// Show some info
Console.WriteLine("\nHere are some C# facts:");
Console.WriteLine("- C# was created by Microsoft");
Console.WriteLine("- First released in 2000");
Console.WriteLine("- Current version: C# 12");
Console.WriteLine("- Used by millions of developers");

Console.WriteLine("\nPress any key to exit...");
Console.ReadKey();
```

Run it:
```bash
dotnet run
```

### Step 7: Understanding the Code

**Key Concepts:**

```csharp
// Console output
Console.WriteLine("Text");     // Prints with newline
Console.Write("Text");          // Prints without newline

// Console input
string? input = Console.ReadLine();  // Read line from user

// Variables
string name = "John";           // String variable
int age = 25;                   // Integer variable

// String interpolation
Console.WriteLine($"Name: {name}, Age: {age}");

// Comments
// Single line comment
/* Multi-line
   comment */
```

### Step 8: Add More Features

Create a more interactive program:

```csharp
// Interactive Hello World Application
using System;

Console.Clear();  // Clear console
Console.ForegroundColor = ConsoleColor.Cyan;
Console.WriteLine("╔════════════════════════════════╗");
Console.WriteLine("║   C# Console Application       ║");
Console.WriteLine("╔════════════════════════════════╗");
Console.ResetColor();
Console.WriteLine();

// Get user information
Console.Write("Enter your name: ");
string? name = Console.ReadLine();

Console.Write("Enter your age: ");
string? ageInput = Console.ReadLine();
int age = Convert.ToInt32(ageInput);

Console.Write("Enter your favorite color: ");
string? color = Console.ReadLine();

// Display information
Console.WriteLine("\n" + new string('-', 40));
Console.WriteLine("Your Information:");
Console.WriteLine(new string('-', 40));
Console.WriteLine($"Name: {name}");
Console.WriteLine($"Age: {age}");
Console.WriteLine($"Favorite Color: {color}");

// Calculate birth year (approximate)
int currentYear = DateTime.Now.Year;
int birthYear = currentYear - age;
Console.WriteLine($"Birth Year (approx): {birthYear}");

// Categorize by age
Console.WriteLine("\nAge Category:");
if (age < 18)
{
    Console.WriteLine("You are a minor.");
}
else if (age < 65)
{
    Console.WriteLine("You are an adult.");
}
else
{
    Console.WriteLine("You are a senior.");
}

Console.WriteLine("\n" + new string('-', 40));
Console.WriteLine("\nPress any key to exit...");
Console.ReadKey();
```

### Step 9: Add a .gitignore File

Create `.gitignore` in the repository root:
```gitignore
# Build results
[Dd]ebug/
[Rr]elease/
[Bb]in/
[Oo]bj/

# Visual Studio
.vs/
*.user
*.suo
*.userosscache

# User-specific files
*.userprefs

# Build artifacts
*.dll
*.exe
*.pdb

# NuGet
*.nupkg
packages/

# OS files
.DS_Store
Thumbs.db
```

### Step 10: Commit to GitHub

```bash
cd ..  # Go back to repo root
git add .
git commit -m "Add C# Hello World console application"
git push origin main
```

## C# Basics Reference

### Data Types

```csharp
// Numeric types
int wholeNumber = 42;
long bigNumber = 1234567890L;
double decimalNumber = 3.14;
float smallDecimal = 3.14f;
decimal money = 99.99m;

// Text
char letter = 'A';
string text = "Hello";

// Boolean
bool isTrue = true;
bool isFalse = false;

// Date and time
DateTime now = DateTime.Now;
```

### Variables and Constants

```csharp
// Variable (can change)
string name = "John";
name = "Jane";  // OK

// Constant (cannot change)
const double PI = 3.14159;
// PI = 3.14;  // Error!

// Read-only (assigned once)
readonly int MaxValue = 100;
```

### String Operations

```csharp
string firstName = "John";
string lastName = "Doe";

// Concatenation
string fullName = firstName + " " + lastName;

// Interpolation (preferred)
string greeting = $"Hello, {firstName} {lastName}!";

// String methods
string upper = text.ToUpper();
string lower = text.ToLower();
int length = text.Length;
bool contains = text.Contains("John");
string replaced = text.Replace("John", "Jane");
```

### Control Flow

```csharp
// If-else
if (age >= 18)
{
    Console.WriteLine("Adult");
}
else
{
    Console.WriteLine("Minor");
}

// Switch
switch (day)
{
    case "Monday":
        Console.WriteLine("Start of week");
        break;
    case "Friday":
        Console.WriteLine("Almost weekend!");
        break;
    default:
        Console.WriteLine("Regular day");
        break;
}

// Loops
for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);
}

while (condition)
{
    // Code
}

do
{
    // Code
} while (condition);
```

### Methods

```csharp
// Method with return value
int Add(int a, int b)
{
    return a + b;
}

// Method without return value
void Greet(string name)
{
    Console.WriteLine($"Hello, {name}!");
}

// Method with default parameter
void PrintMessage(string message = "Default message")
{
    Console.WriteLine(message);
}

// Using methods
int sum = Add(5, 3);
Greet("Alice");
PrintMessage();  // Uses default
PrintMessage("Custom message");
```

## Practice Exercise: Enhanced Calculator

Create a calculator console app:

```csharp
using System;

Console.WriteLine("=== Simple Calculator ===\n");

bool continueCalculation = true;

while (continueCalculation)
{
    Console.Write("Enter first number: ");
    double num1 = Convert.ToDouble(Console.ReadLine());
    
    Console.Write("Enter operator (+, -, *, /): ");
    string? op = Console.ReadLine();
    
    Console.Write("Enter second number: ");
    double num2 = Convert.ToDouble(Console.ReadLine());
    
    double result = 0;
    bool validOperation = true;
    
    switch (op)
    {
        case "+":
            result = num1 + num2;
            break;
        case "-":
            result = num1 - num2;
            break;
        case "*":
            result = num1 * num2;
            break;
        case "/":
            if (num2 != 0)
            {
                result = num1 / num2;
            }
            else
            {
                Console.WriteLine("Error: Division by zero!");
                validOperation = false;
            }
            break;
        default:
            Console.WriteLine("Invalid operator!");
            validOperation = false;
            break;
    }
    
    if (validOperation)
    {
        Console.WriteLine($"\nResult: {num1} {op} {num2} = {result}");
    }
    
    Console.Write("\nContinue? (y/n): ");
    string? response = Console.ReadLine();
    continueCalculation = response?.ToLower() == "y";
    Console.WriteLine();
}

Console.WriteLine("Goodbye!");
```

## Challenge Tasks

### 1. Number Guessing Game
```csharp
// Create a game where computer picks random number
// User has to guess it
// Provide "higher" or "lower" hints
```

### 2. To-Do List
```csharp
// Create a console-based to-do list
// Add, view, mark complete, delete tasks
// Save to file (advanced)
```

### 3. Temperature Converter
```csharp
// Convert between Celsius, Fahrenheit, Kelvin
// Show conversion formulas
// Handle invalid input
```

### 4. Grade Calculator
```csharp
// Input multiple test scores
// Calculate average
// Assign letter grade
// Show statistics (min, max, average)
```

### 5. Simple Banking System
```csharp
// Track account balance
// Deposit, withdraw, check balance
// Transaction history
// Input validation
```

## Common Mistakes

❌ **Forgetting semicolons**
```csharp
Console.WriteLine("Hello")  // Error: missing ;
```

❌ **Case sensitivity**
```csharp
console.writeline("Hello");  // Error: should be Console.WriteLine
```

❌ **Not handling null input**
```csharp
string name = Console.ReadLine();  // Could be null
// Use string? or null checking
```

❌ **Wrong conversion**
```csharp
int age = Console.ReadLine();  // Error: ReadLine returns string
// Use: int age = Convert.ToInt32(Console.ReadLine());
```

## Best Practices

✅ Use meaningful variable names
✅ Add comments for complex logic
✅ Handle user input errors
✅ Use proper formatting and indentation
✅ Follow C# naming conventions:
  - PascalCase for methods: `CalculateSum()`
  - camelCase for variables: `firstName`
  - ALL_CAPS for constants: `MAX_SIZE`

## .NET CLI Commands

```bash
# Create new project
dotnet new console -n ProjectName

# Run project
dotnet run

# Build project
dotnet build

# Clean build artifacts
dotnet clean

# Add NuGet package
dotnet add package PackageName

# Run tests
dotnet test

# Publish for deployment
dotnet publish
```

## Expected Output
- Working C# console application
- Understanding of C# basics
- Ability to handle user input
- Knowledge of .NET CLI

## Troubleshooting

**Problem**: `dotnet: command not found`
**Solution**: Install .NET SDK from Microsoft

**Problem**: Build errors
**Solution**: Check syntax, ensure all statements end with ;

**Problem**: Console window closes immediately
**Solution**: Add `Console.ReadKey()` at the end

## Next Steps
Continue to [Exercise 10: C# Console App - Interactive Program](10-csharp-interactive-app.md) for more advanced C# concepts!

## Resources
- [Microsoft C# Documentation](https://docs.microsoft.com/en-us/dotnet/csharp/)
- [C# Tutorial](https://www.w3schools.com/cs/)
- [.NET Documentation](https://docs.microsoft.com/en-us/dotnet/)
- [C# Programming Guide](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/)
