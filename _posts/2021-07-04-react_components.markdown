---
layout: post
title:      "React Components"
date:       2021-07-04 13:09:31 +0000
permalink:  react_components
---


When using the React framework, you work with components. Essentially, components are a way of splitting up a web application into smaller parts that allow you to render data more efficiently, allow greater ease of use for those using the application, and also to assist with styling. Examples of components would be a navigation bar, an article, an image, etc. Components are split into two major types: class components and functional components. A class component will usually be a container and have state. Functional components will usually be a presentational component and be stateless. What this means is that your class/container components will be responsible for handling data and ensuring the related presentational components get the parts that they need. Your functional/presentational components will simply display the data they're given. 

## JSX
Inside of all components, there is a render method that dictates what each component will be rendering on the page. Class components (containers) will render other components. Functional components (presentational) will render the application's content. React uses a *declarative* style of programming, while Javascript is generally *imperative*. 

Declarative - code that directly says what you'd like to do
```
<div>Hello world!</div>
```

Imperative - code that explains how to do something in detailed steps
```
let div = document.createElement('div')
div.innerText = "Hello world!"
document.body.appendChild(div)
```

In order to allow programmers to write declarative Javascript, JSX is used. JSX is a syntax extension of Javascript that allows html-like syntax in Javascript code, as shown in the example above. Within JSX vanilla Javascript is still able to be used, however it needs to be wrapped in curly braces.
```
const greetingArray = ["Hello!", "Hola", "Bonjour"]
{greetingArray.map(greeting => console.log(greeting)}
```

## Class Components
Class components will generally handle the rendering of other components as well as methods that manipulate the DOM and communicate with the server. They use state which is an object that stores important data and is able to be mutated through the life cycle of the component.

## Functional Components
Functional components do not handle any manipulation of the DOM and do not have state. They simply display data passed into them from their parent components via props. 

## Props
Props are properties passed from a parent component to a child component. 

PARENT
```
render(){
     return(
       <ChildComponent data={this.state} />
	)
}
```

CHILD
```
return(
    {this.props.data}
)

```
