---
layout: post
title: Getting started with syncfusion's blazor components
excerpt: "In this post we will be looking at how to get a fresh .Net core Blazor, serverside app with syncfusions component library installed."
categories: [Programing]
tags: [Syncfusion, Blazor, ASP.net, Setup]
modified: 2019-11-29
comments: true
---

## Description

In this post we will be looking at how to get a fresh .Net core Blazor, serverside app with syncfusions component library installed

## Goals
* Use the dotnet template engine to generate a Blazor serverside app.
* Install syncfusion's nuget package.
* Use a control to test syncfusion has been set up.

## Perquisites

* Computer Windows
* VS Code 
* .Net Core SDK 3.1
* [Syncfusion Community License](https://www.syncfusion.com/products/communitylicense).
      * Free for companies and individuals with less than $1 million USD in annual gross revenue and 5 or fewer developers.

## .Net Core Blazor Server Side App

* In a VS Code terminal navigate to the directory you want to store your app.
* Use the command `dotnet new blazorserver` this will create a project with the name of the folder you navigated to.
* Press F5 and check it loads up and is all working.

## Syncfusion Blazor Component Library
* In VS Code using nuget install `Syncfusion.EJ2.Blazor` package.
* Open `~/_Imports.razor` file and import the `Syncfusion.EJ2.Blazor` by adding the following.

```csharp
@using Syncfusion.EJ2.Blazor
@using Syncfusion.EJ2.Blazor.Calendars
```
* Add the client-side resources through CDN in the `<head>` element of the `~/Pages/_Host.cshtml` page.

```html
<head>
    <link href="https://cdn.syncfusion.com/ej2/17.3.29/material.css" rel="stylesheet" />
    <script src="https://cdn.syncfusion.com/ej2/17.3.29/dist/ej2.min.js"></script>
    <script src="https://cdn.syncfusion.com/ej2/17.3.29/dist/ejs.interop.min.js"></script>
</head>
```
* Now, add the Syncfusion Blazor components in any web page (razor) in the `~/Pages` folder. For example, the calendar component is added in the `~/Pages/Index.razor` page.

```html
<EjsCalendar></EjsCalendar>
```
* Now press F5 to run the website.

## References

*  [Syncfusion Blazor Getting Started](https://ej2.syncfusion.com/blazor/documentation/getting-started/dotnet-cli-blazor-server/)

## What's Next

N/A