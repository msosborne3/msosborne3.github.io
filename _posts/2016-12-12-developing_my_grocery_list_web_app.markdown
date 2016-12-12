---
layout: post
title:  "Developing My Grocery List Web App"
date:   2016-12-12 05:58:29 +0000
---


Hi there! I just completed my Sinatra Portfolio Project, which is a grocery list CRUD app. The app is meant to store a user's grocery list to help shopping go a little smoother! A user is able to create an account, login, logout, view their grocery list, add items to their grocery list, edit items, and delete items.

Here's a link to a walkthrough of my application: https://youtu.be/Z0gQwDUEqks

## CRUD App
So how is this a CRUD app?

C - Create: A user is able to create a new item with a specific quantity.

R - Read: The grocery list then reads all of the items on a user's list.

U - Update: A user can edit an item's name or quantity.

D - Delete: A user can delete an item.

## Tables

I created two tables to control this application.

**Users** - 
columns: username (string) and password_digest (string)

**Items** - 
columns: name(string), quanitity (integer), and user_id (integer)

Items had a user_id so that it could belong to a user and a user could have many items.

## Models

As mentioned above, the Item class belongs_to a user, and the User class has many items. The user class also has_secure_password. This allows bcrypt to encrypt a user's password.

The User class also has a method to slug the username, meaning that if my username were "morgan is super awesome", it would become "morgan-is-super-awesome". Then there is also a method to find a user by the slugged version of its username.

## Controllers
So here is where the part that I find most important comes in (I realize that it's all important because every part has to work in order for the program to work, but here's where I find the most stuff to go wrong). The controllers interact with the views to navigate through the application.

I have created three controllers: **ApplicationController**, **UsersController**, and **ItemsController**.


##### Application Controller

This is the main part of the program that is run. It contains two helper methods that are used within the other two controller methods. One determines whether or not a user is logged in and the other returns the current user. This controller also contains the route to the home page.

##### UsersController

The UsersController is the part of the program that deals with the user's actions. Through the UsersController, a user is able to sign up, log in, view their profile page, and logout.

##### ItemsController

The ItemsController deals with the items. Through the ItemsController, a user can create a new item, edit an item, or even delete an item.

## Conclusion
Overall, I really enjoyed this project. I feel like I got a grasp on creating a Sinatra application from scratch, while realizing how much I can do, and where my limits are. I feel like from this project, I have identified some things that I need to review, as well as some things that I still need to learn, and I'm very excited to try to learn them.



