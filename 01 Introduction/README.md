# 01 Exercises: Introduction

## 1.1 Getting started with .NET programming

### Install .NET

Download .NET 8.0.x SDK from [here](https://dotnet.microsoft.com/download) it.
Make sure that you download version 8.0.x or newer.

### Install IDE

Install an IDE to develop .NET applications.

Your teacher will be using [JetBrains Rider](https://www.jetbrains.com/rider/download/). This is basically IntelliJ for .NET.

Rider does not have a free version, so you will need a [student account](https://www.jetbrains.com/community/education/) at JetBrains (you may already have one from a previous semester).

In one of the steps of the installation wizard for Rider, make sure that you check “Add launchers dir to PATH”

Popular alternatives to Rider are [Visual Studio or Visual Studio Code](https://visualstudio.microsoft.com/downloads/).

## 1.2 Baby steps

Go through the [offical tutorial](https://dotnet.microsoft.com/learn/dotnet/in-browser-tutorial/1)

## 1.3 Your first Solution

Open your IDE and create a Solution. You might have to define an initial Project, but we will create our own Project shortly regardless.

Create a "ConsoleApplication" Project in your newly created Solution.

You should end up with something similar to this

![My first Solution](Images\myfirstsolution.png)

## 1.4 Your first C# class

Create a new `Person` class in your "ConsoleApplication" Project.

The class should contain a `name` Property and an `Introduce()` method that prints "Hi, I am _name_" to the console. Use string interpolation.

In the `Main` method of the `Program` class, create an instance of the `Person` class, specifying the name with either a constructor or [object initializer](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/object-and-collection-initializers) notation and then call the `Introduce()` method on the object.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
namespace ConsoleApplication
{
    public class Person
    {
        public string Name { get; set; }

        public Person()
        {

        }

        public Person(string name)
        {
            Name = name;
        }
    }
}
```

```csharp
using System;

namespace ConsoleApplication
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            //Using constructor
            Person person1 = new Person("Alice");
            Console.WriteLine($"Hi, I am {person1.Name}");

            //Using object initializer
            Person person2 = new Person{Name = "Bob"};
            Console.WriteLine($"Hi, I am {person2.Name}");
        }
    }
}
```

</p>
</details>
</blockquote>

## 1.5 Printing numbers

Create a new Project named "Numbers" for this exercise, inside your Solution folder.

Create a class `NumberWriter` in your Project.

Create a method that prints out all even numbers between 0 and x, where x is a method parameter. Call it from the Main method and verify the output.

Create a method that prints out all uneven numbers between 0 and x, where x is a method parameter. Call it from the Main method and verify the output.

Create a method that prints out all numbers divisible by y, between 0 and x, where x and y are method parameters. Call it from the Main method and verify the output.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;

namespace Numbers
{
    public class NumberWriter
    {
        public void PrintEvenNumbersFromZeroToX(int x)
        {
            for (int i = 0; i <= x; i++)
            {
                if(i % 2 == 0)
                {
                    Console.WriteLine(i);
                }
            }
        }

        public void PrintOddNumbersFromZeroToX(int x)
        {
            for (int i = 0; i <= x; i++)
            {
                if (i % 2 != 0)
                {
                    Console.WriteLine(i);
                }
            }
        }

        public void PrintNumbersFromZeroToXDivisibleByY(int x, int y)
        {
            for (int i = 0; i <= x; i++)
            {
                if (i % y == 0)
                {
                    Console.WriteLine(i);
                }
            }
        }
    }
}
```

```csharp
namespace Numbers
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            NumberWriter numberWriter = new NumberWriter();

            numberWriter.PrintEvenNumbersFromZeroToX(10);
            numberWriter.PrintOddNumbersFromZeroToX(10);
            numberWriter.PrintNumbersFromZeroToXDivisibleByY(10, 3);
        }
    }
}
```

</p>
</details>
</blockquote>

## 1.6 Calculator and method overloading

Create a new Project called "MathLib".

Create a `Calculator` class in the "MathLib" Project.

Create an `Add` method that takes two numbers and returns their sum.

Create an overloaded `Add` method that takes an array of integers, adds all the integers together, then returns the result. Use the for-each loop.

Call both methods from the Main method and verify the output.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
namespace MathLib
{
    public class Calculator
    {
        public int Add(int a, int b)
        {
            return a + b;
        }

        public int Add(int[] numbers)
        {
            int sum = 0;

            foreach (int number in numbers)
            {
                sum += number;
            }

            return sum;
        }
    }
}
```

```csharp
using System;

namespace MathLib
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            Calculator calculator = new Calculator();

            Console.WriteLine($"Calling add with two numbers: {calculator.Add(2, 3)}");
            Console.WriteLine($"Calling add with an integer array: {calculator.Add(new int[]{1,2,3,4,5})}");
        }
    }
}
```

</p>
</details>
</blockquote>

## 1.7 Calculator and reading input from keyboard

Create a new method in the `Calculator` class from the previous exercise that reads two integer inputs from the console and then displays the maximum of the two.

Call the method from the Main method and verify the output.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

(Only new method shown, not showing the rest of class Calculator)

```csharp
public void ReadTwoIntegersAndPrintTheHighest()
{
    Console.WriteLine("Enter the first number: ");
    string firstNumberAsString = Console.ReadLine();
    int firstNumberAsInt = Convert.ToInt32(firstNumberAsString);

    Console.WriteLine("Enter the second number: ");
    string secondNumberAsString = Console.ReadLine();
    int secondNumberAsInt = Convert.ToInt32(secondNumberAsString);

    Console.WriteLine($"The highest numbers is: {Math.Max(firstNumberAsInt, secondNumberAsInt)}");
}
```

```csharp
namespace MathLib
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            Calculator calculator = new Calculator();

            calculator.ReadTwoIntegersAndPrintTheHighest();
        }
    }
}
```

</p>
</details>
</blockquote>

## 1.8 String manipulation

Create a new Project called "Strings" in your Solution.

Create a class `StringManipulator` in your Project.

Create a methods, that, given two strings, a and b, returns the result of putting them together in the order "abba", e.g. "Hi" and "Bye" should return "HiByeByeHi".

Examples:

```
MakeABBA("Hi", "Bye") → "HiByeByeHi"
MakeABBA("Yo", "Alice") → "YoAliceAliceYo"
MakeABBA("What", "Up") → "WhatUpUpWhat"
```

You can use string concatenation, but consider a C# specific approach.

Call your method from the Main method a couple of times with various input and verify the output.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System;

namespace Strings
{
    public class StringManipulator
    {
        public void MakeABBA(string a, string b)
        {
            Console.WriteLine($"{a}{b}{b}{a}");
        }
    }
}
```

```csharp
namespace Strings
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            StringManipulator stringManipulator = new StringManipulator();

            stringManipulator.MakeABBA("A", "B");
            stringManipulator.MakeABBA("Hi", "Bye");
            stringManipulator.MakeABBA("Alice", "Bob");
        }
    }
}
```

</p>
</details>
</blockquote>

## 1.9 Surround With

Create a method in the `StringManipulator` class with the following behaviour:

Given an "outer" string of exactly 4 characters, such as "<<>>" or "(())", and a word, return a new string where the word is in the middle of the out string, e.g. "<<word>>".

Examples:

```
MakeWord("<<>>", "Yay") → "<<Yay>>"
MakeWord("<<>>", "WooHoo") → "<<WooHoo>>"
MakeWord("[[]]", "word") → "[[word]]"
```

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

(Only showing the new method)

```csharp
public string MakeWord(string outer, string word)
{
    return outer.Substring(0, 2) + word + outer.Substring(2);
}
```

```csharp
using System;

namespace Strings
{
    internal class Program
    {
        public static void Main(string[] args)
        {
            StringManipulator stringManipulator = new StringManipulator();

            Console.WriteLine($"Result: {stringManipulator.MakeWord("<<>>", "test")}");
        }
    }
}
```

</p>
</details>
</blockquote>
