---
layout: post
post_identifier: 2020-07-21-Starting-Point-Of-Project-Management-App
title:  "Starting-Point-Of-Project-Management-App"
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
* Create Controllers for Company, People, Address

## Perquisites
* Dotnet SDK

## Creating Project Structure

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Project Structure" %}

### Using the console create the project structure
```console
mkdir ProjectManagerPlus
cd ProjectManagerPlus
dotnet new sln -n ProjectManagerPlus
dotnet new wpf -n ProjectManagerPlus.Wpf
dotnet new webapi -n ProjectManagerPlus.Api
```
### Add the projects to the solution

```console
dotnet sln add ProjectManagerPlus.Wpf
dotnet sln add ProjectManagerPlus.Api
```
### Add To Source Control
Add the solution to to github using visual studio

## Configuring the Project

Https Port: 5001 -ProjectManagerPlus.Api

### ProjectManagerPlus.Api/Properties/LaunchSettings.json
```json
{
  "$schema": "http://json.schemastore.org/launchsettings.json",
  "profiles": {
    "ProjectManagerPlus.Api": {
      "commandName": "Project",
      "applicationUrl": "https://localhost:5001",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```
