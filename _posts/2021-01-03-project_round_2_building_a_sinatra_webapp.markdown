---
layout: post
title:      "Project Round 2: Building A Sinatra Webapp"
date:       2021-01-04 02:23:57 +0000
permalink:  project_round_2_building_a_sinatra_webapp
---

My cohort and I were tasked with building a Sinatra-based web application and it turned out to be a fun and challenging experience. This project had a few requirements to be met:

1. We needed to use Sinatra
2. It had to be an MVC app
3. We also needed to use ActiveRecord with Sinatra
4. There needed to be multiple models
5. We needed to have at least one has-many/belongs-to relationship
6. Our app must have user accounts (allowing signin/signup/logout) that could be validated
7. The users then would need to be able to modify and/or delete their information and that of their resource
8. Data needed to be validated so that no bad data would be persisted to the database
9. Then finally, as a bonus, we could also display the validation failures with error messages


<iframe src="https://giphy.com/embed/dIE18NpNsvKVi" width="480" height="227" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/maudit-supernatural-maudit-dIE18NpNsvKVi"></a></p>

At first glance this project was a bit daunting to say the least. To ease my stress, I decided to tackle this step by step. I began hitting each requirement one by one then rounded back to tie it all together. Before I knew it, I had my own functional webapp! 

## Sinatra? ActiveRecord? MVC?

This project had a lot more to it than the last (CLI) app, that's for sure. While still focusing on creating a user-interaction based app, now we used the Sinatra framework to create a web-based application versus a command-line app. Sinatra allows Ruby code to be used to allow the user to view different pages, enter and save data, interact with that data, etc. ActiveRecord pairs with Sinatra to create a database responsible for storing all of the data that users of the application submit. The application, like many web applications, uses MVC or Model-View-Controller. Models are built to symbolize your data and are responsible for dictating the structure of specific pieces of data. For example, "User" would be a model that is created to store values regarding your users such as username, password, email, etc. Through using the "User" model, an application is able to apply the structure and rules created for a user's profile to every single account created. The views then allow a developer to take their model's information and display it through the browser. All of the interaction between a program and whomever is utilizing it are created in the controller. Controllers use HTTP methods (thanks to Sinatra) that allow a program to communicate with its user and vice versa. 

## Step by Step

### What to do, what to do...

The core goal of our Sinatra project was to create an app that a user would be able to see/edit/delete the data of their resource with. For my application, I decided to go with something more fantasy than real-world. I created "Case Hunter", a webapp that allows its users (the hunters of the *Supernatural* tv series universe) to keep track of their cases, as well as the monsters involved in those cases. My **models** were "Hunter", "Case", and "Monster". 

### Relationships

As models are used to symbolize your data, relationships are used to symbolize how that data interacts with each other. For this project we needed to utilize a *has-many/belongs-to* relationship. I decided to add in a challenge by implementing a *has-many-through* relationship. Instead of directly interacting with each other, two of my models would interact *through* a third model. I set this relationship up like so: a **hunter** has many monsters through cases, a **monster** has many hunters through cases, and **cases** belonged to both hunters and monsters. That way a single hunter could have multiple cases and multiple monsters, a single monster could have multiple cases and multiple monsters, and a case would have a hunter and a monster.

### Let's Get Hunting!

Hunters, my user, are able to create a new profile, login, logout, or delete their profiles. Before breaking down what exactly that means I'll give a little background on what makes that possible: sessions. A **session** is a hash that lives on the server and stores data regarding a user's interactions with a website. How does this relate? In order to **login**, a piece of data that is unique to each individual user (such as their ID) is saved to the session hash. Now the application is able to know which user it is interacting with in that particular session. In order to **logout**, you simply need to clear the session hash's content. Deleting the profile, however, does not deal with the session hash. Instead, to **delete** an account, a user's information is erased from the database. 

### So where does it all go?

Due to the use of ActiveRecord, all *valid* data is saved into your application's database. To store information, databases use *tables*. For each model, there is a corresponding table that holds data about every instance of that model. **Tables** use columns and rows. A *column* holds a specific piece of data while a *row* holds the full collection of data for an instance. For instance, we have a table called hunters that holds the data about every instance of the Hunter model created. It'd look something like this:

| ID | name | username |
| ---------- | -------- | -------- |
| 1    | Dean      | DeanWin79     |
| 2     | Bobby    | BobbySinger   |


### CRUD and RESTful routing

Each model needed their own corresponding controller. Within the controllers, I implemented CRUD functionality using RESTful routes. What does that mean?

* **Create** - a new instance of each model was able to be created. The controller would first lead a user to a page with a form to create a new hunter/case/monster, then once that form was submitted the controller would take that information in and use it to update the database with a new instance. It looked something like this:

```
get '/(model)s/new' do
   erb :/(model)s/new
end
```
This action would render a *view* page with the route "/(model)/new" that contained a form for the user to fill out the required information for that model.
<br>
```
post '/(model)s' do
   model = Model.create(params)
	 redirect "/(model)s/#{model.id}"
end
```
This action takes the information from the form the user submitted that is contained in a hash called *params* and uses it to create a new instance of the model before re-directing the user to a *view* action with that model's information. If the model was the user (Hunter) rather than a resource (Case or Monster) this is where they'd be logged in by assigning their ID to the session hash by adding a line after the model is created like so
```
session[:user_id] = model.id
```

* **Read** - information regarding an instance of the model is displayed in the browser. In the last line of the method above, the controller is sent to the route responsible for showing the information for that specific instance of the model. It would look like this:

```
get '/(model)s/:id' do
   @model = Model.find_by(id: params[:id])
	 erb :"/(model)s/show"
end
```
This action finds the specific instance of the model within the database using the ID provided in the route from the previous method. It then saves it as an instance variable so that the data is able to be accessed from within the view rendered, also known as the show page.

* **Edit** - an instance of the model is able to be edited/updated. Much like in creation, the controller requires two actions to implement this step. First, an action to render the page containing the form to edit, then an action to take that information and update the record within the database.

```
get '/(model)s/:id/edit' do
   model = Model.find_by(id: params[:id])
	 erb :"/(model)s/edit"
end
```
This action once again searches for the particular instance of the model, then saves it into an instance variable to be accessed by the edit view page.

```
patch '/(model)s/:id' do
   model = Model.find_by(params[:id])
	 model.update(params[:model])
	 redirect "/(model)s/#{model.id}"
end
```
This action matches the model ID submitted to one within the database, then takes the information and updates the record with any changes before redirecting the user to a page containing the model's (now updated) information.

* **Delete** - an instance is able to be deleted. The controller will receive a request to delete the instance, do so, then return the user to whatever page is specified by the developer. For my application a user was sent to their homepage when deleting a case or monster, then sent back to the login/signup page when deleting their account. For this action, the "form" is simply a button displayed on the show page, that once clicked, will send the request for deletion.

```
delete '/(model)s/:id' do
     model = Model.find_by(id: params[:id])
		 model.delete
		 
		 redirect "(where ever developer wants)"
end
```
This action receives the delete request for an instance, finds that instance, then deletes it. Once deletion is completed, it will send the user to another action of the developer's choosing.



**A few more things:** 
* There is a difference between render and redirect. To *render* means to load a view page for the user. To *redirect* means to go from one controller action to another. 
* All actions use HTTP verbs which dictate if the action is receiving or sending information. A *get* request is used when the controller is fetching a view page to be rendered. A *post* request is used when a form has been submitted containing information that the controller will pass onto the server. A *patch* request is used when editing information and operates similarly to a post request. A *delete* request is used when deleting, obviously. A delete form doesn't contain information other than the ID of what you're deleting and appears to the user simply as a button. 
* In the routes, a model is pluralized. So, hunter would become hunters. In my examples I used (model) as a placeholder but a specific example of a route would be '/hunters/new'.
* ":id" acts as a placeholder within the routes and is then filled in with its true value in the action. This is called dynamic routes and enables one route to be used with multiple different sets of data.

## Wrapping it Up
This unit and project taught me so much and really had me feeling like a true software engineer. I'm proud of myself for what I've created and challenging myself to do a little more than required. I'm gaining confidence in my abilities and ready to rock what's next. Rails, here I come!

