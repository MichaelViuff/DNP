# 05 Exercises: REST and Controllers

## 5.1 Setting Up a Basic Controller-based WEB API

**Objective:**  
Create a controller based WEB API using ASP.NET Core that will act as the foundation for a small application.

**Task:**  
Set up a new controller based API project and create a simple GET endpoint.

**Steps:**

1. Create a new ASP.NET Web project
2. Select "Web API" template
3. Under "advanced settings" all boxes must be unchecked, except "UseControllers" which must be checked
4. Delete the `WeatherForecastController.cs` file, and the `WeatherForecast.cs` file
5. Leave the Swagger generation as is, or delete it
6. In the "controllers" folder, create a new `PostController` class. 
7. In the `PostController` class, map a GET request to your "/" endpoint and have it output "Hello, Controller-based API!".
8. Run your `Program.cs` (this probably opens the Swagger index if you left that part in step 5)
9. Open your browser and navigate to http://localhost:5142/ and verify that your output is displayed

<blockquote>
<details>
<summary>Display solution...</summary>
<p>



```csharp
using Microsoft.AspNetCore.Mvc;

namespace ControllerBasedWebAPI.Controllers;

[ApiController]
[Route("/")]
public class PostController
{
    // Test GET endpoint
    [HttpGet("/")]
    public IActionResult GetWelcomeMessage()
    {
        return Ok("Hello, Controller-based API!");
    }
}
```

```csharp
namespace ControllerBasedWebAPI;

public class Program
{
    public static void Main(string[] args)
    {
        var builder = WebApplication.CreateBuilder(args);

        // Add services to the container.

        builder.Services.AddControllers();
        // Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
        builder.Services.AddEndpointsApiExplorer();
        builder.Services.AddSwaggerGen();

        var app = builder.Build();

        // Configure the HTTP request pipeline.
        if (app.Environment.IsDevelopment())
        {
            app.UseSwagger();
            app.UseSwaggerUI();
        }

        app.UseHttpsRedirection();

        app.UseAuthorization();


        app.MapControllers();

        app.Run();
    }
}
```

</p>
</details>
</blockquote>



## 5.2 Create mapping using controllers

**Objective:**  
Redo the exercises from last session [4.7 - 4.11](../04%20Web%20API/README.md) to use controllers.

**Task:**  
Extend your controller based API project and set up the same endpoints as in the exercises from.

**Steps:**

1. Open the `PostController.cs` file.
2. Create new endpoints as needed.
3. Run your program.
4. Test your endpoints in your browser.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
namespace ControllerBasedWebAPI;

public class Post
{
    public int Id { get; set; }
    public int UserId { get; set; }
    public string Title { get; set; }
    public string Body { get; set; }
}
```

```csharp
using Microsoft.AspNetCore.Mvc;
using ControllerBasedWebAPI;

[ApiController]
[Route("[controller]")]
public class PostsController : ControllerBase
{
    // In-memory list of posts (for simplicity)
    private static List<Post> posts = new List<Post>
    {
        new Post { Id = 1, UserId = 1, Title = "Post 1", Body = "Body of Post 1" },
        new Post { Id = 2, UserId = 1, Title = "Post 2", Body = "Body of Post 2" }
    };

    // Test GET endpoint
    [HttpGet("/")]
    public IActionResult GetWelcomeMessage()
    {
        return Ok("Hello, Controller-based API!");
    }

    // GET endpoint to retrieve all posts
    [HttpGet]
    public ActionResult<IEnumerable<Post>> GetAllPosts()
    {
        return Ok(posts);
    }

    // GET endpoint to retrieve a specific post by ID
    [HttpGet("{id:int}")]
    public IActionResult GetPostById(int id)
    {
        var post = posts.FirstOrDefault(p => p.Id == id);
        return post is not null ? Ok(post) : NotFound("Post not found");
    }

    // POST endpoint to add a new post
    [HttpPost]
    public IActionResult CreatePost(Post newPost)
    {
        newPost.Id = posts.Max(p => p.Id) + 1; // Assign a new ID
        posts.Add(newPost);
        return CreatedAtAction(nameof(GetPostById), new { id = newPost.Id }, newPost);
    }

    // PUT endpoint to update an existing post
    [HttpPut("{id:int}")]
    public IActionResult UpdatePost(int id, Post updatedPost)
    {
        var postIndex = posts.FindIndex(p => p.Id == id);
        if (postIndex == -1)
        {
            return NotFound("Post not found");
        }

        updatedPost.Id = id; // Ensure the ID remains unchanged
        posts[postIndex] = updatedPost;
        return Ok(posts[postIndex]);
    }

    // DELETE endpoint to delete a post by ID
    [HttpDelete("{id:int}")]
    public IActionResult DeletePost(int id)
    {
        var post = posts.FirstOrDefault(p => p.Id == id);
        if (post is null)
        {
            return NotFound("Post not found");
        }

        posts.Remove(post);
        return NoContent();
    }
}
```

```csharp
namespace ControllerBasedWebAPI;

public class Program
{
    public static void Main(string[] args)
    {
        var builder = WebApplication.CreateBuilder(args);

        // Add services to the container.

        builder.Services.AddControllers();
        // Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
        builder.Services.AddEndpointsApiExplorer();
        builder.Services.AddSwaggerGen();

        var app = builder.Build();

        // Configure the HTTP request pipeline.
        if (app.Environment.IsDevelopment())
        {
            app.UseSwagger();
            app.UseSwaggerUI();
        }

        app.UseHttpsRedirection();

        app.UseAuthorization();


        app.MapControllers();

        app.Run();
    }
}
```

</p>
</details>
</blockquote>

## 5.3 - Implementing REST Principles: Uniform Interface

**Objective:**  
Now we will focus on the REST Principles for our project. 

The first principle is the "Uniform Interface" - we must ensure that actions are consistent across the entire API.

**Task:**  
Ensure your project uses a Uniform Interface.

**Steps:**

1. Validate your existing endpoints structure

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

If everything is set up as in the solution for Exercise 4.2, then you can argue that you have a uniform interface.
GET and POST happen at the /posts endpoint
PUT, DELETE (and GET for specific posts) happens at the /posts/{id} endpoint

</p>
</details>
</blockquote>


## 5.4 Implementing REST Principles: Client/Server

**Objective:**  
The second principle is the "Client/Server" - we must ensure that the client and server are independent and decoupled.

**Task:**  
Build a simple client-side application (C# or HTML) that interacts with our API, fetching and displaying data about products.

**Steps:**

1. Run your program so the server is available.
2. Create a client-side application, either in C# or Java, or a simple HTML page using a form element can do it.
3. Run your client-side application and ensure that you can access your API.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Test root endpoint</title>
</head>
<body>
    <h1>Test root endpoint</h1>

    <form action="http://localhost:5113/" method="GET">

        <input type="submit" value="Contact root endpoint">
    </form>
</body>
</html>
```

</p>
</details>
</blockquote>

## 5.5 Implementing REST Principles: Stateless

**Objective:**  
The third principle is the "Stateless" - we must ensure that the server does not store session data, and that all requests are independent.

**Task:**  
Create a simple authorization set up, where a user can request access, get the super secret handshake in a response, and then use the super secret handshake to access a guarded endpoint.

**Steps:**

1. Open the `PostController.cs` file.
2. Create a new endpoint to get the super secret handshake.
3. Create a guarded endpoint, that requires the super secret handshake to access (must be part of the request)
3. Run your program.
4. Test your newly created endpoints.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
...
    // The super secret handshake
    private const string SuperSecretHandshake = "password123";
    
    // This endpoint returns the super secret handshake
    [HttpGet("/getAccess")]
    public IActionResult GetAccess()
    {
        // Return the super secret handshake in the response
        return Ok(new { handshake = SuperSecretHandshake });
    }
    
    // This endpoint requires the super secret handshake to access
    [HttpGet("/guardedEndpoint")]
    public IActionResult GuardedEndpoint([FromQuery] string handshake)
    {
        // Check if the handshake is correct
        if (handshake == SuperSecretHandshake)
        {
            // If the handshake is correct, allow access
            return Ok("Access granted: Handshake is valid.");
        }

        // If the handshake is incorrect, deny access
        return Unauthorized("Access denied: Invalid handshake.");
    }
...
```
</p>
</details>
</blockquote>

## 5.6 Implementing REST Principles: Cacheable

**Objective:**  
The fourth principle is the "Cacheable" - we must use caching when possible, so that the server doesn't always query or dataset, but reuses older data that is probably still valid.

**Task:**  
Cache the result when finding a specific post, so the "database" (in our instance just the in-memory list) is not queried again if the same post is requested within 60 seconds.

**Steps:**

1. Open the `PostController.cs` file.
2. Modify your `GetPostById` endpoint, so that is stores the result for 60 seconds (you can either use the ASP.NET caching, or create your own)
3. Run your program.
4. Test your endpoint in your browser and verify that you are receiving cached results when expected.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
...
    // Dictionary to store cached posts and their timestamps
    private static Dictionary<int, (Post post, DateTime timestamp)> postCache = new Dictionary<int, (Post, DateTime)>();

    // Cache duration: 60 seconds
    private static readonly TimeSpan cacheDuration = TimeSpan.FromSeconds(60);

    // GET /posts/{id:int}
    [HttpGet("{id:int}")]
    public IActionResult GetPostById(int id)
    {
        // Check if the post is cached
        if (postCache.ContainsKey(id))
        {
            var (cachedPost, cacheTime) = postCache[id];

            // Check if the cached post is still valid (within cacheDuration)
            if (DateTime.Now - cacheTime < cacheDuration)
            {
                return Ok(new
                {
                    cached = true,
                    post = cachedPost
                });
            }
        }

        // If post is not cached or cache is expired, retrieve from original list
        var post = posts.FirstOrDefault(p => p.Id == id);
        if (post is null)
        {
            return NotFound("Post not found");
        }

        // Store the post in the cache with the current timestamp
        postCache[id] = (post, DateTime.Now);

        return Ok(new
        {
            cached = false,
            post = post
        });
    }
```
</p>
</details>
</blockquote>

## 5.7 Implementing REST Principles: Layered System

**Objective:**  
The fifth principle is the "Layered System" - our Web API must be composed internally by layers, where each layer knows nothing about layers beyond the immediate layers they interact with.

**Task:**  
Delegate the caching that you implemented in the previous exercise, to a seperate layer in the Web API.

This is a ideal candidate for Dependency Injection, using `AddSingleton` method on our `WebApplication` builder.

**Steps:**

1. Open the `PostController.cs` file.
2. Extract the caching functionality and move it to a separate file.
3. Create a constructor that takes an instance of the new caching class you just created and assigns it to a global variable.
4. Open the `Program.cs` file and add the necessary dependency injection for our newly created caching service.
5. Run your program.
6. Test your endpoint in your browser and verify that you are receiving cached results when expected.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
namespace ControllerBasedWebAPI;

public class CachingService
{
    // Dictionary to store cached posts and their timestamps
    private static Dictionary<int, (Post post, DateTime timestamp)> postCache = new Dictionary<int, (Post, DateTime)>();

    // Cache duration: 60 seconds
    private static readonly TimeSpan cacheDuration = TimeSpan.FromSeconds(60);

    public Post? GetCachedPost(int id)
    {
        // Check if the post is cached
        if (postCache.ContainsKey(id))
        {
            var (cachedPost, cacheTime) = postCache[id];

            // Check if the cached post is still valid (within cacheDuration)
            if (DateTime.Now - cacheTime < cacheDuration)
            {
                return cachedPost;
            }
        }

        // Return null if no valid cached post is found
        return null;
    }

    // Store the post in the cache with the current timestamp
    public void CachePost(int id, Post post)
    {
        postCache[id] = (post, DateTime.Now);
    }
}
```

```csharp
...
    // GET /posts/{id:int}
    [HttpGet("{id:int}")]
    public IActionResult GetPostById(int id)
    {
        // Check if the post is cached
        var post = _postCache.GetCachedPost(id);

        // If post is not cached or cache is expired, retrieve from original list
        if (post is null)
        {
            post = posts.FirstOrDefault(p => p.Id == id);
            if (post is null)
            {
                return NotFound("Post not found");
            }
            
            // Store the post in the cache
            _postCache.CachePost(id, post);
            
            return Ok(new
            {
                cached = false,
                post
            });
        }

        return Ok(new
        {
            cached = true,
            post
        });
    }
...
```

```csharp
...
    //Dependency Injection
    builder.Services.AddSingleton<CachingService>();  // Singleton because we want to persist cache across requests
...
```

</p>
</details>
</blockquote>

## 5.8 Implementing REST Principles: Code on Demand

**Objective:**  
The sixth principle is the "Layered System" - we can send code as part of our response, that the client can then execute.

**Task:**  
Create an endpoint, that returns some JavaScript code that is executed in the browser.

**Steps:**

1. Open the `PostController.cs` file.
2. Create new endpoints as needed.
3. Run your program so the server is available.
4. Create a HTML file that accesses your endpoint, retrives the code, and executes it. (you may need to create a CORS policy and use it in your `Program.cs` for it to accept this request, and you may have to use HTTPS instead of HTTP)
5. Test your endpoints in your browser using your HTML file.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
...

// Define a CORS policy
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAll", policy =>
    {
        policy.AllowAnyOrigin()     // Allow requests from any origin
              .AllowAnyMethod()     // Allow any HTTP method (GET, POST, etc.)
              .AllowAnyHeader();    // Allow any headers
    });
});

...

// Use the CORS policy
app.UseCors("AllowAll");
...
```

```csharp
...
    // GET /code/getCode
    [HttpGet("/getCode")]
    public IActionResult GetCode()
    {
        // Return JavaScript code to be executed by the client
        string jsCode = "alert('Hello from the server! This is Code on Demand.');";
        return Content(jsCode, "application/javascript");
    }
...
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code on Demand Example</title>
</head>
<body>
    <h1>Click the Button to Fetch and Run Code from the Server</h1>
    <button id="runCode">Run Code from Server</button>

    <script>
        // Button click event to fetch JavaScript from the server and execute it
        document.getElementById('runCode').addEventListener('click', async function() {
            try {
                // Fetch the JavaScript code from the server
                const response = await fetch('https://localhost:7067/getCode');
                const jsCode = await response.text();

                // Execute the fetched JavaScript code
                eval(jsCode);
            } catch (error) {
                console.error('Error fetching or executing code:', error);
            }
        });
    </script>
</body>
</html>
```


</p>
</details>
</blockquote>


## 5.9 Implementing REST Principles: HATEOAS

**Objective:**  
The HATEOAS principle, while not widely adopted, makes discovery of API's easier. 

For this exercise, we we need to design our responses in such a way that they include hypermedia links guiding the client on what they can do next.

**Task:**  
Update your endpoints to be HATEOAS compliant.

To do so, they must return a list related links for each endpoint.

You can generate the links easier, if you name your endpoints using something like this:

```csharp
// GET endpoint to retrieve all posts
[HttpGet(Name = "GetAllPosts")]
public ActionResult<IEnumerable<Post>> GetAllPosts()
{
    return Ok(posts);
}
```

**Steps:**

1. Open the `PostController.cs` file.
2. Update your endpoints as needed.
3. Run your program.
4. Test your endpoints and ensure they return HATEOAS links.
5. Consider whether you want to implement these links in a larger scale system.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
    namespace ControllerBasedWebAPI;

    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);

            // Define a CORS policy
            builder.Services.AddCors(options =>
            {
                options.AddPolicy("AllowAll", policy =>
                {
                    policy.AllowAnyOrigin()     // Allow requests from any origin
                        .AllowAnyMethod()     // Allow any HTTP method (GET, POST, etc.)
                        .AllowAnyHeader();    // Allow any headers
                });
            });
            
            
            
            //Dependency Injection
            builder.Services.AddSingleton<CachingService>();  // Singleton because we want to persist cache across requests
            
            // Add services to the container.

            builder.Services.AddControllers();
            // Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
            builder.Services.AddEndpointsApiExplorer();
            builder.Services.AddSwaggerGen();

            var app = builder.Build();

            // Configure the HTTP request pipeline.
            if (app.Environment.IsDevelopment())
            {
                app.UseSwagger();
                app.UseSwaggerUI();
            }

            app.UseHttpsRedirection();

            app.UseAuthorization();

            // Use the CORS policy
            app.UseCors("AllowAll");

            app.MapControllers();

            app.Run();
        }
    }
```

You often don't want to implement this is a larger system, as it is hard to maintain, and adds little value (assuming you document your API instead)

</p>
</details>
</blockquote>