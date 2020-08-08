---
layout: post
post_identifier: 2020-07-30-M001-MongoDB-Basics
title:  "M001 MongoDB Basics"
date:   2020-07-30 16:02:24 +0100
categories: Tutorial
excerpt: loremNostrud est anim Lorem ea pariatur anim consequat sunt laborum pariatur et et
---

{% assign post_identifier = page.post_identifier %}


## Description/Introduction
Fundamentals of MongoDB: connecting to a MongoDB Cluster, using MongoDB Compass, MongoDB's document storage model and principles of flexible schema design, basic architecture of MongoDB clusters, CRUD operations.

## References
* [M001: MongoDB Basics](https://university.mongodb.com/courses/M001/about)

## Goals
* Loarn Basics Of MongoDB

## Perquisites
* Basic knowledge of programming concepts such as command line or unix shell commands, functions, variables and boolean operators.

## Creating Project Structure

{% include postimage.html 
postname=post_identifier
imageName="1-1.png" 
caption="Project Structure" %}

## Chapter 0: Setup
### Tools Overview
#### MongoDB Atlas
MongoDB Atlas is a platform that provides an interface to manage and deploy MongoDB across cloud providers and regions.

This type of offering is called database as a service.

#### Compass
Compass provides a graphical user interface that allows you to explore the data in your database.

You can use Compass to optimize query performance and manage indexes, among other things.

#### Mongo Shell
Mongo Shell is an interactive JavaScript interface to MongoDB, you will learn how to connect your Atlas instance, query and update data using the Mongo Shell, as well as import a data set into your own Atlas cluster.

The Mongo Shell is great for playing around with all the features that the database has to offer, and really learning about MongoDB query language and functionality.

### Installing MogoDB and Tools
* [M001: MongoDB Basics](https://www.mongodb.com/try/download/enterprise)

Download and run through the installation of MongoDB Enterprise Server.

Leave everything as default settings other than **Uncheck the Install MongoDB Compass**.

### Setting up MongoDB

Using **file explorer** navigate to the mongoDB bin directory
C:\Program Files\MongoDB\Server\x.x\bin
Copy the path to your system's clipboard.

1 Using **windows search bar** search **Edit the system environment variables**

2 Click on **Environment variables**

3 Double click on **Path** under the System variables

4 In the **Edit Environment Variables** window, click on **New** and then **paste** the full path to the bin directory that you just copied to your clipboard. Next, click on **Ok** to save the changes and to close the window.

Open the command prompt by typing **cmd** in the **Windows search bar**.

Type **mongo --nodb** this should start the mongo shell

### What is Atlas
Sales Pitch
We defined Atlas as a database as a service that helps you deploy and manage MongoDB clusters.

### Are you Behind a Firewall?
Please confirm that port 27017 is not blocked by clicking [Port Test](http://portquiz.net:27017).

### Compass
Compass Download Link [Compass](https://www.mongodb.com/try/download/compass).

No Install needed just run it

Paste this string in the connection string field

**mongodb+srv://m001-student:m001-mongodb-basics@cluster0-jxeqq.mongodb.net/test**

Click add to favourites, enter a name and save

Click Connect CONNECT.

### Shell
Using a command prompt connect copy and paste one of the following, the first option is the SRV connection string and option 2 is the fully defined connection string

```console
mongo "mongodb+srv://cluster0-jxeqq.mongodb.net/test" --username m001-student -password m001-mongodb-basics
```

```console
mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics
```

Switch DB's
```console
use video
```

Show all collections in db
```console
show collections
```

Show all documents in a collection
```console
db.movies.find().pretty()
```

## Chapter 1: Introduction
### Json
Much of our interaction with MongoDB will require an understanding of JSON.

MongoDB is a document database, and we frequently discuss data models looking at JSON representations of documents.

JSON, which is an acronym for JavaScript Object Notation, is a popular format for representing documents.

JSON documents begin and end with curly braces.

They're composed of fields, and each field has a key and a value.

In other programming languages, JSON documents are analogous to objects, structs, maps, or dictionaries.

The first is that all keys must be surrounded in double quotes.

Keys and values must be separated by colons.

Fields are separated from one another by commas

JSON documents support a number of value types.

There are strings, which must be surrounded by double quotes.

Also, note that inside strings is the only time when white space is significant within a JSON document.

New lines and other white space outside of quotes is not really part of the JSON document.

In addition to strings, JSON values can be floating point numbers, Boolean values, and the null value.

Finally, arrays and objects can themselves be values and nested in any combination.

JSON arrays are defined by square brackets.

Arrays contain an ordered, comma separated list of values.

Note that three of the elements are strings, but the fourth is actually an embedded document.

Json documents support any level of hierarchy that is appropriate to your application's data model.

For aggregate values, such as arrays and embedded documents, compass shows a tree view that you can click on to expand the value of the field, as we see for cast.

For more information about the JSON document format, feel free to read about it in detail www.JSON.org.

### Architechure
In MongoDB, a database serves as a namespace for collections.

Collections store individual records called documents.

Database.Collection

### Compass Schema
You can navigate to a collection click on Schema tab and then click analyse. this will check all the documents and give a report on the fileds used and what data type the fields are.

### Fields with Documents as Values
Documents can have Documents as fields

```console
-Car
--Oil
---Level(int)
---Tempratur(decimal)
--Water
---Level(int)
---Tempratur(decimal)
```

### Fields with Arrays as Values
Documents can have Arrays as fields

### Filtering Collections with Queries
On ther Schema tab of any collection you can apply a filter to the collection by finding the field you want to filter the collection on and the clicking on a data point on the chart. you can also click and drag to add a selection of data point. for example select all documents with a start time of 9am

```console
{'start time': ISODate('2016-02-19T17:31:54.000Z'),'stop time': ISODate('2016-02-19T23:22:13.000Z')}
```

## Chapter 2: The MongoDB Query Language + Atlas
### Introduction to CRUD
Provision a new atlas mongoDB and set up a admin user

using cmd navigate to your working directory
using shell log into the cluster
```console
load('loadMovieDetailsDataset.js')
```

### Insert many

Orderd false document at bottom means carry of inserting if failiur is encontered true means stop inserting if error is encountered
```json
db.moviesScratch.insertMany(
    [
        {
  	    "_id" : "tt0084726",
  	    "title" : "Star Trek II: The Wrath of Khan",
  	    "year" : 1982,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0796366",
  	    "title" : "Star Trek",
  	    "year" : 2009,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0084726",
  	    "title" : "Star Trek II: The Wrath of Khan",
  	    "year" : 1982,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt1408101",
  	    "title" : "Star Trek Into Darkness",
  	    "year" : 2013,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0117731",
  	    "title" : "Star Trek: First Contact",
  	    "year" : 1996,
  	    "type" : "movie"
        }
	],
	{
		"ordered":false
	}
);
```

### Find
you can nest searches using . notation

```console
find {"wind.type": "c"}
```

{cast: { $in: ['Jack Nicholson', 'John Huston']},viewerRating: {$gt: 7},mpaaRating: 'R'}
viewerRating: {$gt: 7}}
mpaaRating: 'R'}

## Chapter 3: Deeper Dive into the MongoDB Query Language
## Final Exam