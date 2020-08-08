---
layout: post
post_identifier: 2020-07-22-Dotnet-Core-Web-Api
title:  "Dotnet Core Web Api"
date:   2020-07-21 16:02:24 +0100
categories: Tutorial
excerpt: loremNostrud est anim Lorem ea pariatur anim consequat sunt laborum pariatur et et
---

{% assign post_identifier = page.post_identifier %}


## Description/Introduction
Starting Point Of Project Management App

## References
* [GitHub](https://github.com/Xeppit/WpfIdentity)

## Goals
* Set up WebApi
* Add MongoDb To Api
* Create Controllers for Company, People, Address and Projects

## Perquisites
* Dotnet SDK

## Creating Project Structure

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Project Structure" %}

### Using the console create the project structure
```console
mkdir ApiSample
cd ApiSample
dotnet new sln -n ApiSample
dotnet new webapi -n ApiSample.Api
```
### Add the projects to the solution

```console
dotnet sln add ApiSample.Api
```
### Add To Source Control
Add the solution to to github using visual studio

## Configuring the Project
Https Port: 5000 -ApiSample.Api

### ApiSample.Api/Properties/LaunchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "MicroserviceArchitecture.Api": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:5000",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## Configering the Api

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
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```