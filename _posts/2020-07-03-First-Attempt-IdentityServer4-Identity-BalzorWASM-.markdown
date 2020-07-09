---
layout: post
post_identifier: 2020-07-03-Identity-Server-4-Course
title:  "Securing .Net API's Using Idendity Server 4"
date:   2020-07-09 16:02:24 +0100
categories: Tutorial
excerpt: loremNostrud est anim Lorem ea pariatur anim consequat sunt laborum pariatur et et
---

{% assign post_identifier = page.post_identifier %}


## Description/Introduction
First attempt at trying to integrate Blazor Web Assembly, .Net Core Identity, IdentityServer4 & a protected API into one solution.

## References
* [Acticle 1](https://medium.com/@marcodesanctis2/securing-blazor-webassembly-with-identity-server-4-ee44aa1687ef)

## Goals
* Create solution
* Create IdentityServer4 project
* Create WebApi with an authorized controller
* Use blazor site to get token and user info from itentity server and use that to access the protected API

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
### FirstAttempt.Blazor/Properties/LaunchSettings.json
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

### FirstAttempt.IdentityServer/Config.cs
```csharp
public static bool ShowLogoutPrompt = false;
public static bool AutomaticRedirectAfterSignOut = true;
```

## Configure Blazor
### FirstAttempt.Blazor/Quickstart/Account/AccountOptions.cs
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

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Create a blank solution" %}