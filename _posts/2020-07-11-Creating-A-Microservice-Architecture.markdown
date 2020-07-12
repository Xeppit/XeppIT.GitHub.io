---
layout: post
post_identifier: 2020-07-11-Creating-A-Microservice-Architecture
title:  "Creating A Microservice Architecture"
date:   2020-07-09 16:02:24 +0100
categories: Tutorial
excerpt: loremNostrud est anim Lorem ea pariatur anim consequat sunt laborum pariatur et et
---

{% assign post_identifier = page.post_identifier %}


## Description/Introduction
Get Started with .NET Core Identity Server and Securing your Applications!

## References
* [GitHub Pages](https://pages.github.com/)

## Goals
* Develop Web API's using .NET Core
* Security using Identity Server 4

## Perquisites
* Dotnet SDK
* Postman
* Webbrowser Extention "JSON Formatter"

## Creating Project Structure

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Project Structure" %}

### Using the console create the project structure
```console
mkdir MicroserviceArchitecture
cd MicroserviceArchitecture
dotnet new sln -n MicroserviceArchitecture
dotnet new is4aspid -n MicroserviceArchitecture.IdentityServer
dotnet new blazorwasm -au individual -n MicroserviceArchitecture.Blazor
dotnet new webapi -n MicroserviceArchitecture.GatewayApi
dotnet new webapi -n MicroserviceArchitecture.MicroServiceApi1
dotnet new webapi -n MicroserviceArchitecture.MicroServiceApi2
dotnet new webapi -n MicroserviceArchitecture.MicroServiceApi3
```
### Add the projects to the solution

```console
dotnet sln add MicroserviceArchitecture.IdentityServer
dotnet sln add MicroserviceArchitecture.Blazor
dotnet sln add MicroserviceArchitecture.GatewayApi
dotnet sln add MicroserviceArchitecture.MicroServiceApi1
dotnet sln add MicroserviceArchitecture.MicroServiceApi2
dotnet sln add MicroserviceArchitecture.MicroServiceApi3
```

### Add To Source Control
Add the solution to to github using visual studio

## Configuring the Project

Https Port: 5000 -MicroserviceArchitecture.IdentityServer

Https Port: 5001 -MicroserviceArchitecture.Blazor

Https Port: 5002 -MicroserviceArchitecture.GatewayApi

Https Port: 5003 -MicroserviceArchitecture.MicroServiceApi1

Https Port: 5004 -MicroserviceArchitecture.MicroServiceApi2

Https Port: 5005 -MicroserviceArchitecture.MicroServiceApi3

### MicroserviceArchitecture.IdentityServer/Properties/LaunchSettings.json
```json
{
  "profiles": {
    "SelfHost": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "https://localhost:5000"
    }
  }
}
```
### MicroserviceArchitecture.Blazor/Properties/LaunchSettings.json
```json
{
  "profiles": {
    "MicroserviceArchitecture.Blazor": {
      "commandName": "Project",
      "launchBrowser": true,
      "inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}",
      "applicationUrl": "https://localhost:5001",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```
### MicroserviceArchitecture.GatewayApi/Properties/LaunchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "MicroserviceArchitecture.GatewayApi": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:5002",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```
### MicroserviceArchitecture.MicroServiceApi1/Properties/LaunchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "MicroserviceArchitecture.MicroServiceApi1": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:5003",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```
### MicroserviceArchitecture.MicroServiceApi2/Properties/LaunchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "MicroserviceArchitecture.MicroServiceApi2": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:5004",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```
### MicroserviceArchitecture.MicroServiceApi3/Properties/LaunchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "MicroserviceArchitecture.MicroServiceApi3": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:5005",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## Configure IdentityServer
### MicroserviceArchitecture.IdentityServer/CustomIdentityResourceRoleClaim.cs
Creat a new class

```csharp
using IdentityModel;
using IdentityServer4.Models;

namespace MicroserviceArchitecture.IdentityServer
{
    public class CustomIdentityResourcesProfile : IdentityResources.Profile
    {
        public CustomIdentityResourcesProfile()
        {
            this.UserClaims.Add(JwtClaimTypes.Role);
        }
    }
}
```

### MicroserviceArchitecture.IdentityServer/Config.cs
```csharp
public static IEnumerable<IdentityResource> IdentityResources =>
    new IdentityResource[]
    {
        new IdentityResources.OpenId(),
        //new IdentityResources.Profile(),
        //overridden below
        new CustomIdentityResourcesProfile(), // New Line
        new IdentityResources.Email(),       
    };
```

```csharp
public static IEnumerable<ApiScope> ApiScopes =>
    new ApiScope[]
    {
        new ApiScope("gatewayapi", "The Gateway Api", new[] { JwtClaimTypes.Role }), // Modified Line
        new ApiScope("microserviceapi1", "Microservice Api 1"), // New Line
        new ApiScope("microserviceapi2", "Microservice Api 2"), // New Line
        new ApiScope("microserviceapi3", "Microservice Api 3")  // New Line
    };
```

### MicroserviceArchitecture.IdentityServer/Config.cs
```csharp
public static IEnumerable<Client> Clients =>
    // New Client
    new Client[]
    {
        // interactive client using code flow + pkce
        new Client
        {
            ClientId = "blazor",
            AllowedGrantTypes = GrantTypes.Code,
            AllowedCorsOrigins = {"https://localhost:5001"},
            RequirePkce = true,
            RequireClientSecret = false,
            RedirectUris = {"https://localhost:5001/authentication/login-callback"},
            PostLogoutRedirectUris = {"https://localhost:5001/"},
            AllowedScopes = {"openid", "profile", "email", "gatewayapi"}
        },
    };
```

### MicroserviceArchitecture.IdentityServer/SeedData.cs
The database need to be re-seeded after this modification
```csharp
if (alice == null)
{
    alice = new ApplicationUser
    {
        UserName = "alice",
        Email = "AliceSmith@email.com",
        EmailConfirmed = true,
    };
    var result = userMgr.CreateAsync(alice, "Pass123$").Result;
    if (!result.Succeeded)
    {
        throw new Exception(result.Errors.First().Description);
    }
    result = userMgr.AddClaimsAsync(alice, new Claim[]{
        new Claim(JwtClaimTypes.Name, "Alice Smith"),
        new Claim(JwtClaimTypes.GivenName, "Alice"),
        new Claim(JwtClaimTypes.FamilyName, "Smith"),
        new Claim(JwtClaimTypes.WebSite, "http://alice.com"),
        new Claim(JwtClaimTypes.Role, "Admin") // New Line
    }).Result;
    if (!result.Succeeded)
    {
        throw new Exception(result.Errors.First().Description);
    }
    Log.Debug("alice created");
}
else
{
    Log.Debug("alice already exists");
}
```

### MicroserviceArchitecture.IdentityServer/Quickstart/Account/AccountOptions.cs
Redirect back to blazor after logout

```csharp
public static bool ShowLogoutPrompt = false;                    // Modified Line
public static bool AutomaticRedirectAfterSignOut = true;        // Modified Line
```

## Configure Blazor
Using nuget manager add "Microsoft.Extension.Http" to MicroserviceArchitecture.Blazor

### MicroserviceArchitecture.Blazor/wwwroot/appsettings.json
```json
{
  "oidc": {
    "Authority": "https://localhost:5000/",
    "ClientId": "blazor",
    "DefaultScopes": [
      "openid",
      "profile",
      "email",
      "gatewayapi"
    ],
    "PostLogoutRedirectUri": "/",
    "ResponseType": "code"
  }
}
```

### MicroserviceArchitecture.Blazor/Program.cs

```csharp
public static async Task Main(string[] args)
{
    var builder = WebAssemblyHostBuilder.CreateDefault(args);
    builder.RootComponents.Add<App>("app");
    
    builder.Services.AddOidcAuthentication(options =>
    {
        builder.Configuration.Bind("oidc", options.ProviderOptions);    // Modified Line
        options.UserOptions.RoleClaim = "role";                         // New Line
    });

    builder.Services.AddHttpClient("api")                               // New Line
        .AddHttpMessageHandler(sp =>                                    // New Line
        {                                                               // New Line
            var handler = sp.GetService<AuthorizationMessageHandler>()  // New Line
                .ConfigureHandler(                                      // New Line
                    authorizedUrls: new[] { "https://localhost:5004" }, // New Line
                    scopes: new[] { "testapi" });                       // New Line
            return handler;                                             // New Line
        });                                                             // New Line

    builder.Services.AddScoped(                                         // New Line
        sp => sp.GetService<IHttpClientFactory>().CreateClient("api")); // New Line
    
    await builder.Build().RunAsync();
}
```

### MicroserviceArchitecture.Blazor/Pages/Claims.Razor
New File
```html
@page "/claims"
<AuthorizeView>
    <Authorized>
        <h2>
            Hello @context.User.Identity.Name,
            here's the list of your claims:
        </h2>
        <ul>
            @foreach (var claim in context.User.Claims)
            {
                <li><b>@claim.Type</b>: @claim.Value</li>
            }
        </ul>
    </Authorized>
    <NotAuthorized>
        <p>I'm sorry, I can't display anything until you log in</p>
    </NotAuthorized>
</AuthorizeView>
```

### MicroserviceArchitecture.Blazor/Shared/NavMenu.razor
```html
<div class="@NavMenuCssClass" @onclick="ToggleNavMenu">
    <ul class="nav flex-column">
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="" Match="NavLinkMatch.All">
                <span class="oi oi-home" aria-hidden="true"></span> Home
            </NavLink>
        </li>
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="counter">
                <span class="oi oi-plus" aria-hidden="true"></span> Counter
            </NavLink>
        </li>
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="fetchdata">
                <span class="oi oi-list-rich" aria-hidden="true"></span> Fetch data
            </NavLink>
        </li>
        <li class="nav-item px-3">                                              <!--New Line-->
            <NavLink class="nav-link" href="claims">                            <!--New Line-->
                <span class="oi oi-list-rich" aria-hidden="true"></span> Claims <!--New Line-->
            </NavLink>                                                          <!--New Line-->
        </li>                                                                   <!--New Line-->
    </ul>
</div>
```

### MicroserviceArchitecture.Blazor/Pages/FetchData.razor
``` csharp
@page "/fetchdata"
@using Microsoft.AspNetCore.Authorization                                       //New Line
@inject HttpClient Http
@attribute [Authorize]                                                          //New Line

<AuthorizeView Roles="Admin">                                                   //New Line
    <Authorized>                                                                //New Line
        You can only see this if you're an admin or superuser.                  //New Line
    </Authorized>                                                               //New Line
</AuthorizeView>                                                                //New Line
```

```csharp
@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        // Modified Line
        forecasts = await Http.GetFromJsonAsync<WeatherForecast[]>("https://localhost:5004/WeatherForecast");
    }

    public class WeatherForecast
    {
        public DateTime Date { get; set; }

        public int TemperatureC { get; set; }

        public string Summary { get; set; }

        public int TemperatureF { get; set; }                                   // Modified Line
    }
}
```

## Configering the Gateway Api
Using nuget manager add "Microsoft.AspNetCore.Authentication.JwtBearer" to MicroserviceArchitecture.Api

### MicroserviceArchitecture.GatewayApi/Startup.cs
```csharp
services.AddAuthentication("Bearer")
    .AddJwtBearer("Bearer", options =>
    {
        options.Authority = "https://localhost:5000";
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateAudience = false
        };
    });
```

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseCors(config =>
    {
        config.AllowAnyOrigin();
        config.AllowAnyMethod();
        config.AllowAnyHeader();
    });

    app.UseHttpsRedirection();
    app.UseRouting();
    app.UseAuthentication(); // New Line
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

### MicroserviceArchitecture.GatewayApi/Controllers/WeatherForecastController.cs
```csharp
[Authorize]                                                                     //New Line
[ApiController]
[Route("[controller]")]
```