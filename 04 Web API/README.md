# 04 Exercises: Web API

## 4.1 HTTP GET Request

**Objective:**  
Understand the basic structure of a GET request using an API test tool and observe how the server responds.

**Task:**  
Send a GET request to the [JSONPlaceholder API](https://jsonplaceholder.typicode.com/) to retrieve a list of posts and analyze the response.

**Steps:**

1. Use [Postman](https://www.postman.com/), [cURL](https://curl.se/), [HTTPie](https://httpie.io/app), or any other tool you want.
2. Send a GET request to retrieve a list of posts from the API endpoint `/posts`.
3. Analyze the status code, response headers, and the JSON body.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```
GET https://jsonplaceholder.typicode.com/posts/
```

The response should display the status code <code>(200 OK)</code>, response headers, and the JSON body containing the list of posts, like this:

```json
[
  {
    "userId": 1,
    "id": 1,
    "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
    "body": "quia et suscipit\nsuscipit..."
  },
  ...
]
```

</p>
</details>
</blockquote>

## 4.2 HTTP POST Requests

**Objective:**  
Learn how to send data to a server using a POST request and understand server responses.

**Task:**  
Send a POST request to create a new resource on the [JSONPlaceholder API server](https://jsonplaceholder.typicode.com/).

**Steps:**

1. Generate a JSON string with the content of a new post.
2. Set the appropriate headers (`Content-Type: application/json`) to inform the server about the data format.
3. Send a POST request to the API endpoint `/posts` with the JSON data in the body to create a new post.
4. Analyze the response, focusing on the status code and the newly created resource.

**Solution using HTTPie Command Line:**

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

Data in JSON format

```json
{
  "id": 1,
  "title": "foo",
  "body": "bar",
  "userId": 1
}
```

```bash
POST https://jsonplaceholder.typicode.com/posts
```

The response should display the status code (`201 Created`), response headers, and the JSON body containing the new post.

```json
{
  "id": 101,
  "title": "foo",
  "body": "bar",
  "userId": 1
}
```

**Note** that no actual changes takes place on the JSONPlaceholder API server. The response is faked, and the data remains the same.

</p>
</details>
</blockquote>

## 4.3 HTTP PUT Request to update data

**Objective:**  
Understand how to update existing resources on a server using PUT requests.

**Task:**  
Send a PUT request to update a post on the [JSONPlaceholder API](https://jsonplaceholder.typicode.com/) and examine how the server responds.

**Steps:**

1. Generate a JSON string with the updated values of an existing post.
2. Send a PUT request to the API endpoint `/posts/1` to update the existing post.
3. Analyze the response, focusing on the status code and the updated resource.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

Data in JSON format

```json
{
  "id": 1,
  "title": "updated title",
  "body": "updated body",
  "userId": 1
}
```

```bash
PUT https://jsonplaceholder.typicode.com/posts/1
```

The response should display the status code (`200 OK`), response headers, and the JSON body containing the updated post.

```json
{
  "id": 1,
  "title": "updated title",
  "body": "updated body",
  "userId": 1
}
```

**Note** that no actual changes takes place on the JSONPlaceholder API server. The response is faked, and the data remains the same.

</p>
</details>
</blockquote>

## 4.4 GET Request using `HttpClient`

**Objective:**  
Understand how to use the existing `HttpClient`class in the .NET library to send HTTP requests.

**Task:**  
Use the `HttpClient` to send the same request as you did in exercise 4.1 on the [JSONPlaceholder API](https://jsonplaceholder.typicode.com/) and examine how the server responds.

**Steps:**

1. Create a Main-method where you create an instance of the `HttpClient` class.
2. Make a GET request using the `GetAsync()` method and store the result in a `HttpResponseMessage` variable.
3. Extract the body from the `HttpResponseMessage` and print it to console

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
class Program
{
    static async Task Main()
    {
        HttpClient client = new HttpClient();
        HttpResponseMessage response = await client.GetAsync("https://jsonplaceholder.typicode.com/posts");
        string responseBody = await response.Content.ReadAsStringAsync();
        Console.WriteLine($"Status Code: {response.StatusCode}");
        Console.WriteLine($"Response Body: {responseBody}");
    }
}
```

</p>
</details>
</blockquote>

## 4.5 POST Request using `HttpClient`

**Objective:**  
Understand how to use the existing `HttpClient`class in the .NET library to send HTTP requests.

**Task:**  
Use the `HttpClient` to send the same request as you did in exercise 4.2 on the [JSONPlaceholder API](https://jsonplaceholder.typicode.com/) and examine how the server responds.

**Steps:**

1. Create a Main-method where you create an instance of the `HttpClient` class.
2. Create a JSON string containing the values for the new Post to create.
3. Convert the string to a `StringContent` object with proper encoding.
4. Make a POST request using the `PostAsync()` method with your JSON data, and store the result in a `HttpResponseMessage` variable.
5. Extract the body from the `HttpResponseMessage` and print it to console

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System.Text;

class Program
{
    static async Task Main()
    {
        HttpClient client = new HttpClient();
        string jsonData = "{\"title\": \"foo\", \"body\": \"bar\", \"userId\": 1}";
        StringContent content = new StringContent(jsonData, Encoding.UTF8, "application/json");
        HttpResponseMessage response = await client.PostAsync("https://jsonplaceholder.typicode.com/posts", content);
        string responseBody = await response.Content.ReadAsStringAsync();
        Console.WriteLine($"Status Code: {response.StatusCode}");
        Console.WriteLine($"Response Body: {responseBody}");
    }
}
```

</p>
</details>
</blockquote>

## 4.6 PUT Request using `HttpClient`

**Objective:**  
Understand how to use the existing `HttpClient`class in the .NET library to send HTTP requests.

**Task:**  
Use the `HttpClient` to send the same request as you did in exercise 4.3 on the [JSONPlaceholder API](https://jsonplaceholder.typicode.com/) and examine how the server responds.

**Steps:**

1. Create a Main-method where you create an instance of the `HttpClient` class.
2. Create a JSON string containing the updated values for the Post to update.
3. Convert the string to a `StringContent` object with proper encoding.
4. Make a PUT request using the `PutAsync()` method with your JSON data, and store the result in a `HttpResponseMessage` variable.
5. Extract the body from the `HttpResponseMessage` and print it to console

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using System.Text;

class Program
{
    static async Task Main()
    {
        HttpClient client = new HttpClient();
        string jsonData = "{\"id\": 1, \"title\": \"updated title\", \"body\": \"updated body\", \"userId\": 1}";
        StringContent content = new StringContent(jsonData, Encoding.UTF8, "application/json");
        HttpResponseMessage response = await client.PutAsync("https://jsonplaceholder.typicode.com/posts/1", content);
        string responseBody = await response.Content.ReadAsStringAsync();
        Console.WriteLine($"Status Code: {response.StatusCode}");
        Console.WriteLine($"Response Body: {responseBody}");
    }
}
```

</p>
</details>
</blockquote>

## 4.7 Setting Up a Basic Minimal API

**Objective:**  
Create a basic minimal API using ASP.NET Core that will act as the foundation for a small application.

**Task:**  
Set up a new minimal API project and create a simple GET endpoint.

**Steps:**

1. Create a new project, with type "Web" and use the template "Empty"
2. The `Program.cs` file should already create a Builder using the `WebApplication` class, and run the `Build()` method. If not, create this.
3. Map a new GET request to the endpoint "/helloworld" endpoint and have it output "Hello, Minimal API!".
4. Run your program.
5. Open your browser and navigate to http://localhost:5000/ and verify that your output is displayed

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => "Hello World!");
app.MapPost("/helloworld", () => "Hello, Minimal API!");

app.Run();
```

</p>
</details>
</blockquote>

## 4.8 Create a Basic GET Endpoint for `Posts`

**Objective:**  
Create a simple GET endpoint to return a list of posts using in-memory data.

**Task:**  
Add a GET endpoint that retrieves all posts from an in-memory list.

**Steps:**

1. Create a new class `Post`. It must contain a `Id`, `UserId`, `Title`, and `Body`
2. Open the `Program.cs` file.
3. Define a list of posts with some initial data.
4. Map a GET endpoint to return the list of posts.
5. Run your program.
6. Open your browser and navigate to http://localhost:5000/posts and verify that all posts are displayed

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
namespace MinimalWebAPI;

public class Post
{
    public int Id { get; set; }
    public int UserId { get; set; }
    public string Title { get; set; }
    public string Body { get; set; }
}
```

```csharp
using Microsoft.AspNetCore.Builder;

namespace MinimalWebAPI;

internal class Program
{
    private static void Main()
    {
        var builder = WebApplication.CreateBuilder();
        var app = builder.Build();

        // In-memory list of posts
        var posts = new List<Post>
        {
            new Post { Id = 1, UserId = 1, Title = "Post 1", Body = "Body of Post 1" },
            new Post { Id = 2, UserId = 1, Title = "Post 2", Body = "Body of Post 2" }
        };

        // GET endpoint to retrieve all posts
        app.MapGet("/posts", () => posts);

        app.Run();
    }
}
```

</p>
</details>
</blockquote>

## 4.9 Create a GET Endpoint to Retrieve a Specific Post

**Objective:**  
Add functionality to retrieve a specific post by its ID.

**Task:**  
Create a GET endpoint that takes an ID as a parameter and returns the corresponding post.

**Steps:**

1. Open the `Program.cs` file.
2. Map a new GET endpoint that accepts an ID and retrieves the post with the matching ID.
3. Run your program.
4. Open your browser and navigate to http://localhost:5000/posts/1 and verify that all the specific post is displayed.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;

namespace MinimalWebAPI;

internal class Program
{
    private static void Main()
    {
        var builder = WebApplication.CreateBuilder();
        var app = builder.Build();

        var posts = new List<Post>
        {
            new Post { Id = 1, UserId = 1, Title = "Post 1", Body = "Body of Post 1" },
            new Post { Id = 2, UserId = 1, Title = "Post 2", Body = "Body of Post 2" }
        };

        // GET endpoint to retrieve all posts
        app.MapGet("/posts", () => posts);

        // GET endpoint to retrieve a specific post by ID
        app.MapGet("/posts/{id:int}", (int id) =>
        {
            var post = posts.FirstOrDefault(p => p.Id == id);
            return post is not null ? Results.Ok(post) : Results.NotFound("Post not found");
        });

        app.Run();
    }
}
```

</p>
</details>
</blockquote>

## 4.10 Create a POST Endpoint to Add a New Post

**Objective:**  
Add functionality to create a new post and add it to the in-memory list.

**Task:**  
Create a POST endpoint that accepts post data and adds it to the list.

**Steps:**

1. Open the `Program.cs` file.
2. Map a new POST endpoint to receive a new post from the client, assign it a new ID, and add it to the list (ensure that the ID is unique).
3. Run your program.
4. Test your endpoint using either `HttpClient` or a Web API test tool.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;

namespace MinimalWebAPI;

internal class Program
{
    private static void Main()
    {
        var builder = WebApplication.CreateBuilder();
        var app = builder.Build();

        var posts = new List<Post>
        {
            new Post { Id = 1, UserId = 1, Title = "Post 1", Body = "Body of Post 1" },
            new Post { Id = 2, UserId = 1, Title = "Post 2", Body = "Body of Post 2" }
        };

        // GET endpoint to retrieve all posts
        app.MapGet("/posts", () => posts);

        // GET endpoint to retrieve a specific post by ID
        app.MapGet("/posts/{id:int}", (int id) =>
        {
            var post = posts.FirstOrDefault(p => p.Id == id);
            return post is not null ? Results.Ok(post) : Results.NotFound("Post not found");
        });

        // POST endpoint to add a new post
        app.MapPost("/posts", (Post newPost) =>
        {
            newPost.Id = posts.Max(p => p.Id) + 1; // Assign a new ID
            posts.Add(newPost);
            return Results.Created($"/posts/{newPost.Id}", newPost);
        });

        app.Run();
    }
}
```

</p>
</details>
</blockquote>

## 4.11 Create PUT and DELETE Endpoints

**Objective:**  
Implement the functionality to update and delete posts.

**Task:**  
Create PUT and DELETE endpoints for updating and deleting posts by their ID.

**Steps:**
1. Open the `Program.cs` file.
2. Map a new DELETE endpoint to remove an element with a specific ID from the list.
3. Map a new PUT endpoint to update an existing element with a specific ID with new values.
3. Run your program.
4. Test your endpoint using either `HttpClient` or a Web API test tool.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Http;

namespace MinimalWebAPI;

internal class Program
{
    private static void Main()
    {
        var builder = WebApplication.CreateBuilder();
        var app = builder.Build();

        var posts = new List<Post>
        {
            new Post { Id = 1, UserId = 1, Title = "Post 1", Body = "Body of Post 1" },
            new Post { Id = 2, UserId = 1, Title = "Post 2", Body = "Body of Post 2" }
        };

        // GET endpoint to retrieve all posts
        app.MapGet("/posts", () => posts);

        // GET endpoint to retrieve a specific post by ID
        app.MapGet("/posts/{id:int}", (int id) =>
        {
            var post = posts.FirstOrDefault(p => p.Id == id);
            return post is not null ? Results.Ok(post) : Results.NotFound("Post not found");
        });

        // POST endpoint to add a new post
        app.MapPost("/posts", (Post newPost) =>
        {
            newPost.Id = posts.Max(p => p.Id) + 1; // Assign a new ID
            posts.Add(newPost);
            return Results.Created($"/posts/{newPost.Id}", newPost);
        });

        // PUT endpoint to update an existing post
        app.MapPut("/posts/{id:int}", (int id, Post updatedPost) =>
        {
            var postIndex = posts.FindIndex(p => p.Id == id);
            if (postIndex == -1) return Results.NotFound("Post not found");
            updatedPost.Id = id; //Ensuring Id isn't changed maliciously 
            posts[postIndex] = updatedPost;
            return Results.Ok(posts[postIndex]);
        });
        
        // DELETE endpoint to delete a post by ID
        app.MapDelete("/posts/{id:int}", (int id) =>
        {
            var post = posts.FirstOrDefault(p => p.Id == id);
            if (post is null) return Results.NotFound("Post not found");

            posts.Remove(post);
            return Results.NoContent();
        });

        app.Run();
    }
}
```

</p>
</details>
