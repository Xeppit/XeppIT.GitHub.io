---
layout: post
title: How to create a .net core WebApi with swagger
excerpt: "How to create a .net core WebApi with swagger."
categories: [Programming]
tags: [ASP.net Core, WebApi, Swagger]
modified: 2019-11-29
comments: true
---
## Description

How to create a .net core WebApi with swagger.

## Goals

* Create a new .netcore WebApi.

* Install Swagger.

## Perquisites

* Computer Win/Mac/Linux
* VS Code
* .Net Core 3.1 SDK

[Github Repo](https://github.com/Xeppit/Education.WebApiWithSwagger/)

## Creating the Project & Adding Swagger

In a vscode terminal navigate to the directory you want to create the project in and run the command

    dotnet new webapi

Now add the **Swashbuckle.AspNetCore -v  5.0.0-rc4** package to the project using the Nuget

In the  `Startup`  class, import the following namespace to use the  `OpenApiInfo`  class:


```csharp
using Microsoft.OpenApi.Models;

```

Add the Swagger generator to the services collection in the  `Startup.ConfigureServices`  method:


```csharp
public void ConfigureServices(IServiceCollection services)
{ services.AddDbContext<TodoContext>(opt =>
	opt.UseInMemoryDatabase("TodoList"));
services.AddControllers(); // Register the Swagger generator, defining 1 or more Swagger documents
services.AddSwaggerGen(c =>
{ 
	c.SwaggerDoc("v1", new OpenApiInfo { Title = "My API", Version = "v1" });
});
```

In the  `Startup.Configure`  method, enable the middleware for serving the generated JSON document and the Swagger UI:



```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable middleware to serve generated Swagger as a JSON endpoint.
    app.UseSwagger();

    // Enable middleware to serve swagger-ui (HTML, JS, CSS, etc.),
    // specifying the Swagger JSON endpoint.
    app.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
    });

    app.UseRouting();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

The preceding  `UseSwaggerUI`  method call enables the  [Static File Middleware](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/static-files?view=aspnetcore-3.0). If targeting .NET Framework or .NET Core 1.x, add the  [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)  NuGet package to the project.

Launch the app, and navigate to  `http://localhost:<port>/swagger/v1/swagger.json`. The generated document describing the endpoints appears as shown in  [Swagger specification (swagger.json)](https://docs.microsoft.com/en-us/aspnet/core/tutorials/web-api-help-pages-using-swagger?view=aspnetcore-3.0#swagger-specification-swaggerjson).

The Swagger UI can be found at  `http://localhost:<port>/swagger`. Explore the API via Swagger UI and incorporate it in other programs.

Tip

To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the  `RoutePrefix`  property to an empty string:

C#Copy

```csharp
app.UseSwaggerUI(c =>
{
    c.SwaggerEndpoint("/swagger/v1/swagger.json", "My API V1");
    c.RoutePrefix = string.Empty;
});
```

If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the  `./`  prefix. For example,  `./swagger/v1/swagger.json`. Using  `/swagger/v1/swagger.json`  instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used). For example, use  `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json`  instead of  `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.


## References

*  [Microsoft Official](https://docs.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.0&tabs=visual-studio-code)


## What's Next

*  [Jekyll](https://jekyllrb.com/)