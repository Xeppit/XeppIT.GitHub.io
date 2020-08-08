---
layout: post
post_identifier: 2020-08-02-M220N-MongoDB-For-DotNet-Developers
title:  "M220N MongoDB For DotNet Developers"
date:   2020-08-02 16:02:24 +0100
categories: Tutorial
excerpt: Learn the essentials of ASP.NET application development with MongoDB.
---

{% assign post_identifier = page.post_identifier %}


## Description/Introduction
You will play the role of a back-end developer for an ASP.NET application, where your job is to implement the application's communication with MongoDB. Using the C# driver you will read and write data to the database, use the aggregation framework, manage the configuration of the database client, and create a robust application by handling exceptions and timeouts.

## References
* [M220N: MongoDB For DotNet Developers](https://university.mongodb.com/courses/M220N/about)

## Goals
* Learn MongoDB for dot net

## Perquisites
* We highly recommend taking M001 prior to taking this course.

* A basic understanding of MongoDB's document model as well as familiarity with C# development environments will help you get the most out of this course.

## Creating Project Structure

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Project Structure" %}

## Chapter 1: Getting Started
Install MongoDB.Driver from nuget

```csharp
var client = new MongoClient(connectionString);
var database = client.GetDatabase("TestApi";
var addressCollection = database.GetCollection<AddressModel>("Addresses");
```
