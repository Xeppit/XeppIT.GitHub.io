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

## Content

### Creating basic API Project Structure

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Create a blank solution" %}

{% include postimage.html 
postname=post_identifier
imageName="1-2.png" 
caption="" %}

{% include postimage.html 
postname=post_identifier
imageName="1-3.png" 
caption="" %}

{% include postimage.html 
postname=post_identifier
imageName="1-4.png" 
caption="Add a new ASP.NET Core Web Application to the solution" %}

{% include postimage.html 
postname=post_identifier
imageName="1-5.png" 
caption="The name of the project should follow a convention SolutionName.xyzAPI" %}

{% include postimage.html 
postname=post_identifier
imageName="1-6.png" 
caption="Select the API template" %}

{% include postimage.html 
postname=post_identifier
imageName="1-7.png" 
caption="" %}

{% include postimage.html 
postname=post_identifier
imageName="1-8.png" 
caption="In your chosen webbrowser install and extension/add on that will format json documents. For Edge I use JSON Beauty Formatter" %}

{% include postimage.html 
postname=post_identifier
imageName="1-9.png" 
caption="In visual Studio Press F5 or click the play button and the API should launch your chosen webbrowser and show you some json that has been retreived from your API." %}

### Adding A Model

{% include postimage.html 
postname=post_identifier
imageName="2-1.png" 
caption="Create a new class to use as the model" %}

### Adding the DBContext

{% include postimage.html 
postname=post_identifier
imageName="3-1.png" 
caption="Add the following nuget Packages" %}

{% include postimage.html 
postname=post_identifier
imageName="3-2.png" 
caption="Create a new class to use as the DbContext" %}

### Configuring DBContext

{% include postimage.html 
postname=post_identifier
imageName="4-1.png" 
caption="Add the code highlighted to Rolodex.API.Startup.ConfigureServices" %}

### Adding the Controller

{% include postimage.html 
postname=post_identifier
imageName="5-1.png" 
caption="In visual studio right click on the Controllers folder --> Add --> Controller. The window above will be shown" %}

{% include postimage.html 
postname=post_identifier
imageName="5-2.png" 
caption="Select the model and dbcontext as above" %}

{% include postimage.html 
postname=post_identifier
imageName="5-3.png" 
caption="If this is the first time you have generated a controller in this project Visual Studio will first need to install some nuget packages, once installed A controller will be generated for you." %}

### Launch the new API

{% include postimage.html 
postname=post_identifier
imageName="6-1.png" 
caption="In Rolodex.API.launchSettings.json change the launchUrl to the controllers name without the word controller e.g. AddressesController = Addresses" %}

{% include postimage.html 
postname=post_identifier
imageName="6-2.png" 
caption="Press F5/Runn the solution. As there are no entries an empty json array is returned" %}

### Testing the New API with Postman

{% include postimage.html 
postname=post_identifier
imageName="7-1.png" 
caption="A GET request to the URL returns all records" %}

{% include postimage.html 
postname=post_identifier
imageName="7-2.png" 
caption="A GET request to the URL + an Id will return a single record" %}

{% include postimage.html 
postname=post_identifier
imageName="7-3.png" 
caption="A POST request to the url with a json body will insert a record" %}

{% include postimage.html 
postname=post_identifier
imageName="7-4.png" 
caption="A PUT request to the URL + Id & a json body will update a record" %}

{% include postimage.html 
postname=post_identifier
imageName="7-5.png" 
caption="A DELETE request to the URL + an Id will delete a record" %}

