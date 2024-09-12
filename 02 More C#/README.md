# 02 Exercises: More C#

## 2.1 Simple Lambda Expression for Arithmetic Operations

**Objective:** Practice creating basic lambda expressions and using them to perform arithmetic operations.

**Task:**
1. Write a lambda expression that takes two integers as input and returns their sum.
2. Write another lambda expression that returns the product of two integers.
3. Use these expressions in a console application to print the sum and product of two numbers entered by the user.

**Steps:**
- Define a lambda expression for addition and multiplication.
- Prompt the user for two integer inputs.
- Use the lambda expressions to calculate and display the results.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
Func<int, int, int> add = (a, b) => a + b;
Func<int, int, int> multiply = (a, b) => a * b;

Console.Write("Enter first number: ");
int num1 = int.Parse(Console.ReadLine());

Console.Write("Enter second number: ");
int num2 = int.Parse(Console.ReadLine());

Console.WriteLine($"Sum: {add(num1, num2)}");
Console.WriteLine($"Product: {multiply(num1, num2)}");
```

</p>
</details>
</blockquote>

## 2.2 Lambda Expressions with Custom Sorting

**Objective:** Learn how to use lambda expressions for custom sorting in a list.

**Task:**
1. Create a list of strings representing names.
2. Write a lambda expression to sort the list in descending order by the length of each name.
3. Display the sorted list.

**Steps:**
- Create a list of names.
- Use the `List<T>.Sort` method with a lambda expression to define the custom sort order.
- Print the sorted list.


<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
List<string> names = new List<string> { "Alice", "Bob", "Christopher", "David", "Eve" };

// Sorting names by length in descending order using a lambda expression
names.Sort((name1, name2) => name2.Length.CompareTo(name1.Length));

Console.WriteLine("Sorted names by length (descending):");
foreach (var name in names)
{
    Console.WriteLine(name);
}
```

</p>
</details>
</blockquote>

## 2.3 Lambda Expressions with LINQ (Filtering and Selecting)

**Objective:** Practice using lambda expressions in combination with LINQ to filter and transform collections.

**Task:**
1. Create a list of integers representing test scores.
2. Use a LINQ query with a lambda expression to:
   - Filter scores that are 70 or above.
   - Project the filtered scores into a new list with a message indicating whether the score is a "Pass" or "Fail" (based on 70).
3. Display the results.

**Steps:**
- Use `Where` to filter scores based on the pass mark.
- Use `Select` to transform each score into a message ("Pass" or "Fail").
- Display the transformed results.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
List<int> scores = new List<int> { 85, 67, 92, 45, 76, 88, 59 };

var filteredScores = scores
    .Where(score => score >= 70)
    .Select(score => $"{score}: Pass");

Console.WriteLine("Filtered and processed scores:");
foreach (var result in filteredScores)
{
    Console.WriteLine(result);
}
```

</p>
</details>
</blockquote>

## 2.4 Handling Nullable Value Types

**Objective:** Practice working with nullable value types and null-coalescing operators.

**Task:**
1. Create a console application that asks the user to input an integer (age).
2. Store the age in a nullable `int?` variable.
3. Use the null-coalescing operator (`??`) to provide a default value if the user input is null or invalid.
4. Display the user’s age or the default value (e.g., "Age not provided: Default value = 18").

**Steps:**
- Read user input and attempt to parse it to `int?`.
- Handle invalid or empty input by assigning `null`.
- Use the `??` operator to ensure a default value is used when `null` is encountered.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
int? userAge = null;
Console.Write("Enter your age: ");
string input = Console.ReadLine();
if (int.TryParse(input, out int age))
{
    userAge = age;
}
Console.WriteLine($"Your age is: {userAge ?? 18}");
```

</p>
</details>
</blockquote>

## 2.5 Nullable Reference Types with Class Properties

**Objective:** Practice working with nullable reference types and the null-conditional operator (`?.`).

**Task:**
1. Create a `Person` class with two properties: `string? Name` and `int? Age`.
2. Write a method to display the person’s name and age.
3. In the `Main` method, create two `Person` objects:
   - One with both properties set to values.
   - One with both properties set to `null`.
4. Use null-conditional operators to safely display values without causing exceptions.

**Steps:**
- Use `?.` to check for nulls before accessing object properties.
- Display appropriate messages when properties are `null`.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
class Person
{
    public string? Name { get; set; }
    public int? Age { get; set; }

    public void DisplayInfo()
    {
        Console.WriteLine($"Name: {Name ?? "No Name"}");
        Console.WriteLine($"Age: {Age?.ToString() ?? "No Age"}");
    }
}

Person person1 = new Person { Name = "Alice", Age = 25 };
Person person2 = new Person { Name = null, Age = null };

person1.DisplayInfo();
person2.DisplayInfo();
```

</p>
</details>
</blockquote>


## 2.6 Working with Nullable Types and LINQ

**Objective:** Practice combining nullable types with LINQ queries.

**Task:**
1. Create a list of nullable integers (`List<int?>`) representing exam scores.
2. Populate the list with a mixture of `int` values and `null` values.
3. Write a LINQ query to:
   - Filter out the `null` values.
   - Calculate and display the average of the non-null scores.

**Steps:**
- Use the `Where()` clause to filter non-null values.
- Use the `Average()` method to compute the average.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
List<int?> examScores = new List<int?> { 85, 90, null, 75, 100, null, 95 };
var validScores = examScores.Where(score => score.HasValue).Select(score => score.Value);
double averageScore = validScores.Average();

Console.WriteLine($"The average score is: {averageScore}");
```

</p>
</details>
</blockquote>

## 2.7 Nullable References with Methods

**Objective:** Understand how to work with nullable reference types as method parameters and return types.

**Task:**
1. Create a `Book` class with a `Title` and `Author`, both as nullable reference types (`string?`).
2. Write a method `string? GetAuthor(Book? book)` that takes a nullable `Book` object as input and returns the author's name if available.
3. If the `Book` object is `null` or the `Author` property is `null`, the method should return `null`.
4. In the `Main` method, test the function by passing both `null` and valid `Book` objects.

**Steps:**
- Make sure to handle `null` inputs using nullable references.
- Use null-checks and the null-conditional operator to return appropriate values.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
class Book
{
    public string? Title { get; set; }
    public string? Author { get; set; }
}

string? GetAuthor(Book? book)
{
    return book?.Author;
}

Book? book1 = new Book { Title = "1984", Author = "George Orwell" };
Book? book2 = new Book { Title = "Unknown", Author = null };
Book? book3 = null;

Console.WriteLine(GetAuthor(book1) ?? "No author provided");
Console.WriteLine(GetAuthor(book2) ?? "No author provided");
Console.WriteLine(GetAuthor(book3) ?? "No book provided");
```

</p>
</details>
</blockquote>



## 2.8 Nullability Warnings and the `!` Operator

**Objective:** Learn to handle nullable references and how to suppress nullability warnings using the `!` (null-forgiving) operator.

**Task:**
1. Create a `Student` class with two nullable reference type properties: `string? Name` and `string? Email`.
2. Write a method `SendEmail(Student student)` that attempts to send an email to the student. If `Email` is `null`, the method should print "Email address not available."
3. In the `Main` method, create a `Student` object with a `null` email.
4. Modify the code to deliberately ignore the nullability warning using the `!` operator when accessing `student.Email`.

**Steps:**
- Handle the nullable `Email` property in the `SendEmail` method safely.
- Use the `!` operator to forcefully tell the compiler that `Email` is not null, and observe the behavior.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
class Student
{
    public string? Name { get; set; }
    public string? Email { get; set; }
}

void SendEmail(Student student)
{
    if (student.Email == null)
    {
        Console.WriteLine("Email address not available.");
    }
    else
    {
        // Using null-forgiving operator
        Console.WriteLine($"Sending email to {student.Email!}");
    }
}

Student student1 = new Student { Name = "John", Email = null };
SendEmail(student1);
```

</p>
</details>
</blockquote>



## 2.9 Nullable References in Collections

**Objective:** Work with collections of nullable reference types and explore how to handle null values in a list or array.

**Task:**
1. Create a `List<string?>` that stores a collection of names, some of which may be `null`.
2. Write a method `DisplayNames(List<string?> names)` that iterates through the list and prints each name.
3. For each `null` entry in the list, print "Unknown Name" instead.
4. In the `Main` method, initialize a list with a few names and `null` values and pass it to the `DisplayNames` method.

**Steps:**
- Use null-coalescing operators to provide fallback values for `null` entries in the list.
- Safely handle nullable references in the context of collection iteration.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
void DisplayNames(List<string?> names)
{
    foreach (var name in names)
    {
        Console.WriteLine(name ?? "Unknown Name");
    }
}

List<string?> namesList = new List<string?> { "Alice", null, "Bob", null, "Charlie" };
DisplayNames(namesList);
```

</p>
</details>
</blockquote>

## 2.10 Creating a Simple Task

**Objective:** Understand how to create and run a `Task`.

**Task:**
1. Create a console application that defines a simple `Task` to print a message after a delay of 3 seconds.
2. Run the `Task` and print a "Task Started" message before the delay and a "Task Completed" message after the delay.

**Steps:**
- Use `Task.Delay` for simulating the delay.
- Run the task using `Task.Run()`.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
Task myTask = Task.Run(() =>
{
    Console.WriteLine("Task Started");
    Task.Delay(3000).Wait(); // Simulate work
    Console.WriteLine("Task Completed");
});
myTask.Wait();
```

</p>
</details>
</blockquote>

## 2.11 Returning a Value from a Task

**Objective:** Learn how to return a value from a `Task`.

**Task:**
1. Write a `Task<int>` that returns the sum of two numbers after a delay.
2. Run the task and print the result.

**Steps:**
- Use `Task.Run()` to create a task that returns a value.
- Use `Task.Result` to get the result of the task.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
Task<int> sumTask = Task.Run(() =>
{
    Task.Delay(2000).Wait(); // Simulate work
    return 5 + 7;
});

int result = sumTask.Result;
Console.WriteLine($"Sum: {result}");
```

</p>
</details>
</blockquote>

## 2.12 Using `async` and `await`

**Objective:** Learn how to use `async` and `await` for asynchronous methods.

**Task:**
1. Create an `async` method called `GetMessageAsync` that returns a message after a 2-second delay.
2. Call this method using `await` and print the returned message.

**Steps:**
- Use `await Task.Delay()` inside the asynchronous method.
- Use `await` to call the asynchronous method.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
async Task<string> GetMessageAsync()
{
    await Task.Delay(2000); // Simulate work
    return "Hello, Async!";
}

string message = await GetMessageAsync();
Console.WriteLine(message);
```

</p>
</details>
</blockquote>

## 2.13 Running Multiple Tasks in Parallel

**Objective:** Run multiple tasks in parallel using `Task.WhenAll()`.

**Task:**
1. Create three `Task` instances that each take 2 seconds to complete.
2. Use `Task.WhenAll()` to wait for all tasks to complete, then print a message indicating that all tasks are finished.

**Steps:**
- Use `Task.Delay()` to simulate work.
- Use `Task.WhenAll()` to await all tasks simultaneously.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
Task task1 = Task.Delay(2000);
Task task2 = Task.Delay(2000);
Task task3 = Task.Delay(2000);

await Task.WhenAll(task1, task2, task3);
Console.WriteLine("All tasks completed.");
```

</p>
</details>
</blockquote>

## 2.14

Handling Task Exceptions

**Objective:** Learn how to handle exceptions in asynchronous tasks.

**Task:**
1. Create an `async` method that throws an exception after a delay.
2. Call the method using `await` inside a `try-catch` block and handle the exception.

**Steps:**
- Use `await Task.Delay()` and then throw an exception.
- Catch the exception using a `try-catch` block.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
async Task FailAsync()
{
    await Task.Delay(1000);
    throw new InvalidOperationException("Something went wrong.");
}

try
{
    await FailAsync();
}
catch (Exception ex)
{
    Console.WriteLine($"Exception caught: {ex.Message}");
}
```

</p>
</details>
</blockquote>

## 2.15 Using `Task.Run` and `await`

**Objective:** Combine `Task.Run()` with `await` to offload work to a background thread.

**Task:**
1. Create a method that performs a CPU-bound task (e.g., summing an array of numbers).
2. Use `Task.Run()` to run this task in the background and return the result using `await`.

**Steps:**
- Use `Task.Run()` for CPU-bound tasks.
- Use `await` to await the result of the task.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
int[] numbers = { 1, 2, 3, 4, 5 };

Task<int> sumTask = Task.Run(() =>
{
    return numbers.Sum();
});

int result = await sumTask;
Console.WriteLine($"Sum: {result}");
```

</p>
</details>
</blockquote>

## 2.16 Chaining Tasks with `ContinueWith`

**Objective:** Chain tasks together using `Task.ContinueWith()`.

**Task:**
1. Create a `Task` that prints a message.
2. Use `ContinueWith()` to create a second `Task` that prints a follow-up message after the first task completes.

**Steps:**
- Use `ContinueWith()` to chain tasks together.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
Task task1 = Task.Run(() =>
{
    Console.WriteLine("First task completed.");
});

Task task2 = task1.ContinueWith(t =>
{
    Console.WriteLine("Second task following the first one.");
});

await task2;
```

</p>
</details>
</blockquote>

## 2.17 Canceling a Task with `CancellationToken`

**Objective:** Learn how to cancel tasks using `CancellationToken`.

**Task:**
1. Create a `Task` that runs in a loop and checks for a cancellation token to stop.
2. Use a `CancellationTokenSource` to trigger the cancellation.

**Steps:**
- Use `CancellationToken` to check if the task should be canceled.
- Use `CancellationTokenSource` to trigger the cancellation.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
CancellationTokenSource cts = new CancellationTokenSource();
CancellationToken token = cts.Token;

Task task = Task.Run(() =>
{
    for (int i = 0; i < 10; i++)
    {
        if (token.IsCancellationRequested)
        {
            Console.WriteLine("Task was canceled.");
            return;
        }
        Console.WriteLine($"Processing {i}");
        Task.Delay(500).Wait();
    }
}, token);

// Cancel the task after 2 seconds
await Task.Delay(2000);
cts.Cancel();

await task;
```

</p>
</details>
</blockquote>

## 2.18 Using `Task.WhenAny` to Handle Multiple Tasks

**Objective:** Learn how to handle the first completed task using `Task.WhenAny()`.

**Task:**
1. Create three `Task` instances that each take different times to complete.
2. Use `Task.WhenAny()` to wait for the first task to complete and print a message indicating which task finished first.

**Steps:**
- Use `Task.Delay()` to simulate different task completion times.
- Use `Task.WhenAny()` to get the first completed task.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
Task task1 = Task.Delay(2000);
Task task2 = Task.Delay(3000);
Task task3 = Task.Delay(1000);

Task firstCompletedTask = await Task.WhenAny(task1, task2, task3);
Console.WriteLine("First task completed.");
```

</p>
</details>
</blockquote>

## 2.19 Async File I/O with `async` and `await`

**Objective:** Use `async` and `await` with file I/O operations.

**Task:**
1. Write an `async` method that reads the contents of a file asynchronously.
2. Call the method using `await` and print the file contents.

**Steps:**
- Use `StreamReader.ReadToEndAsync()` for asynchronous file reading.
- Use `await` to read the file content.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
async Task<string> ReadFileAsync(string filePath)
{
    using (StreamReader reader = new StreamReader(filePath))
    {
        return await reader.ReadToEndAsync();
    }
}

string content = await ReadFileAsync("example.txt");
Console.WriteLine(content);
```

</p>
</details>
</blockquote>