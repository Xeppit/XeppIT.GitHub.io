---
layout: post
post_identifier: 2020-07-03-Identity-Server-4-Course
title:  "Securing .Net API's Using Idendity Server 4"
date:   2020-07-04 16:02:24 +0100
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

### Using the console create the project structure
```
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

```
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