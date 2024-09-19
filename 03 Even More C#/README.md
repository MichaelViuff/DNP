# 03 Exercises: Even More C#

## 3.1 Managing a List of Student Names

**Objective:**  
Learn how to create and manipulate a `List` of strings in C#. This exercise focuses on basic list operations such as adding, sorting, and removing elements.

**Task:**  
Create a program that manages a list of student names by performing various operations, including adding names from user input, sorting the list, and removing specific names.

**Steps:**
1. **Create a List:** Create a `List<string>` called `studentNames`.
2. **Add Initial Names:** Add the following student names to the list: `"Alice"`, `"Bob"`, `"Charlie"`, `"Diana"`, `"Eve"`.
3. **Display Names:** Print all the names currently in the list.
4. **Add User Input:** Prompt the user to enter a new name and add it to the list.
5. **Sort the List:** Sort the list alphabetically and display the sorted list.
6. **Remove a Name:** Remove the name `"Bob"` from the list and display the updated list.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Step 1: Create a List of student names
        List<string> studentNames = new List<string> { "Alice", "Bob", "Charlie", "Diana", "Eve" };
        
        // Step 2: Display the original list
        Console.WriteLine("Original List: " + string.Join(", ", studentNames));
        
        // Step 3: Ask the user to input a name and add it to the list
        Console.Write("Enter a name to add: ");
        string newName = Console.ReadLine();
        studentNames.Add(newName);
        
        // Step 4: Sort the list alphabetically and display it
        studentNames.Sort();
        Console.WriteLine("Sorted List: " + string.Join(", ", studentNames));
        
        // Step 5: Remove a specific name ("Bob") and display the updated list
        studentNames.Remove("Bob");
        Console.WriteLine("List after removing 'Bob': " + string.Join(", ", studentNames));
    }
}
```

</p>
</details>
</blockquote>

## 3.2 Counting Word Occurrences Using Dictionary

**Objective:**  
Learn how to use a `Dictionary` in C# to count occurrences of words in a sentence. This exercise focuses on working with key-value pairs to store and retrieve data efficiently.

**Task:**  
Create a program that counts the number of times each word appears in a user-input sentence using a `Dictionary`.

**Steps:**
1. **Input Sentence:** Prompt the user to input a sentence.
2. **Split into Words:** Split the sentence into words and store them in a `List<string>`.
3. **Create Dictionary:** Create a `Dictionary<string, int>` to keep track of word counts.
4. **Count Words:** Iterate over the list of words, updating the count of each word in the dictionary.
5. **Display Counts:** Display each word along with its count.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Step 1: Ask the user to input a sentence
        Console.Write("Enter a sentence: ");
        string input = Console.ReadLine();
        
        // Step 2: Split the sentence into words
        string[] words = input.Split(new char[] { ' ', '.', ',', '!', '?' }, StringSplitOptions.RemoveEmptyEntries);
        
        // Step 3: Create a Dictionary to count word occurrences
        Dictionary<string, int> wordCounts = new Dictionary<string, int>();
        
        // Step 4: Count each word's occurrences
        foreach (string word in words)
        {
            string lowerWord = word.ToLower();
            if (wordCounts.ContainsKey(lowerWord))
            {
                wordCounts[lowerWord]++;
            }
            else
            {
                wordCounts[lowerWord] = 1;
            }
        }
        
        // Step 5: Display each word and its count
        Console.WriteLine("Word counts:");
        foreach (var kvp in wordCounts)
        {
            Console.WriteLine($"{kvp.Key}: {kvp.Value}");
        }
    }
}
```

</p>
</details>
</blockquote>


## 3.3 Filtering a List of Numbers Using the `.Where` Method

**Objective:**  
Learn how to use the `.Where` method from LINQ in C# to filter elements in a list based on a specified condition.

**Task:**  
Create a program that filters a list of numbers, selecting only those that meet certain criteria using the `.Where` method.

**Steps:**
1. **Create a List:** Create a `List<int>` called `numbers` and populate it with the following values: `10, 23, 35, 42, 56, 67, 72, 89, 91, 100`.
2. **Display Original List:** Print the original list of numbers.
3. **Filter with `.Where`:** Use the `.Where` method to filter the list, selecting only even numbers.
4. **Display Filtered List:** Display the list of even numbers.
5. **Filter with Custom Condition:** Use the `.Where` method again to filter numbers greater than 50 and display the result.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        // Step 1: Create a List of integers
        List<int> numbers = new List<int> { 10, 23, 35, 42, 56, 67, 72, 89, 91, 100 };
        
        // Step 2: Display the original list
        Console.WriteLine("Original List: " + string.Join(", ", numbers));
        
        // Step 3: Use the .Where method to filter only even numbers
        var evenNumbers = numbers.Where(n => n % 2 == 0).ToList();
        Console.WriteLine("Even Numbers: " + string.Join(", ", evenNumbers));
        
        // Step 4: Use the .Where method to filter numbers greater than 50
        var numbersGreaterThanFifty = numbers.Where(n => n > 50).ToList();
        Console.WriteLine("Numbers Greater Than 50: " + string.Join(", ", numbersGreaterThanFifty));
    }
}
```

</p>
</details>
</blockquote>


## 3.4 Filtering and Sorting a List of Objects Using LINQ

**Objective:**  
Learn how to use the `.Where` method and other LINQ methods in C# to filter and sort a list of objects based on multiple conditions.

**Task:**  
Create a program that manages a list of products, filtering and sorting them based on price and category using LINQ.

**Steps:**
1. **Define a Product Class:** Create a `Product` class with properties: `Name` (string), `Category` (string), and `Price` (decimal).
2. **Create a List of Products:** Instantiate a `List<Product>` and add the following products:
   - `("Laptop", "Electronics", 1200.99)`
   - `("Phone", "Electronics", 799.49)`
   - `("Tablet", "Electronics", 499.99)`
   - `("Desk", "Furniture", 150.00)`
   - `("Chair", "Furniture", 85.50)`
   - `("Monitor", "Electronics", 299.99)`
3. **Display All Products:** Print all the products in the list.
4. **Filter by Category and Price:** Use the `.Where` method to filter products that are in the `"Electronics"` category and priced above `$500`.
5. **Sort the Filtered List:** Sort the filtered products by price in descending order.
6. **Display Filtered and Sorted List:** Print the filtered and sorted list of products.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Product
{
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }

    public Product(string name, string category, decimal price)
    {
        Name = name;
        Category = category;
        Price = price;
    }
}

class Program
{
    static void Main()
    {
        // Step 2: Create a List of products
        List<Product> products = new List<Product>
        {
            new Product("Laptop", "Electronics", 1200.99m),
            new Product("Phone", "Electronics", 799.49m),
            new Product("Tablet", "Electronics", 499.99m),
            new Product("Desk", "Furniture", 150.00m),
            new Product("Chair", "Furniture", 85.50m),
            new Product("Monitor", "Electronics", 299.99m)
        };

        // Step 3: Display all products
        Console.WriteLine("All Products:");
        foreach (var product in products)
        {
            Console.WriteLine($"{product.Name} - {product.Category} - ${product.Price}");
        }

        // Step 4: Use .Where to filter products in the Electronics category and priced above $500
        var filteredProducts = products
            .Where(p => p.Category == "Electronics" && p.Price > 500)
            .OrderByDescending(p => p.Price)
            .ToList();

        // Step 5: Display the filtered and sorted list
        Console.WriteLine("\nFiltered and Sorted Products (Electronics, Price > $500):");
        foreach (var product in filteredProducts)
        {
            Console.WriteLine($"{product.Name} - {product.Category} - ${product.Price}");
        }
    }
}
```

</p>
</details>
</blockquote>


## 3.5 Finding a Unique Product Using `.SingleOrDefault`

**Objective:**  
Learn how to use the `.SingleOrDefault` method in C# to find a single element in a collection that matches a specific condition. This method is used when you expect exactly one matching element or none.

**Task:**  
Create a program that manages a list of products and uses the `.SingleOrDefault` method to find a unique product based on user input.

**Steps:**
1. **Define a Product Class:** Create a `Product` class with properties: `Name` (string), `Category` (string), and `Price` (decimal).
2. **Create a List of Products:** Instantiate a `List<Product>` and add the following products:
   - `("Laptop", "Electronics", 1200.99)`
   - `("Phone", "Electronics", 799.49)`
   - `("Tablet", "Electronics", 499.99)`
   - `("Desk", "Furniture", 150.00)`
   - `("Chair", "Furniture", 85.50)`
   - `("Monitor", "Electronics", 299.99)`
3. **Display All Products:** Print all the products in the list.
4. **Search for a Product by Name:** Prompt the user to enter a product name and use `.SingleOrDefault` to find the product.
5. **Handle Not Found or Multiple Results:** Display a message if the product is not found or handle any potential errors if the conditions for `.SingleOrDefault` are not met.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Product
{
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }

    public Product(string name, string category, decimal price)
    {
        Name = name;
        Category = category;
        Price = price;
    }
}

class Program
{
    static void Main()
    {
        // Step 2: Create a List of products
        List<Product> products = new List<Product>
        {
            new Product("Laptop", "Electronics", 1200.99m),
            new Product("Phone", "Electronics", 799.49m),
            new Product("Tablet", "Electronics", 499.99m),
            new Product("Desk", "Furniture", 150.00m),
            new Product("Chair", "Furniture", 85.50m),
            new Product("Monitor", "Electronics", 299.99m)
        };

        // Step 3: Display all products
        Console.WriteLine("All Products:");
        foreach (var product in products)
        {
            Console.WriteLine($"{product.Name} - {product.Category} - ${product.Price}");
        }

        // Step 4: Ask the user to enter a product name
        Console.Write("\nEnter the name of the product to find: ");
        string searchName = Console.ReadLine();

        // Step 5: Use .SingleOrDefault to find the product
        var foundProduct = products.SingleOrDefault(p => p.Name.Equals(searchName, StringComparison.OrdinalIgnoreCase));

        // Step 6: Display the result or handle not found
        if (foundProduct != null)
        {
            Console.WriteLine($"Product found: {foundProduct.Name} - {foundProduct.Category} - ${foundProduct.Price}");
        }
        else
        {
            Console.WriteLine("Product not found.");
        }
    }
}
```

</p>
</details>
</blockquote>

## 3.6 Working with JSON in C#

**Objective:**  
Learn how to serialize and deserialize objects to and from JSON format using C#. This exercise focuses on basic JSON operations using the `System.Text.Json` namespace.

**Task:**  
Create a program that serializes a list of products into JSON format and then deserializes the JSON back into a list of objects.

**Steps:**
1. **Define a Product Class:** Create a `Product` class with properties: `Name` (string), `Category` (string), and `Price` (decimal).
2. **Create a List of Products:** Instantiate a `List<Product>` and add the following products:
   - `("Laptop", "Electronics", 1200.99)`
   - `("Phone", "Electronics", 799.49)`
   - `("Tablet", "Electronics", 499.99)`
3. **Serialize to JSON:** Use the `JsonSerializer` class to convert the list of products to a JSON string.
4. **Display JSON:** Print the JSON string to the console.
5. **Deserialize JSON:** Use the `JsonSerializer` class to convert the JSON string back into a list of `Product` objects.
6. **Display Deserialized Products:** Print the deserialized products to verify the data.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;
using System.Text.Json;

class Product
{
    public string Name { get; set; }
    public string Category { get; set; }
    public decimal Price { get; set; }

    public Product(string name, string category, decimal price)
    {
        Name = name;
        Category = category;
        Price = price;
    }
}

class Program
{
    static void Main()
    {
        // Step 2: Create a List of products
        List<Product> products = new List<Product>
        {
            new Product("Laptop", "Electronics", 1200.99m),
            new Product("Phone", "Electronics", 799.49m),
            new Product("Tablet", "Electronics", 499.99m)
        };

        // Step 3: Serialize the list of products to JSON
        string json = JsonSerializer.Serialize(products);
        
        // Step 4: Display the JSON string
        Console.WriteLine("Serialized JSON:\n" + json);

        // Step 5: Deserialize the JSON string back into a List of Product objects
        List<Product> deserializedProducts = JsonSerializer.Deserialize<List<Product>>(json);

        // Step 6: Display the deserialized products
        Console.WriteLine("\nDeserialized Products:");
        foreach (var product in deserializedProducts)
        {
            Console.WriteLine($"{product.Name} - {product.Category} - ${product.Price}");
        }
    }
}
```

</p>
</details>
</blockquote>


## 3.7 Advanced JSON Serialization and Deserialization with Nested Objects

**Objective:**  
Learn how to handle JSON serialization and deserialization of complex objects, including nested collections and custom settings in C#. This exercise focuses on working with more advanced JSON operations using the `System.Text.Json` namespace.

**Task:**  
Create a program that manages a complex data structure involving products and categories, serializes it to JSON with custom settings, and deserializes it back into C# objects.

**Steps:**
1. **Define Classes:** 
   - Create a `Category` class with properties: `Name` (string) and `Products` (`List<Product>`).
   - Create a `Product` class with properties: `Name` (string), `Price` (decimal), and `IsAvailable` (bool).
2. **Create a List of Categories:** Instantiate a `List<Category>` and add the following categories with products:
   - `"Electronics"`: Products - `("Laptop", 1200.99, true)`, `("Phone", 799.49, false)`, `("Tablet", 499.99, true)`
   - `"Furniture"`: Products - `("Desk", 150.00, true)`, `("Chair", 85.50, true)`
3. **Customize JSON Serialization:** Configure `JsonSerializerOptions` to format JSON with indented formatting and handle null values appropriately.
4. **Serialize to JSON:** Use the `JsonSerializer` class to convert the list of categories to a formatted JSON string.
5. **Display JSON:** Print the formatted JSON string to the console.
6. **Deserialize JSON:** Use the `JsonSerializer` class to convert the JSON string back into a list of `Category` objects.
7. **Display Deserialized Data:** Print the deserialized categories and their products to verify the data.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;
using System.Text.Json;
using System.Text.Json.Serialization;

class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public bool IsAvailable { get; set; }

    public Product(string name, decimal price, bool isAvailable)
    {
        Name = name;
        Price = price;
        IsAvailable = isAvailable;
    }
}

class Category
{
    public string Name { get; set; }
    public List<Product> Products { get; set; }

    public Category(string name, List<Product> products)
    {
        Name = name;
        Products = products;
    }
}

class Program
{
    static void Main()
    {
        // Step 2: Create a list of categories with nested products
        List<Category> categories = new List<Category>
        {
            new Category("Electronics", new List<Product>
            {
                new Product("Laptop", 1200.99m, true),
                new Product("Phone", 799.49m, false),
                new Product("Tablet", 499.99m, true)
            }),
            new Category("Furniture", new List<Product>
            {
                new Product("Desk", 150.00m, true),
                new Product("Chair", 85.50m, true)
            })
        };

        // Step 3: Configure custom JSON serialization options
        var options = new JsonSerializerOptions
        {
            WriteIndented = true, // Format JSON with indentation
            DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull // Ignore null values
        };

        // Step 4: Serialize the list of categories to JSON
        string json = JsonSerializer.Serialize(categories, options);

        // Step 5: Display the formatted JSON string
        Console.WriteLine("Serialized JSON:\n" + json);

        // Step 6: Deserialize the JSON string back into a list of Category objects
        List<Category> deserializedCategories = JsonSerializer.Deserialize<List<Category>>(json, options);

        // Step 7: Display the deserialized categories and their products
        Console.WriteLine("\nDeserialized Categories and Products:");
        foreach (var category in deserializedCategories)
        {
            Console.WriteLine($"Category: {category.Name}");
            foreach (var product in category.Products)
            {
                Console.WriteLine($"  - {product.Name} | Price: ${product.Price} | Available: {product.IsAvailable}");
            }
        }
    }
}
```

</p>
</details>
</blockquote>


## 3.8 Simple File I/O Operations in C#

**Objective:**  
Learn how to perform basic File I/O operations in C#, such as writing to a file, reading from a file, and appending text to an existing file using the `System.IO` namespace.

**Task:**  
Create a program that writes some text to a file, reads the contents of the file, and appends additional text to the file.

**Steps:**
1. **Write to a File:** Create a new file called `example.txt` and write the text `"Hello, World!"` to it.
2. **Read from the File:** Read the contents of `example.txt` and display it on the console.
3. **Append to the File:** Append the text `"This is an additional line."` to the file.
4. **Display Updated File Content:** Read and display the updated contents of the file.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.IO;

class Program
{
    static void Main()
    {
        string filePath = "example.txt";

        // Step 1: Write to a file
        File.WriteAllText(filePath, "Hello, World!");
        Console.WriteLine("Text written to file.");

        // Step 2: Read from the file
        string content = File.ReadAllText(filePath);
        Console.WriteLine("Current File Content:");
        Console.WriteLine(content);

        // Step 3: Append to the file
        File.AppendAllText(filePath, "\nThis is an additional line.");
        Console.WriteLine("Text appended to file.");

        // Step 4: Display updated file content
        string updatedContent = File.ReadAllText(filePath);
        Console.WriteLine("Updated File Content:");
        Console.WriteLine(updatedContent);
    }
}
```

</p>
</details>
</blockquote>


## 3.9 Advanced File I/O with JSON in C#

**Objective:**  
Learn how to combine File I/O operations with JSON serialization and deserialization in C#. This exercise focuses on saving complex objects to a file in JSON format and reading them back into the application.

**Task:**  
Create a program that manages a list of tasks, saves them to a file in JSON format, and reads them back from the file.

**Steps:**
1. **Define a Task Class:** Create a `TaskItem` class with properties: `Id` (int), `Title` (string), and `IsCompleted` (bool).
2. **Create a List of Tasks:** Initialize a list of tasks with some sample data.
3. **Serialize and Save to File:** Serialize the list of tasks to JSON format and save it to a file named `tasks.json`.
4. **Read and Deserialize from File:** Read the JSON data from `tasks.json` and deserialize it back into a list of `TaskItem` objects.
5. **Display the Tasks:** Print the tasks from the file to verify the data was saved and loaded correctly.
6. **Add a New Task and Update the File:** Add a new task to the list, update the JSON file, and display the updated tasks.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Text.Json;

class TaskItem
{
    public int Id { get; set; }
    public string Title { get; set; }
    public bool IsCompleted { get; set; }

    public TaskItem(int id, string title, bool isCompleted)
    {
        Id = id;
        Title = title;
        IsCompleted = isCompleted;
    }
}

class Program
{
    static void Main()
    {
        string filePath = "tasks.json";

        // Step 2: Create a list of tasks
        List<TaskItem> tasks = new List<TaskItem>
        {
            new TaskItem(1, "Complete C# project", false),
            new TaskItem(2, "Read documentation", true),
            new TaskItem(3, "Submit assignment", false)
        };

        // Step 3: Serialize the list of tasks to JSON and save it to a file
        SaveTasksToFile(tasks, filePath);

        // Step 4: Read and deserialize tasks from the file
        List<TaskItem> loadedTasks = LoadTasksFromFile(filePath);

        // Step 5: Display the tasks loaded from the file
        Console.WriteLine("Tasks Loaded from File:");
        DisplayTasks(loadedTasks);

        // Step 6: Add a new task, update the file, and display the updated tasks
        TaskItem newTask = new TaskItem(4, "Review code", false);
        loadedTasks.Add(newTask);
        SaveTasksToFile(loadedTasks, filePath);
        
        Console.WriteLine("\nUpdated Tasks:");
        DisplayTasks(loadedTasks);
    }

    // Method to save tasks to a file
    static void SaveTasksToFile(List<TaskItem> tasks, string filePath)
    {
        var options = new JsonSerializerOptions { WriteIndented = true };
        string json = JsonSerializer.Serialize(tasks, options);
        File.WriteAllText(filePath, json);
        Console.WriteLine($"Tasks saved to {filePath}.");
    }

    // Method to load tasks from a file
    static List<TaskItem> LoadTasksFromFile(string filePath)
    {
        if (!File.Exists(filePath))
        {
            Console.WriteLine("File not found, creating a new task list.");
            return new List<TaskItem>();
        }

        string json = File.ReadAllText(filePath);
        return JsonSerializer.Deserialize<List<TaskItem>>(json) ?? new List<TaskItem>();
    }

    // Method to display tasks
    static void DisplayTasks(List<TaskItem> tasks)
    {
        foreach (var task in tasks)
        {
            Console.WriteLine($"ID: {task.Id}, Title: {task.Title}, Completed: {task.IsCompleted}");
        }
    }
}
```

</p>
</details>
</blockquote>


## 3.10 Handling JSON Case Sensitivity Issues in C#

**Objective:**  
Learn how to handle case sensitivity issues when working with JSON data generated from different programming languages. This exercise focuses on resolving mismatches in JSON property names using the `PropertyNameCaseInsensitive` option in C#.

**Task:**  
Create a program that reads JSON data with property names that don't match the exact casing of your C# object properties, demonstrating how to deserialize it correctly using the `PropertyNameCaseInsensitive` option.

**Steps:**
1. **Define a Product Class:** Create a `Product` class with properties: `Name` (string), `Price` (decimal), and `IsAvailable` (bool).
2. **Prepare JSON Data with Different Case:** Create JSON data with properties in various cases (e.g., `name`, `PRICE`, `isavailable`) to simulate data generated from other languages.
3. **Attempt Deserialization Without Case Insensitivity:** Attempt to deserialize the JSON without using `PropertyNameCaseInsensitive` and observe the failure.
4. **Use Case Insensitivity Option:** Use `JsonSerializerOptions` with `PropertyNameCaseInsensitive` set to `true` to correctly deserialize the JSON.
5. **Display Deserialized Objects:** Print the deserialized objects to verify that the data is correctly mapped.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;
using System.Collections.Generic;
using System.Text.Json;

class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public bool IsAvailable { get; set; }
}

class Program
{
    static void Main()
    {
        // Step 2: Prepare JSON data with different casing
        string jsonData = @"
        [
            { ""name"": ""Laptop"", ""PRICE"": 1200.99, ""isavailable"": true },
            { ""name"": ""Phone"", ""PRICE"": 799.49, ""isavailable"": false },
            { ""name"": ""Tablet"", ""PRICE"": 499.99, ""isavailable"": true }
        ]";

        // Step 3: Attempt deserialization without case insensitivity
        Console.WriteLine("Attempting deserialization without case insensitivity:");
        try
        {
            List<Product> products = JsonSerializer.Deserialize<List<Product>>(jsonData);
            DisplayProducts(products);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Deserialization failed: {ex.Message}");
        }

        // Step 4: Deserialize using PropertyNameCaseInsensitive
        var options = new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true
        };

        Console.WriteLine("\nDeserialization with PropertyNameCaseInsensitive set to true:");
        try
        {
            List<Product> productsInsensitive = JsonSerializer.Deserialize<List<Product>>(jsonData, options);
            DisplayProducts(productsInsensitive);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Deserialization failed: {ex.Message}");
        }
    }

    // Method to display products
    static void DisplayProducts(List<Product> products)
    {
        if (products == null)
        {
            Console.WriteLine("No products were deserialized.");
            return;
        }

        foreach (var product in products)
        {
            Console.WriteLine($"Name: {product.Name}, Price: {product.Price}, Available: {product.IsAvailable}");
        }
    }
}
```

</p>
</details>
</blockquote>