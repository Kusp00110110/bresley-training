# Exercise 10: C# Console App - Interactive Program with OOP

## Overview
Build more advanced C# console applications using Object-Oriented Programming (OOP) principles, collections, and file handling.

## Prerequisites
- Completed Exercise 9: C# Hello World
- .NET SDK installed
- Basic understanding of C# syntax
- Code editor

## Learning Objectives
- Understand Object-Oriented Programming concepts
- Work with classes and objects
- Use collections (List, Dictionary)
- Handle exceptions properly
- Read and write files
- Create a complete application

## Instructions

### Step 1: Create a New Project

```bash
# Create repo on GitHub: csharp-contact-manager
git clone https://github.com/YOUR-USERNAME/csharp-contact-manager.git
cd csharp-contact-manager

# Create console application
dotnet new console -n ContactManager
cd ContactManager
```

### Step 2: Create a Contact Class

Create a new file `Contact.cs`:

```csharp
namespace ContactManager;

public class Contact
{
    // Properties
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
    public string PhoneNumber { get; set; }
    
    // Constructor
    public Contact(string firstName, string lastName, string email, string phoneNumber)
    {
        FirstName = firstName;
        LastName = lastName;
        Email = email;
        PhoneNumber = phoneNumber;
    }
    
    // Method to display contact information
    public void Display()
    {
        Console.WriteLine($"\nName: {FirstName} {LastName}");
        Console.WriteLine($"Email: {Email}");
        Console.WriteLine($"Phone: {PhoneNumber}");
    }
    
    // Override ToString for easy display
    public override string ToString()
    {
        return $"{FirstName},{LastName},{Email},{PhoneNumber}";
    }
    
    // Create contact from CSV string
    public static Contact FromString(string csv)
    {
        string[] parts = csv.Split(',');
        return new Contact(parts[0], parts[1], parts[2], parts[3]);
    }
}
```

### Step 3: Create a ContactManager Class

Create `ContactManager.cs`:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

namespace ContactManager;

public class ContactManager
{
    private List<Contact> contacts;
    private const string FileName = "contacts.csv";
    
    public ContactManager()
    {
        contacts = new List<Contact>();
        LoadContacts();
    }
    
    // Add a new contact
    public void AddContact(Contact contact)
    {
        contacts.Add(contact);
        Console.WriteLine($"\n✓ Contact '{contact.FirstName} {contact.LastName}' added successfully!");
        SaveContacts();
    }
    
    // View all contacts
    public void ViewAllContacts()
    {
        if (contacts.Count == 0)
        {
            Console.WriteLine("\nNo contacts found.");
            return;
        }
        
        Console.WriteLine("\n" + new string('=', 50));
        Console.WriteLine($"Total Contacts: {contacts.Count}");
        Console.WriteLine(new string('=', 50));
        
        for (int i = 0; i < contacts.Count; i++)
        {
            Console.WriteLine($"\n[{i + 1}]");
            contacts[i].Display();
        }
        
        Console.WriteLine("\n" + new string('=', 50));
    }
    
    // Search contacts by name
    public void SearchContacts(string searchTerm)
    {
        var results = contacts.Where(c => 
            c.FirstName.Contains(searchTerm, StringComparison.OrdinalIgnoreCase) ||
            c.LastName.Contains(searchTerm, StringComparison.OrdinalIgnoreCase)
        ).ToList();
        
        if (results.Count == 0)
        {
            Console.WriteLine($"\nNo contacts found matching '{searchTerm}'");
            return;
        }
        
        Console.WriteLine($"\nFound {results.Count} contact(s):");
        foreach (var contact in results)
        {
            contact.Display();
        }
    }
    
    // Delete a contact
    public void DeleteContact(int index)
    {
        if (index < 0 || index >= contacts.Count)
        {
            Console.WriteLine("\n✗ Invalid contact number!");
            return;
        }
        
        var contact = contacts[index];
        contacts.RemoveAt(index);
        Console.WriteLine($"\n✓ Contact '{contact.FirstName} {contact.LastName}' deleted.");
        SaveContacts();
    }
    
    // Save contacts to file
    private void SaveContacts()
    {
        try
        {
            using (StreamWriter writer = new StreamWriter(FileName))
            {
                foreach (var contact in contacts)
                {
                    writer.WriteLine(contact.ToString());
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"\n✗ Error saving contacts: {ex.Message}");
        }
    }
    
    // Load contacts from file
    private void LoadContacts()
    {
        try
        {
            if (File.Exists(FileName))
            {
                string[] lines = File.ReadAllLines(FileName);
                foreach (string line in lines)
                {
                    if (!string.IsNullOrWhiteSpace(line))
                    {
                        contacts.Add(Contact.FromString(line));
                    }
                }
                Console.WriteLine($"Loaded {contacts.Count} contact(s).");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"✗ Error loading contacts: {ex.Message}");
        }
    }
    
    // Get number of contacts
    public int Count => contacts.Count;
}
```

### Step 4: Create the Main Program

Update `Program.cs`:

```csharp
using ContactManager;
using System;

Console.Clear();
Console.ForegroundColor = ConsoleColor.Cyan;
Console.WriteLine("╔════════════════════════════════════════╗");
Console.WriteLine("║     Contact Manager Application        ║");
Console.WriteLine("╚════════════════════════════════════════╝");
Console.ResetColor();

var manager = new ContactManager.ContactManager();

bool running = true;

while (running)
{
    ShowMenu();
    string? choice = Console.ReadLine();
    
    Console.WriteLine();
    
    switch (choice)
    {
        case "1":
            AddNewContact(manager);
            break;
        case "2":
            manager.ViewAllContacts();
            break;
        case "3":
            SearchContact(manager);
            break;
        case "4":
            DeleteContact(manager);
            break;
        case "5":
            running = false;
            Console.WriteLine("Goodbye!");
            break;
        default:
            Console.WriteLine("✗ Invalid choice. Please try again.");
            break;
    }
    
    if (running)
    {
        Console.WriteLine("\nPress any key to continue...");
        Console.ReadKey();
        Console.Clear();
    }
}

// Helper methods
void ShowMenu()
{
    Console.WriteLine("\n" + new string('─', 40));
    Console.WriteLine("Menu:");
    Console.WriteLine(new string('─', 40));
    Console.WriteLine("1. Add Contact");
    Console.WriteLine("2. View All Contacts");
    Console.WriteLine("3. Search Contacts");
    Console.WriteLine("4. Delete Contact");
    Console.WriteLine("5. Exit");
    Console.WriteLine(new string('─', 40));
    Console.Write("Enter your choice (1-5): ");
}

void AddNewContact(ContactManager.ContactManager manager)
{
    Console.Write("First Name: ");
    string? firstName = Console.ReadLine();
    
    Console.Write("Last Name: ");
    string? lastName = Console.ReadLine();
    
    Console.Write("Email: ");
    string? email = Console.ReadLine();
    
    Console.Write("Phone Number: ");
    string? phone = Console.ReadLine();
    
    if (string.IsNullOrWhiteSpace(firstName) || string.IsNullOrWhiteSpace(lastName))
    {
        Console.WriteLine("\n✗ First name and last name are required!");
        return;
    }
    
    var contact = new Contact(firstName, lastName, email ?? "", phone ?? "");
    manager.AddContact(contact);
}

void SearchContact(ContactManager.ContactManager manager)
{
    Console.Write("Enter search term: ");
    string? searchTerm = Console.ReadLine();
    
    if (!string.IsNullOrWhiteSpace(searchTerm))
    {
        manager.SearchContacts(searchTerm);
    }
}

void DeleteContact(ContactManager.ContactManager manager)
{
    manager.ViewAllContacts();
    
    if (manager.Count == 0) return;
    
    Console.Write("\nEnter contact number to delete: ");
    string? input = Console.ReadLine();
    
    if (int.TryParse(input, out int number))
    {
        manager.DeleteContact(number - 1);
    }
    else
    {
        Console.WriteLine("✗ Invalid number!");
    }
}
```

### Step 5: Run and Test

```bash
dotnet run
```

Test all features:
1. Add several contacts
2. View all contacts
3. Search for a contact
4. Delete a contact
5. Exit and restart (data should persist)

### Step 6: Add .gitignore and Commit

```bash
cd ..  # Go to repo root

# Create .gitignore
cat > .gitignore << 'EOF'
bin/
obj/
*.user
.vs/
.vscode/
*.suo
*.dll
*.exe
*.pdb
contacts.csv
EOF

git add .
git commit -m "Add Contact Manager console application with OOP"
git push origin main
```

## OOP Concepts Explained

### Classes and Objects

```csharp
// Class = Blueprint
public class Car
{
    // Properties
    public string Brand { get; set; }
    public string Model { get; set; }
    public int Year { get; set; }
    
    // Method
    public void Start()
    {
        Console.WriteLine($"{Brand} {Model} is starting...");
    }
}

// Object = Instance of the class
Car myCar = new Car();
myCar.Brand = "Toyota";
myCar.Model = "Camry";
myCar.Year = 2023;
myCar.Start();
```

### Encapsulation

```csharp
public class BankAccount
{
    // Private field
    private decimal balance;
    
    // Public property with validation
    public decimal Balance
    {
        get { return balance; }
        private set { balance = value; }  // Only settable within class
    }
    
    // Public methods to interact with private data
    public void Deposit(decimal amount)
    {
        if (amount > 0)
        {
            balance += amount;
        }
    }
    
    public bool Withdraw(decimal amount)
    {
        if (amount > 0 && amount <= balance)
        {
            balance -= amount;
            return true;
        }
        return false;
    }
}
```

### Inheritance

```csharp
// Base class
public class Animal
{
    public string Name { get; set; }
    
    public virtual void MakeSound()
    {
        Console.WriteLine("Some sound");
    }
}

// Derived class
public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof!");
    }
}

// Usage
Animal myDog = new Dog { Name = "Rex" };
myDog.MakeSound();  // Output: Woof!
```

### Polymorphism

```csharp
public abstract class Shape
{
    public abstract double GetArea();
}

public class Circle : Shape
{
    public double Radius { get; set; }
    
    public override double GetArea()
    {
        return Math.PI * Radius * Radius;
    }
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    
    public override double GetArea()
    {
        return Width * Height;
    }
}

// Usage
List<Shape> shapes = new List<Shape>
{
    new Circle { Radius = 5 },
    new Rectangle { Width = 4, Height = 6 }
};

foreach (var shape in shapes)
{
    Console.WriteLine($"Area: {shape.GetArea()}");
}
```

## Collections in C#

### List<T>

```csharp
// Create list
List<string> names = new List<string>();

// Add items
names.Add("Alice");
names.Add("Bob");
names.AddRange(new[] { "Charlie", "David" });

// Access items
string first = names[0];
string last = names[names.Count - 1];

// Iterate
foreach (string name in names)
{
    Console.WriteLine(name);
}

// LINQ operations
var filtered = names.Where(n => n.StartsWith("A")).ToList();
var sorted = names.OrderBy(n => n).ToList();
```

### Dictionary<TKey, TValue>

```csharp
// Create dictionary
Dictionary<string, int> ages = new Dictionary<string, int>();

// Add items
ages.Add("Alice", 25);
ages["Bob"] = 30;

// Access items
int aliceAge = ages["Alice"];

// Check if key exists
if (ages.ContainsKey("Charlie"))
{
    Console.WriteLine(ages["Charlie"]);
}

// Iterate
foreach (var pair in ages)
{
    Console.WriteLine($"{pair.Key}: {pair.Value}");
}
```

## Exception Handling

```csharp
try
{
    // Code that might throw an exception
    int result = 10 / 0;
}
catch (DivideByZeroException ex)
{
    Console.WriteLine($"Error: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
}
finally
{
    // Always executes
    Console.WriteLine("Cleanup code here");
}
```

## File Operations

```csharp
// Write to file
File.WriteAllText("data.txt", "Hello, World!");
File.WriteAllLines("lines.txt", new[] { "Line 1", "Line 2" });

// Append to file
File.AppendAllText("data.txt", "\nNew line");

// Read from file
string content = File.ReadAllText("data.txt");
string[] lines = File.ReadAllLines("lines.txt");

// Check if file exists
if (File.Exists("data.txt"))
{
    // File operations
}

// Using statement for automatic resource cleanup
using (StreamWriter writer = new StreamWriter("output.txt"))
{
    writer.WriteLine("Line 1");
    writer.WriteLine("Line 2");
}
```

## Challenge Tasks

### 1. Library Management System
Create a system to manage books:
- Book class (Title, Author, ISBN, Available)
- Add, remove, search books
- Check out and return books
- View available books

### 2. Student Grade Manager
Track student grades:
- Student class with List of grades
- Calculate average, highest, lowest
- Assign letter grades
- Generate report cards

### 3. Bank Account System
Create a banking system:
- Account class with balance
- Deposit and withdraw methods
- Transaction history
- Multiple account types (Checking, Savings)

### 4. Inventory System
Manage product inventory:
- Product class (Name, Price, Quantity)
- Add/remove stock
- Low stock alerts
- Generate reports

### 5. Task Manager
Enhanced to-do application:
- Task class with priority and due date
- Categories
- Mark complete
- Filter by status/priority
- Sort by due date

## Best Practices

✅ **Single Responsibility**: Each class should have one job
✅ **Meaningful Names**: Use descriptive names for classes and methods
✅ **Error Handling**: Use try-catch for risky operations
✅ **Validation**: Check user input before processing
✅ **Documentation**: Add XML comments to public methods
✅ **SOLID Principles**: Follow object-oriented design principles

## Expected Output
- Working console application with OOP
- Understanding of classes and objects
- Ability to work with collections
- Knowledge of file handling

## Next Steps
Continue to [Exercise 11: ASP.NET Core MVC - Basic Setup](11-aspnet-mvc-setup.md) to build web applications!

## Resources
- [C# OOP Tutorial](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/tutorials/oop)
- [Collections in C#](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/collections)
- [File I/O](https://docs.microsoft.com/en-us/dotnet/standard/io/)
- [LINQ](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/)
