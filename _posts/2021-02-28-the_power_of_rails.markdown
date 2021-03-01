---
layout: post
title:      "The Power of Rails"
date:       2021-03-01 03:39:27 +0000
permalink:  the_power_of_rails
---

Constantly through this past mod I've heard talk of "the power of Rails". I couldn't help but picture a big, strong, old-fashioned locomotive barreling down the tracks when thinking of Rails. Something like this:

![](https://media.giphy.com/media/8F3bK4aq1tCo0TLkf7/giphy.gif)

I feel my imagination was right as this project felt very much like a train in power but I also found myself struggling to keep up. Luckily with the help of my cohort lead and some great classmates, I managed to catch my train and complete my journey.
### All Aboard!
This project's requirements and structure were very similar to that of the Sinatra project, which was both and advantage and a challenge. Advantage, because a lot of the things I was implementing were fairly familiar. Challenge, because the Sinatra way and the Rails way differ and there were things I had to re-learn from a slightly different angle. After taking a look through the requirements and getting my notes and lecture videos handy, I was ready to go to work.

## The Journey
The web application I decided to build was inspired by my previous application: Case Hunter. This time around I created "Show Search", a web application where users can create, update, browse information, and leave reviews regarding tv shows, their characters, and the actors that play those characters. My requirements were:
* Use the Ruby on Rails framework
* Models had to include: at least one has_many, at least one belongs_to, at least two has_many :through relationships, and a many-to-many relationship implemented with has_many :through
* Models had to include reasonable validations for the simple attributes
* Include at least one chainable class level ActiveRecord scope method
* Must provide standard user authentication, including signup, login, logout, and passwords
* System must also allow login from some other service
*  Must include and make use of a nested resource with the appropriate RESTful URLs
*  Must include a nested new route with form that relates to the parent resource
*  Must include a nested index or show route
*  Forms should correctly display validation errors
*  Application must be, within reason, a DRY (Do-Not-Repeat-Yourself) rails app
*  Do not use scaffolding

## Meeting the Requirements
The first requirement was an instant-pass (yay!), the others took a little more work.

### MODELS

I ended up using something called *aliasing* with my models, because the relationships I set up got a little complicated. Basically, when you use aliasing, you're able to have multiple types of relationships between two models, under different names. My User class was not only able to create instances of my other classes they were also able to edit them. I broke that down into two relationships "creator" and "editor". The syntax when using aliasing is very similar to your regular association syntax.

In the User class I had:
```
  has_many :created_actors, foreign_key: "creator_id", class_name: "Actor"
  has_many :edited_actors, foreign_key: "editor_id", class_name: "Actor"
```
While in the Actor class I had:
```
    belongs_to :creator, class_name: "User" 
    belongs_to :editor, class_name: "User"
```

This allowed my database to store separate foreign key values that identified the creating User and the editing User. While normally to get the child in a has-many relationship I'd use: 
```
user.actors
```
I now had access to:
```
user.created_actors
user.edited_actors
```




**User**

* Has many Actors, Characters, and TvShows (as creator and/or editor)
* Has many Reviews


**Actor**

* Belongs to a User (as created by and/or edited by)
* Has many Characters
* Has many TvShows through Characters


**Character**

* Belongs to a User (as created by and/or edited by)
* Belongs to an Actor and a TvShow


**TvShow**

* Belongs to a User (as created by and/or edited by)
* Has many Characters
* Has many reviews
* Has many Actors through Characters


**Review**

* Belongs to User
* Belongs to TvShow


As you can see, my models have a lot going on.

### Validation and Authentication

In order to ensure no bad data is passed, we use validation. In order to ensure security, we use authentication. To validate an attribute, you simply establish which attribute you wish to validate and then what you're validating for:
```
class User

  validates :name, :email, presence: :true
  validates :email, uniqueness: :true
```
The line above tells ActiveRecord not to save any submitted user data without a name and email present. It also ensures that no two users have the same email address. If any of the data was deemed invalid, the user would be returned to the form to see an alert telling them their submission had failed and explaining what was wrong with the data. Authentication was made easier with the handy `.authenticate` method provided when using the bcrypt gem and `has_secure_password`. When a user logged in, the database first checks to see if there is a user that matches the submitted email, then runs the authenticate method on their password to ensure that matches as well.

### Sessions

If a user passed authentication, they'd be logged in via the sessions hash, or a collection of data that's available to the controllers during the timeframe that the web application is being used. To log someone in a user-specific attribute (like their user id) is saved into the session hash, allowing the application to know who is using it and track the user as they navigate through. To log out the session hash is cleared, removing the data. A  rewarding new task was to allow users to be authenticated and log-in via an outside source. While more complicated of a process, it allowed me to get some practice at something I know I will be implementing continuously in future applications. 

###  Nesting

While not required, I created a double nested form within my Character model, which allowed the creation of a new Actor and/or Tv Show while I created a new Character. What was required, however, were some nested routes. Your typical route will look like this: `'/tv_shows/1`. That is an example of a route to the show page for the Tv Show with the id of 1. A nested route adds on to that and can show a certain attribute, for example: `'/tv_shows/1/characters'` would be a route that leads to an index of characters that are associated with the Tv Show that has an id of 1. 

## End of The Line
This project was by far the most challenging so far, but it was also very rewarding once completed. I don't have to say I *feel* like a software developer anymore because *I am* a software developer! I built a fully-functional Ruby on Rails web application. Time to ride the rails on over to JavaScript to continue my journey with Flatiron!
