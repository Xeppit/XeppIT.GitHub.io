---
layout: post
title: How to effectively write use cases
excerpt: "A description on how to write useful Use Cases"
categories: [Software Design]
tags: [Use Cases, Programming, Software Design]
modified: 2019-11-29
comments: true
---

## Description
This tutorial will look at why and how to write use cases.

## Problems/Purpose
* Writing software without a specification causes messy code.
  * Use cases can help as it breaks down your code into structured units of work.

* Writing software can involve a lot of code which needs to be organized.
  * Again, use cases will help you break down your code into a manageable, smaller, independent block.

* Communicating how your software works to someone can be difficult.
  * Use cases are simple enough for everyone to understand and even be involved in creating.

## Goals
* Create a Use case.

## Perquisites
* Pen and Paper

## How To Write Use Case's
1. List who/what is going to be using the system.
2. For each who/what in the list define what tasks that item needs to do.
3. Each task is a use case. Fill out a use case form for each task.
4. Compare all the use cases and combine similar use cases if possible.
5. Repeat the steps 1 through 4 for all other who/what's.
6. Compare all the use cases and combine similar use cases if possible.

## Description use case elements

##### ID
A unique Id for the use case.

##### Actor
A person/software/hardware system that interacts with your software to achieve the Postconditions of this use case.

##### Title
A short, active verb phrase.

##### Trigger
An event that triggers the use case

##### Overview
Describe the goal of the use case. this should be the title expanded on.

##### Priority
How important this use case is on a scale 1 - 10

##### Difficulty
How difficult this use case is on a scale 1 - 10

##### Preconditions
What is needed before the use case is executed.

##### Postconditions
What the result of the use case is.

##### Main Success Flow
The steps it takes to get from the preconditions to postconditions, with no errors

##### Alternate Flow
These errors that happen when things go wrong at the system.

## Example

**ID** :
UC001

**Actor** :
Me

**Title** :
Cook Baked Beans

**Trigger** :
Get Hungry

**Overview** :
When I get hungry I go to the bake bean cupboard and get a can of bake beans, I empty the bake beans into a bowl and then cook them in the microwave. Once the have finished cooking I take them out of the microwave and eat them.

**Priority** :
5

**Difficulty** :
5

**Preconditions** :-
* Baked Beans
* Microwave
* Bowl

**Postconditions** :
* Full Belly

**Main Success Flow**
1. Go to cupboard and get baked beans.
2. Open tin
3. Put beans in bowl.
4. Cook in microwave
5. Eat beans

**Alternate Flows**

3.1 No clean bowls
  * 3.1.1 Wash dirty bowl.
  * 3.1.2 Re-enter Main flow at 3

3.2 No bowls
  * 3.2.1 Use something else.
  * 3.2.2 throw error

5.1 Beans to hot
  * 5.1.1 Leave beans cool
  * 5.1.2 Re-enter Main flow at 5


## References
* [usability.gov](https://www.usability.gov/how-to-and-tools/methods/use-cases.html)
* [projectmanagementdocs.com](https://www.projectmanagementdocs.com/template/project-documents/use-case-document/#axzz66hvMDUZ5)

## What's Next
N/A
