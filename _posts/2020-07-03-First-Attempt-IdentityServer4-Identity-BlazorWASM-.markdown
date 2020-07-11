---
layout: post
post_identifier: 2020-07-03-Identity-Server-4-Course
title:  "First Attempt IdentityServer4 Identity Blazor WASM"
date:   2020-07-09 16:02:24 +0100
categories: Tutorial
excerpt: loremNostrud est anim Lorem ea pariatur anim consequat sunt laborum pariatur et et
---

{% assign post_identifier = page.post_identifier %}


## Description/Introduction
First attempt at trying to integrate Blazor Web Assembly, .Net Core Identity, IdentityServer4 & a protected API into one solution.

## References
* [Marco De Sanctis, Securing Blazor WebAssembly with Identity Server 4 (updated to Preview 5)](https://medium.com/@marcodesanctis2/securing-blazor-webassembly-with-identity-server-4-ee44aa1687ef)

## Github Repo
* [FirstAttempt)](https://github.com/Xeppit/FirstAttempt)

## Goals
Create three projects, IdentityServer, Blazor Webassembely, .NetCore WebApi.
Configure all three so that you can login using the blazor project and access the protected Api using the logged in user.

## Creating Project Structure

### Using the console install the IdendityServer Templates
```
dotnet new -i IdentityServer4.Templates
```

### Using the console create the project structure
```
mkdir FirstAttempt
cd FirstAttempt
dotnet new sln -n FirstAttempt
dotnet new blazorwasm -au individual -n FirstAttempt.Blazor
dotnet new is4aspid -n FirstAttempt.IdentityServer
dotnet new webapi -n FirstAttempt.Api
```
### Add the projects to the solution

```
dotnet sln add FirstAttempt.Blazor
dotnet sln add FirstAttempt.IdentityServer
dotnet sln add FirstAttempt.Api
```

### Add To Source Control
Add the solution to to github using visual studio

## Configuring the Project

IdentityServer on port Https:5000

Blazor on port https:5002, Http:5001

API on Port https:5004, http:5003

### FirstAttempt.IdentityServer/Properties/LaunchSettings.json
```json
{
  "profiles": {
    "SelfHost": {
      "commandName": "Project",
      "launchBrowser": false,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "https://localhost:5000"
    }
  }
}
```

### FirstAttempt.Blazor/Properties/LaunchSettings.json
```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:8511",
      "sslPort": 44330
    }
  },
  "profiles": {
    "FirstAttempt.Blazor": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "https://localhost:5002;http://localhost:5001",
      "inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
    }
  }
}
```
### FirstAttempt.Api/Properties/LaunchSettings.json
```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iisExpress": {
      "applicationUrl": "http://localhost:20037",
      "sslPort": 44333
    }
  },
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "FirstAttempt.Api": {
      "commandName": "Project",
      "launchUrl": "weatherforecast",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "https://localhost:5004;http://localhost:5003"
    }
  }
}
```

## Configure IdentityServer
### FirstAttempt.IdentityServer/Config.cs
```csharp
public static IEnumerable<Client> Clients =>
    new Client[]
    {
        // interactive client using code flow + pkce
        new Client
        {
            ClientId = "blazor",
            AllowedGrantTypes = GrantTypes.Code,
            AllowedCorsOrigins = {"https://localhost:5002"},
            RequirePkce = true,
            RequireClientSecret = false,
            RedirectUris = {"https://localhost:5002/authentication/login-callback"},
            PostLogoutRedirectUris = {"https://localhost:5002/"},
            AllowedScopes = {"openid", "profile", "scope2"}
        },
    };
```

### FirstAttempt.Blazor/Quickstart/Account/AccountOptions.cs
```csharp
public static bool ShowLogoutPrompt = false;
public static bool AutomaticRedirectAfterSignOut = true;
```

## Configure Blazor
### FirstAttempt.Blazor/wwwroot/appsettings.json
```json
{
  "oidc": {
    "Authority": "https://localhost:5000/",
    "ClientId": "blazor",
    "DefaultScopes": [
      "openid",
      "profile",
      "scope2"
    ],
    "PostLogoutRedirectUri": "/",
    "ResponseType": "code"
  }
}
```
### FirstAttempt.Blazor/Program.cs/Main
```csharp
builder.Services.AddOidcAuthentication(options =>
{
    // Configure your authentication provider options here.
    // For more information, see https://aka.ms/blazor-standalone-auth
    builder.Configuration.Bind("oidc", options.ProviderOptions);
});

```

## Customising Scopes and Claims

### FirstAttempt.IdentityServer/Config.cs
```csharp
public static IEnumerable<IdentityResource> IdentityResources =>
    new IdentityResource[]
    {
        new IdentityResources.OpenId(),
        new IdentityResources.Profile(),
        new IdentityResources.Email()
    };

public static IEnumerable<Client> Clients =>
    new Client[]
    {
        // interactive client using code flow + pkce
        new Client
        {
            ClientId = "blazor",
            AllowedGrantTypes = GrantTypes.Code,
            AllowedCorsOrigins = {"https://localhost:5002"},
            RequirePkce = true,
            RequireClientSecret = false,
            RedirectUris = {"https://localhost:5002/authentication/login-callback"},
            PostLogoutRedirectUris = {"https://localhost:5002/"},
            AllowedScopes = {"openid", "profile", "scope2", "email"}
        },
    };
```

### FirstAttempt.Blazor/wwwroot/appsettings.json
```json
{
  "oidc": {
    "Authority": "https://localhost:5000/",
    "ClientId": "blazor",
    "DefaultScopes": [
      "openid",
      "profile",
      "scope2",
      "email"
    ],
    "PostLogoutRedirectUri": "/",
    "ResponseType": "code"
  }
}
```

### FirstAttempt.Blazor/Pages/Claims.Razor
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

### FirstAttempt.Blazor/Shared/NavMenu.razor
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
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="claims">
                <span class="oi oi-list-rich" aria-hidden="true"></span> Claims
            </NavLink>
        </li>
    </ul>
</div>
```

## Configering the Api
Using nuget manager add "Microsoft.AspNetCore.Authentication.JwtBearer" to FirstAttempt.Api

### FirstAttempt.Api/Startup.cs
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

### FirstAttempt.Api/Controllers/WeatherForecastController.cs
```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
```

### FirstAttempt.IdentityServer/Config.cs
```csharp
public static IEnumerable<ApiScope> ApiScopes =>
    new ApiScope[]
    {
        new ApiScope("testapi"),
    };
```

## Configering Blazor to use the new Api
Using nuget manager add "Microsoft.Extension.Http" to FirstAttempt.Blazor

### FirstAttempt.Blazor/Program
```csharp
public static async Task Main(string[] args)
{
    var builder = WebAssemblyHostBuilder.CreateDefault(args);
    builder.RootComponents.Add<App>("app");
    
    builder.Services.AddOidcAuthentication(options =>
    {
        builder.Configuration.Bind("oidc", options.ProviderOptions);
    });
    builder.Services.AddHttpClient("api")
        .AddHttpMessageHandler(sp =>
        {
            var handler = sp.GetService<AuthorizationMessageHandler>()
                .ConfigureHandler(
                    authorizedUrls: new[] { "https://localhost:5004" },
                    scopes: new[] { "testapi" });
            return handler;
        });
    // we use the api client as default HttpClient
    builder.Services.AddScoped(
        sp => sp.GetService<IHttpClientFactory>().CreateClient("api"));
    await builder.Build().RunAsync();
}
```

### FirstAttempt.Blazor/wwwroot/appsettings.json
```json
{
  "oidc": {
    "Authority": "https://localhost:5000/",
    "ClientId": "blazor",
    "DefaultScopes": [
      "openid",
      "profile",
      "email",
      "testapi"
    ],
    "PostLogoutRedirectUri": "/",
    "ResponseType": "code"
  }
}
```

### FirstAttempt.Blazor/Pages/FetchData.razor
``` csharp
@page "/fetchdata"
@using Microsoft.AspNetCore.Authorization
@inject HttpClient Http
@attribute [Authorize]
```

```csharp
@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        forecasts = await Http.GetFromJsonAsync<WeatherForecast[]>("https://localhost:5004/WeatherForecast");
    }

    public class WeatherForecast
    {
        public DateTime Date { get; set; }

        public int TemperatureC { get; set; }

        public string Summary { get; set; }

        public int TemperatureF { get; set; }
    }
}
```