---
layout: post
title:      "Gettin' Clicky with JavaScript"
date:       2021-04-30 21:57:48 +0000
permalink:  gettin_clicky_with_javascript
---


My fourth mod's focus was JavaScript and SPA (single page applications). I learned that your best friend when creating a SPA using JavaScript is events! Through the use of events, some CSS, a bit of HTML, and DOM manipulation, I saw my project slowly come to life. In the end, I now have a one-page web application with multiple features that operates entirely on that page - no refreshes needed.  

  

One thing that definitely helped me through this mod was the similarities that coding languages can have. Through my knowledge of Ruby, I was able to grasp a lot of the JavaScript terminology fairly quickly. For starters, both languages are able to be Object Oriented. Through the use of classes and instances, objects and their behaviors are created and executed as virtual models of real-life practices. We can use my Item class for an example. Upon creating an Item in Ruby, that item is *initialized* with certain attributes. In this case Items had a name, quantity, description, price, and the ID of their associated category. In JavaScript, class creation uses the *constructor* method, which takes in and assigns properties to each instance of Item. In Ruby, an Item instance is given its behaviors/functionality through *methods*, whereas in JavaScript they come from *functions*.  

 

The difference, however, is that Ruby classes and methods handle the back-end of an application. They're responsible for data structure and paired with the Rails framework and ActiveRecord, my back-end handled interactions with the database. In JavaScript and the front-end of the application, the responsibility was on display. The front-end used JavaScript, CSS, and HTML to display the data on the page and manipulate the elements on the DOM as needed. Through fetch/http requests, my front and back end were able to communicate and send data to eachother.

  

DOM stands for Document Object Model. Essentially the DOM is the page you're viewing. When you manipulate the DOM, you're changing the elements (like text, images, headers, paragraphs, lists, etc.) on the page. The best way to implement DOM programming is through JavaScript events. An event can be described as any action from the user on the page. Whether it be a click, mouseover, a submission, or various other events. When using JavaScript events, you first need to add a listener to an element. In my application I would grab an element, like a button, and add a listener to that element. Whenever the specified action happened to/with that element, it then would trigger a callback function which would manipulate the DOM. An example of this in my project would be when a user clicks on the "all items" button, then a function is executed that makes a list of all the items in the inventory appear on the screen. In my project I am especially fond of click events and use them to trigger the changes to the page. 

  

This project was a lot of fun! I look forward to expanding my knowledge and am proud to say that I am now able to create a full-stack web application! 
