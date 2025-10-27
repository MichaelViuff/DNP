# 08 Exercises: Authentication and Authorization

## 8.1 Creating a Login Endpoint

**Objective:**  
Create a Web API endpoint that accepts credentials and returns a JWT.

**Task:**  
Set up a new controller-based API project and implement a login endpoint.

**Steps:**

1. Create a new ASP.NET Web API project.
2. Configure JWT authentication in `Program.cs`.
3. Add a controller named AuthController.
4. Implement a POST endpoint `/api/auth/login` that accepts username and password.
5. Hardcode a user (`admin` / `password`) and validate credentials.
6. Generate a JWT with claims (e.g. name).
7. Return the JWT in a JSON response.
8. Test the endpoint using HTTPie or Postman or curl or some other API testing tool.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
//Program.cs
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;
using System.Text;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuer = true,
            ValidateAudience = true,
            ValidateLifetime = false,
            ValidateIssuerSigningKey = true,
            ValidIssuer = "your-issuer",
            ValidAudience = "your-audience",
            IssuerSigningKey = new SymmetricSecurityKey(
                Encoding.UTF8.GetBytes("SuperSecretKeyThatIsAtMinimum32CharactersLong"))
        };
    });

builder.Services.AddAuthorization();

var app = builder.Build();

app.UseAuthentication();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

```csharp
//AuthController.cs
using Microsoft.AspNetCore.Mvc;
using Microsoft.IdentityModel.Tokens;
using System.IdentityModel.Tokens.Jwt;
using System.Security.Claims;
using System.Text;

[ApiController]
[Route("api/[controller]")]
public class AuthController : ControllerBase
{
    [HttpPost("login")]
    public IActionResult Login([FromBody] LoginModel model)
    {
        if (model.Username == "admin" && model.Password == "password")
        {
            var claims = new[]
            {
                new Claim(ClaimTypes.Name, model.Username)
            };

            var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("SuperSecretKeyThatIsAtMinimum32CharactersLong"));
            var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

            var token = new JwtSecurityToken(
                issuer: "your-issuer",
                audience: "your-audience",
                claims: claims,
                expires: DateTime.Now.AddMinutes(30),
                signingCredentials: creds);

            return Ok(new { token = new JwtSecurityTokenHandler().WriteToken(token) });
        }

        return Unauthorized();
    }
}

public class LoginModel
{
    public string Username { get; set; }
    public string Password { get; set; }
}
```

```http
POST http://localhost:5025/api/auth/login
Content-Type: application/json

{
  "username": "admin",
  "password": "password"
}

```

</p>
</details>
</blockquote>

## 8.2 Accessing a Protected Endpoint with JWT

**Objective:**  
Use the JWT from the login endpoint to access a protected resource.

**Task:**  
Create a protected endpoint that requires a valid JWT.

**Steps:**

1. Add a new controller named ProtectedController.
2. Implement a GET endpoint `/api/protected/data`.
3. Add the `[Authorize]` attribute to the endpoint.
4. Use HTTPie or Postman or curl or some other API testing tool to call the endpoint with the JWT from Exercise 8.1 in the `Authorization: Bearer <token>` header.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
// ProtectedController.cs
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;

[ApiController]
[Route("api/[controller]")]
public class ProtectedController : ControllerBase
{
    [HttpGet("secure-data")]
    [Authorize]
    public IActionResult GetSecureData()
    {
        return Ok("This is protected data, accessible only to authenticated users.");
    }
}
```

```csharp
GET http://localhost:5025/api/protected/secure-data
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

</p>
</details>
</blockquote>

## 8.3 Creating a Blazor Login Form

**Objective:**  
Build a Blazor frontend that allows users to log in.

**Task:**  
Create a login form that sends credentials to the Web API and displays the received JWT.

**Steps:**

1. Create a new Blazor project.
2. Add a login page with input fields for username and password and an output for the JWT.
3. Create a class `AuthService` that can use `HttpClient` to send a POST request to `/api/auth/login` with the credentials from the input fields.
4. Receive the JWT in the response, and return it.
5. Display the JWT in the output on the page.
6. Update `Program.cs` to inject the `AuthService`.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```razor
//Login.razor
@page "/login"
@inject AuthService AuthService
@rendermode InteractiveServer

<h3>Login</h3>

<input @bind="username" placeholder="Username" />
<input @bind="password" placeholder="Password" type="password" />
<button @onclick="LoginMethod">Login</button>

@if (!string.IsNullOrEmpty(jwt))
{
    <p><strong>JWT:</strong></p>
    <textarea rows="6" style="width:100%">@jwt</textarea>
}

@code {
    private string username;
    private string password;
    private string jwt;

    private async Task LoginMethod()
    {
        jwt = await AuthService.LoginAndReturnToken(username, password);
    }
}
```

```csharp
//AuthService.cs
public class AuthService
{
    private readonly HttpClient _http;

    public AuthService(HttpClient http)
    {
        _http = new HttpClient
        {
            BaseAddress = new Uri("https://localhost:5025")
        };
    }

    public async Task<string> LoginAndReturnToken(string username, string password)
    {
        var response = await _http.PostAsJsonAsync("api/auth/login", new { Username = username, Password = password });
        if (!response.IsSuccessStatusCode) return "Login failed.";

        var result = await response.Content.ReadFromJsonAsync<TokenResponse>();
        return result?.Token ?? "No token received.";
    }
}

public class TokenResponse
{
    public string Token { get; set; }
}
```

```csharp
//Update Program.cs
builder.Services.AddScoped<AuthService>();
```

</p>
</details>
</blockquote>

## 8.4 Creating a Protected Page in Blazor

**Objective:**  
Restrict access to a page based on authentication.

**Task:**  
Create a page that is only accessible to authenticated users.

**Steps:**

1. Add a new page `/secure` in Blazor.
2. Create an implementation of `AuthenticationStateProvider` that can receive a JWT and create a `ClaimsPrincipal`.
3. Inject this in `Program.cs`.
4. Use `<AuthorizeView>` to conditionally render content:
   - While not authenticated: Show an input field to take the JWT and send it to `AuthenticationStateProvider`.
   - While authenticated: Show a message that tells the user that he is authenticated.
5. Remeber to wrap Router with `CascadingAuthenticationState`.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```razor
//Secure.razor
@page "/secure"
@using System.Security.Claims
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthProvider

<AuthorizeView>
    <Authorized>
        <h3>Welcome!</h3>
        <p>You have access to the secure area.</p>
        <button class="btn btn-outline-secondary" @onclick="SignOut">Sign out</button>
    </Authorized>

    <NotAuthorized>
        <h3>Log in with JWT</h3>
        <p>Insert your JWT-token</p>

        <div class="mb-3">
            <label for="jwtBox" class="form-label">JWT</label>
            <textarea id="jwtBox"
                      class="form-control"
                      @bind="jwt"
                      rows="5"
                      placeholder="eyJhbGciOi..."></textarea>
        </div>

        <button class="btn btn-primary" @onclick="SignIn">Sign In</button>

        @if (!string.IsNullOrWhiteSpace(error))
        {
            <div class="text-danger mt-2">@error</div>
        }
    </NotAuthorized>
</AuthorizeView>

@code {
    private string jwt = string.Empty;
    private string? error;

    private async Task SignIn()
    {
        try
        {
            if (AuthProvider is TokenAuthenticationStateProvider provider)
            {
                await provider.SignIn(jwt);
                jwt = string.Empty;
                error = null;
            }
            else
            {
                error = "AuthenticationStateProvider not an instance of TokenAuthenticationStateProvider.";
            }
        }
        catch (Exception ex)
        {
            error = $"Invalid token: {ex.Message}";
        }
    }

    private async Task SignOut()
    {
        if (AuthProvider is TokenAuthenticationStateProvider provider)
        {
            await provider.SignOut();
        }
    }
}
```

```csharp
//Program.cs
using Microsoft.AspNetCore.Components.Authorization;

builder.Services.AddAuthorizationCore();
builder.Services.AddScoped<AuthenticationStateProvider, TokenAuthenticationStateProvider>();
```

```csharp
//TokenAuthenticationStateProvider.cs
using System.Security.Claims;
using System.IdentityModel.Tokens.Jwt;
using Microsoft.AspNetCore.Components.Authorization;

public class TokenAuthenticationStateProvider : AuthenticationStateProvider
{
    private AuthenticationState _state = new(new ClaimsPrincipal(new ClaimsIdentity()));

    public override Task<AuthenticationState> GetAuthenticationStateAsync() => Task.FromResult(_state);

    public Task SignIn(string jwt)
    {
        var claims = ParseClaimsFromJwt(jwt);
        var identity = new ClaimsIdentity(claims, authenticationType: "jwt", nameType: "name", roleType: "role");
        var user = new ClaimsPrincipal(identity);

        _state = new AuthenticationState(user);
        NotifyAuthenticationStateChanged(Task.FromResult(_state));
        return Task.CompletedTask;
    }

    public Task SignOut()
    {
        _state = new AuthenticationState(new ClaimsPrincipal(new ClaimsIdentity()));
        NotifyAuthenticationStateChanged(Task.FromResult(_state));
        return Task.CompletedTask;
    }

    private static IEnumerable<Claim> ParseClaimsFromJwt(string jwt)
    {
        var handler = new JwtSecurityTokenHandler();
        var token = handler.ReadJwtToken(jwt);
        return token.Claims;
    }
}
```

```razor
//App.razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(App).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    @* Here you can have global NotAuthorized-text, but in our /secure we handle it locally *@
                    <p>No Access!.</p>
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <h1>Page not found</h1>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```

</p>
</details>
</blockquote>

## 8.5 Conditional Rendering Based on Claims

**Objective:**  
Use claims in the JWT to control what content is shown.

**Task:**  
Render different UI elements based on user claims.

**Steps:**

1. Modify the generated JWT in Exercise 8.1 to include a claim (e.g. `level: 4`).
2. Create a new page, `/level` for instance, where you can go after having been authenticated in the previous exercise.
3. Use `AuthorizeView` or custom logic to show/hide content based on these claims on this page.

<blockquote>
<details>
<summary>Display solution...</summary>
<p>

```csharp
//AuthController.cs
using System.Security.Claims;
using System.Text;
using System.IdentityModel.Tokens.Jwt;
using Microsoft.AspNetCore.Mvc;
using Microsoft.IdentityModel.Tokens;

[HttpPost("login")]
public IActionResult Login([FromBody] LoginModel model)
{
    if (model.Username == "admin" && model.Password == "password")
    {
        var claims = new[]
        {
            new Claim(ClaimTypes.Name, model.Username),
            new Claim("level", "4", ClaimValueTypes.Integer) // <-- added claim
        };

        var key = new SymmetricSecurityKey(
            Encoding.UTF8.GetBytes("SuperSecretKeyThatIsAtMinimum32CharactersLong"));
        var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

        var token = new JwtSecurityToken(
            issuer: "your-issuer",
            audience: "your-audience",
            claims: claims,
            expires: DateTime.UtcNow.AddMinutes(30),
            signingCredentials: creds);

        var jwt = new JwtSecurityTokenHandler().WriteToken(token);
        return Ok(new { token = jwt });
    }

    return Unauthorized();
}
```

```csharp
//Program.cs
builder.Services.AddAuthorizationCore(options =>
{
    options.AddPolicy("Level4OrHigher", policy =>
        policy.RequireAssertion(ctx =>
        {
            var c = ctx.User.FindFirst("level")?.Value;
            return int.TryParse(c, out var lvl) && lvl >= 4;
        }));
});
```

```razor
//Level.razor
@page "/level"
@using Microsoft.AspNetCore.Components.Authorization
@using System.Security.Claims

<h3>Level-based access (policy)</h3>

<AuthorizeView Policy="Level4OrHigher">
    <Authorized Context="ctx">
        @{
            var level = GetLevel(ctx.User);
        }
        <p>You are authenticated. Your <code>level</code> is <strong>@level</strong>.</p>
        <div class="alert alert-success">
            You meet the <code>Level4OrHigher</code> policy and can access this content.
        </div>
    </Authorized>

    <NotAuthorized>
    <!-- We distinguish between "not logged in" and "logged in, but not sufficient level" -->
        <AuthorizeView>
            <Authorized Context="ctx">
                @{
                    var level = GetLevel(ctx.User);
                }
                <p>You are authenticated. Your <code>level</code> is <strong>@level</strong>.</p>
                <div class="alert alert-warning">
                    You do not meet the <code>Level4OrHigher</code> policy (requires level ≥ 4).
                </div>
            </Authorized>
            <NotAuthorized>
                <div class="alert alert-secondary">
                    You are not authenticated. Please log in elsewhere to get a token with a <code>level</code> claim.
                </div>
            </NotAuthorized>
        </AuthorizeView>
    </NotAuthorized>
</AuthorizeView>

@code {
    private static int GetLevel(ClaimsPrincipal user) =>
        int.TryParse(user.FindFirst("level")?.Value, out var lvl) ? lvl : 0;
}
```

</p>
</details>
</blockquote>
