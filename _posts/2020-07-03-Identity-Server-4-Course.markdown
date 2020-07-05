---
layout: post
title:  "Securing .Net API's Using Idendity Server 4"
date:   2020-07-04 16:02:24 +0100
categories: Tutorial
excerpt: loremNostrud est anim Lorem ea pariatur anim consequat sunt laborum pariatur et et
---

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

Create a blank solution

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-1.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-2.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-3.png" %}

Add a new "ASP.NET Core Web Application" to the solution

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-4.png" %}

The name of the project should follow the convention

"SolutionName"."UniqueDescription""ProjectType"

I have used the name "Rolodex.API" as I know there is only going to be one API in this example but in the real world the name should be somthing like "Rolodex.CustomerAPI"

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-5.png" %}

Select the "API" template

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-6.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-7.png" %}

In your chosen webbrowser install and extension/add on that will format json documents.

For Edge I use "JSON Beauty Formatter"

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-8.png" %}

In visual Studio Press F5 or click the play button and the API should launch your chosen webbrowser and show you some json that has been retreived from your API.

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-1-9.png" %}

### Adding A Model

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-2-1.png" %}

### Adding the DBContext

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-3-1.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-3-2.png" %}

### Configuring DBContext

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-4-1.png" %}

### Adding the Controller

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-5-1.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-5-2.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-5-3.png" %}

### Launch the new API

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-6-1.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-6-2.png" %}

### Testing the New API with Postman

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-7-1.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-7-2.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-7-3.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-7-4.png" %}

{% include postimage.html postname="2020-07-03-Identity-Server-4-Course" imageName="2020-07-03-Identity-Server-4-Course-7-5.png" %}
