#AngularJS
In this lesson we're going to be looking into AngularJS (commonly referred to as just "Angular"), an open-source framework that was released in 2009. It is currently being mainly maintained by Google as well as a massive community of developers.

It aims to solve the problems faced when creating single page applications - we'll talk all about them shortly.

Ever wondered how Facebook can have a different timeline for millions of users? How Twitter can display tweets for 300 million people? How Gmail can let you send and receive emails, chat with your friends and view your contacts without having to even refresh the page? That's where frontend frameworks come in.

Frontend frameworks have grown massively in the last 3 years. They help developers structure their applications and create interactive, super quick applications.

Before we had the pleasure of using frontend frameworks, code would be written very procedurally. JavaScript code would become unwieldy as there was no easy way to structure code for each page that the client would see.
  
You'd be limited to just simple JavaScript interactions, such as submitting a form via AJAX. Anything more wouldn't have been scalable, resulting in a hard to debug/manage codebase.

You might be wondering what we're talking about when it comes to scaling your application. Imagine we have a simple To-do application -- the user can add/remove to-dos from a list. We'd take the initial data (previously saved to-dos) and display them in a list. Then, when the user adds another to-do, we just append another list element to our list.
 
What if we change the HTML structure of our list? Well you'd better make sure that you update both the code that displays the previously saved to-dos and the code that adds our new to-do to the list - else things could become out of sync quickly. Now imagine you had a lot of code, not necessarily structured correctly, that does similar things that our to-do app does. Not only are we repeating ourselves twice, but just by forgetting to update our code in one place would result in a bad user experience and an out-of-sync UI.

That's where frontend frameworks save the day! Imagine being able to write complex JavaScript applications, having a framework deal with scaling your code, splitting it out into logical chunks and not have to worry about one part breaking another... Sounds like a dream! Oh wait... Angular does that for us!

What else does Angular give us? Components! Using a simplistic API (more about that soon!) we can quickly create components that can be reused throughout our whole application -- this allows us to code both faster and smarter!

### Traditional Rendering
Before mainstream JavaScript usage, the server would do everything for us. When we'd go to a website, everything we needed on that page would be given to us there and then. For instance, if you go to http://wikipedia.org, the server loads the content and sends it back to us.

The problem with this is that data displayed on the page could only be updated when the user refreshes. This also meant no smooth transitions between pages (every page the user navigated to would be reloaded in the web browser).

We'd also then have all of our frontend logic mixed in with the backend logic -- possibly resulting in a horrible mess of spaghetti code!

### The role of Angular
Angular brings power to the frontend, handling routing as well as all of our pretty, interactive views. This allows frontend developers and backend developers to focus on their areas -- there's no need to mix code between the two.

### Code Architecture

Angular promotes correct and consistent code architecture by splitting our code up for us into logical chunks. By then having different folders and files for these chunks, it enforces clean and structured code.

### Dynamic Views
Angular allows us to write powerful applications straight out of the box. It attaches onto the disadvantages that static web pages have, giving us the ability to write dynamic views that are constantly updating. 

How does it do this? By implementing more features into HTML! With the use of Angular, we can have dynamic lists repeating, elements hidden and shown dynamically and much much more -- just by writing our HTML (with a few, simple modifications that we will come onto soon!).

### Back in the Stone Age
Before Angular, we'd generally use jQuery to make all of our AJAX requests, as it gave us a cross-browser implementation (there used to be no standard way of making AJAX requests!). This was great, but you were left to fend for yourself to do stuff (like update your view, or display error messages, etc.) after the request had been completed.

However, with Angular, we're given the tools to easily make AJAX requests and keep our pages updating in real-time without having to refresh. Awesome!

### Awesome! But why is it popular?
Not only does it have a massive development community with thousands of plugins, it's also a "one-stop-shop." We can depend on Angular and Angular alone to render and control our whole application. This means there are fewer technologies to learn as we're only dependent on Angular! 

## MVVM and Data Binding in Angular
In this lesson, we are going to look at the core concepts that we will use in every Angular application. Some will be familiar to you, however some may seem a bit strange. Don't worry if they take time to sink in!

Angular allows us to create single page applications (SPAs). This means that the user goes to one page and very rarely will have to refresh/leave that page to go to other parts of the website. Ever wondered how we can go to Twitter and never have to actually refresh the page? Yet all of our notifications, tweets, etc are all real-time and constantly up-to-date? You guessed it - Twitter is a single page application!

Single page applications handle all the routing, calling different parts of your codebase depending on what URL the user is currently at. If we look back at Twitter, you'll notice how the URL does change yet the page doesn't refresh. This is because we have now handed all routing over to the client side instead of the server. The client can see where the user has intended to go, and select the specific code for that page - magical!

Traditionally, server-side rendering would be like the following -

![Server-side rendering](http://www.michaelgallego.fr/images/posts/2012-11-26-client-side-1.png)

The user requests the page from the server, and the server will return all the HTML to display on that page. The data would not be updated until the user refreshes. This puts overhead on the server, as it will have to render the page on every page load. If you head over to the [BBC News](http://bbc.co.uk/news) you'll see how the content is already there - no waiting whatsoever.

With Angular, the flow will go like this instead -

![Client-side rendering](http://branchandbound.net/pics/client-side-rendering-only.png)

This looks a lot more complicated, but it isn't! Instead of returning rendered HTML for each user, the server would return the same page for every user. The client will then make a request to the server, requesting what's necessary for that page to render (such as a list of products). The client will then render this page for the user.

This has massive advantages - if 1000 users are on the website, instead of one server rendering 1000 pages, 1000 clients are rendering one page, reducing strain on the server. We can also re-use backend endpoints - our mobile website and our desktop website can both request the same data from the server but display it differently.

One example of this that you can check out for yourself is if you go to Facebook. You might notice how you don't actually see anything until the content very quickly loads and then the client renders this - saving so many resources in the meantime!

## MVC/MVVM

You might have seen the terms MVC and MVVM being thrown around. These stand for Model-View-Controller and Model-View-ViewModel respectively. 

These are concepts for how we architect our software. We do this to promote separation of concerns. Separation of concerns is dividing our code into logical chunks, with each chunk addressing a separate concern. This allows us to create testable, reusable and scalable well designed code.

They are both quite similar, however there are a few differences -

### MVC (Model-View-Controller)

Three parts to MVC; model, view, controller. The model manages the fundamental behaviours and data of the application. This could be retrieved from a database or any other sort of data store.

The view is the user interface. This takes data from the model and displays it nicely for the user. Any user interaction in the view is then handed over to the controller.

The controller receives user input and makes calls to the model and view to update/modify either. This keeps the main aspects of our application separate.

An easy way to visualize this is if we imagine we are ordering a coffee. The Model is the information about the drink we've ordered, the Controller is the barista and the View is the finished drink. The barista would make the drink from our order (the Model), and serve it to us (the View).

So for example, if you walked into a coffee bar and ordered a hot chocolate, you'd get a hot chocolate. If you then wanted marshmallows, you'd ask the barista, who would go back to the till and modify your order to add on marshmallows. They would also then put marshmallows in your drink, and return you the updated drink (the View).

If we weren't using a single page application and instead the server was rendering the page for us, the barista would have simply thrown out the drink and remade it with marshmallows - what a waste!  

If you've touched on MVC before, you might have learnt it a little differently. Not to worry! MVC in the frontend is slightly different, but the main concepts still apply!

### MVVM (Model-View-ViewModel)

Similar to MVC, however the controller is replaced with a ViewModel.

Let's take a trip back to the coffee shop. You'll notice there is a new member of staff - a cashier. The barista was too overloaded as a controller, and has stepped down to just being a helper. The cashier has been brought in as a ViewModel, in order to make the coffee shop more efficient.

Like before, we order a hot chocolate. The cashier takes your order, updating the Model. They then shout to the barista that they need a hot chocolate. The barista makes the hot chocolate, and returns it to the cashier. The cashier then writes your name on the lid, and returns it to you.

The differences here is that we no longer have one person taking your order and delivering your drink. Instead, the cashier (ViewModel) uses a helper (business logic) to get your drink order (Model) completed and only modifies the lid with your name (view logic) and returns it to you (the View).

This is the preferred architecture for Angular applications. Why? We no longer have a controller doing everything for us. Instead, we can simplify the application down massively by having helpers that we can use everywhere.

The ViewModel is responsible for all view logic - changing the model data appropriately for use in the view. (For example, our model might contain a UNIX timestamp for a date but we need it in human readable format). Your model will contain all of your business logic, and your view is there to display your model (modified by the ViewModel).

## Data-binding

Data-binding is the concept that allows us to easily keep our view and model synchronised without having to write excessive amounts of code whenever we would like to update either. This comes in two formats - one-way and two-way data-binding.

### One-way data-binding

Imagine you have a form input, asking for the user's name. You've also got a `<h2>` tag below, that you want to populate to say "Hey, {name}!". Wouldn't it be neat if this `<h2>` tag could update as the user types in their name? This is called one-way data-binding. The model (our user's name) is updated and our view is updated to reflect the change to our model.

For example, you'd use one-way data-binding when you've got a search box - the user can type in their search query and the title can change to "Results for: {searchQuery}" - pretty cool!

[Here's an example of one-way data-binding.](https://jsfiddle.net/6z6d5b7x/)
 
An easy way to visualize this is to go back to when we were receiving our drink from the barista. If the drink was one-way bound from the barista, the drink would always be updated and correct. However, the barista wouldn't be able to see the drink. If it got knocked over or was mouldy, the barista wouldn't have any idea!

### Two-way data-binding 

Two-way data-binding occurs when you bind your model value to an element that can both change and display the value of the model (such as an input).

You aren't restricted to updating your model value just in the `<input />` however - you can update this Model value elsewhere (such as a "reset" button to reset the model) and it will be automatically reflected in the input (it will go blank).

[Checkout this example of two-way data-binding](https://jsfiddle.net/6z6d5b7x/1/)

You can see the flow of this here -

![Two-way data-binding](https://docs.angularjs.org/img/Two_Way_Data_Binding.png)

Going back to our barista - with two-way data-binding, our barista can now see everything! If our drink was to get knocked over after given to us, the barista would notice and be able to act upon that wild change of events!

Two-way data-binding has a one-directional flow of data which ensures that the View and the Model data is always synchronized.

In the above example our template compiles to become our View, which is a representation of the Model data. The View can then change our Model (for instance, an `<input />` being updated), which then updates our View. Model changes drive View changes and View changes drive Model changes. This keeps everything always synchronized. 

## Resources

- [AngularJS Dependency Injection Manual](https://docs.angularjs.org/guide/di)
- [AngularJS Databinding Documentation](https://docs.angularjs.org/guide/databinding)

# Angular Setup + directory structure

## Overview

We need to ensure that we have a level-headed way of structuring our applications from the beginning, otherwise we could end up with files everywhere, which means it's hard to know exactly what you are editing and where.

## Objectives

- Define the core structure of an Angular application
- Create a feature based file structure

## Core Structure

There are plenty of other ways to structure your application. One popular way is to just have folders for controllers, directives etc without having folders for each module, as follows - 

```
├── js/
│   ├── directives/
│   │   ├── TodoEscape.js
│   │   ├── TodoFocus.js
│   │   ├── LoginForm.js
│   ├── controllers/
│   │   ├── TodoController.js
│   │   ├── LoginController.js
│   ├── services/
│   │   ├── TodoService.js
│   │   ├── LoginService.js
│   ├── templates/
│   │   ├── partials/
│   │   │	├── TodoForm.html
│   │   │	├── LoginForm.html
│   │   ├── views/
│   │   │	├── Todo.html
│   │   │	├── Login.html
│   ├── app.js
│   ├── angular.js
├── img/
├── css/
├── index.html
```

This works, but as your application gets bigger, the folders will end up with loads of files in them, without us knowing what part of the application they are responsible for.

This is why we separate our app into "modules". We can then immediately know what file is used in what part of the application just by looking at what folder it is in. Each module will have directives, templates, controllers and services.

Like a normal application, we'll have separate folders for our JavaScript, CSS and images. However, inside our JavaScript folder we'll have individual folders for each module in our Angular application. Angular needs at least one module to kick things off, and we're going to call our main module "app".

This means that you'll end up with a directory structure like this:

```
├── js/
│   ├── todos/
│   │   ├── directives/
│   │   │   ├── TodoEscape.js
│   │   │   ├── TodoFocus.js
│   │   ├── controllers/
│   │   │   ├── TodoController.js
│   │   ├── services/
│   │   │   ├── TodoService.js
│   │   ├── templates/
│   │   │   ├── partials/
│   │   │   │   ├── TodoForm.html
│   │   │   ├── views/
│   │   │   │   ├── Todo.html
│   ├── login/
│   │   ├── directives/
│   │   │   ├── LoginForm.js
│   │   ├── controllers/
│   │   │   ├── LoginController.js
│   │   ├── services/
│   │   │   ├── LoginService.js
│   │   ├── templates/
│   │   │   ├── partials/
│   │   │   │   ├── LoginForm.html
│   │   │   ├── views/
│   │   │   │   ├── Login.html
│   ├── app.js
│   ├── angular.js
├── img/
├── css/
├── index.html
```

This is a feature based file structure, as we're splitting off into individual modules instead of having all of our controllers/directives/etc. in one folder. Don't be scared - we will be touching on controllers/directives/modules/etc very soon!

# Modules

## Overview

In this lesson we're going to go into modules and how to create and load our own. You'll recognize the syntax from our previous module!

## Objectives

- Describe the setter/getter syntax
- Create a module using a setter
- Fetch the module using a getter
- Import a submodule

## What is a module?

Think of a module as a container for specific parts of your application. Each module should provide a set of functionality (for instance you might have a "login" module that handles everything to do with user authentication and logging the user in). Modules can contain views, components, routes, etc, but that's not important right now (you'll learn all about these very shortly).
 
It's important that we have at least one module - otherwise we couldn't do anything!

Why do we split our application into modules? Modules allow us to separate our application into logical chunks (much like MVC/MVVM that we touched on earlier) that can be reused. It's recommended that you create modules for 

- Each feature in your application
- Each reusable component in your application
- An application module that our application uses as a base

Angular comes with a lot of plugins available on the web. Each one of these will be its own module - meaning that you can just slot it into your application and not have to worry about it interfering with any other aspects of your app!

## Setter/getter syntax

### Modules

Angular is magical, but it isn't *that* magical. We need to tell Angular about all of our modules, controllers, etc, when we create them - otherwise, it would have absolutely no idea what is used for what.

The way we tell Angular about our modules is:

```js
angular
  .module('exampleModule', [])
```

The `angular` object we're using there is the Angular core. This is the core API - this allows us to create and load modules (using `angular.module`).

You might be wondering why we have a second parameter with an empty array in it. This is very important - it tells Angular that we need to *create* a new module. If we omit the second parameter, it would tell Angular that we'd like to *retrieve* a module with the name `exampleModule`.

The empty array is very important for our module too. This is an array of modules that our module depends on. If you were to download a plugin online, you can't just drop the code in and expect Angular to know that it needs to be a part of your application. This is where this array comes in - you can tell Angular that your module depends on the plugin's module that you just downloaded, and it will include it into your application and allow you to use it - amazing!

The number of parameters is true for everything we set/get in Angular. If we only have one parameter, we are telling Angular to `get`. If we have two parameters, we are telling Angular to `set`.


If we were to be doing routing in our app, we'd include Angular's `ngRoute` module. First of all, we'd download the files from the web and make sure we include them via script tags in our HTML file. Then, we'd tell Angular that our module relies on `ngRoute` - otherwise Angular wouldn't know that we actually would like to use the module, even though it exists:

```js
angular
  .module('exampleModule', ['ngRoute'])
```

Simple! That is the `setter` syntax for modules.

### Controllers, directives and everything else
 
Now, we can't just call `angular.controller` for our controllers. Controllers, directives, services, etc, must be attached to our modules. This is done as so:
```js
function ExampleController() {
}

// We assume that the module "exampleModule" has already been created
angular
  .module('exampleModule') // fetch the module
  .controller('ExampleController', ExampleController); // create the controller
  
// both of these do exactly the same thing
var module = angular.module('exampleModule'); // fetch the module
module.controller('ExampleController', ExampleController); // create the controller
```

This is similar to our module setter/getter. Much like `angular` having `angular.module`, all of our modules have the method `.controller`, allowing us to get (or set) the controller.

Why are we fetching the module? Well, we could possibly be in a completely different file. How would Angular know what module our controller belongs to unless we tell it?

We can then retrieve the controller we've created by omitting the second paramater.

```js
// We assume that the module "exampleModule" has already been created
angular
  .module('exampleModule') // fetch the module
  .controller('ExampleController'); // fetch the controller
  
// both of these do exactly the same thing
var module = angular.module('exampleModule'); // fetch the module
module.controller('ExampleController'); // fetch the controller
```

## Using our module in our application

Now we've got Angular aware of our module, we need to tell it where our module actually needs to be used.

### ng-app

We can do this via the `ng-app` HTML attribute Angular makes available for us. All we need to do is find the HTML element where we want Angular to start rendering, and add `ng-app="exampleModule"`.

```html
<div class="app" ng-app="exampleModule">
</div>
```

## Resources
- [Todd Motto's guide for Angular Modules, Setters & Getters](https://toddmotto.com/angular-modules-setters-getters/)
- [Angular Documentation for Modules](https://docs.angularjs.org/guide/module)
<p data-visibility='hidden'>View <a href='https://learn.co/lessons/angular-modules-readme' title='angular-modules-readme'>angular-modules-readme</a> on Learn.co and start learning to code for free.</p>

# Very small [hello world](https://github.com/BrunoBoehm/angular-modules-lab-v-000/blob/cf12223f82ea2fc1143e50f58fe9f53cdf71cc86/index.html) app
Let's create the html
```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Angular Application</title>
</head>
<body>

    <div ng-app="app">
        <div ng-controller="MainController">
            Hello, {{ name }}!
        </div>
    </div>

    <script src="js/angular.js"></script>
    <script src="js/angular-route.js"></script>
    <script src="js/app/app.js"></script>
    <script src="js/app/controllers/MainController.js"></script>

</body>
</html>
```

In our js/app/app.js
```js
angular
	.module('app', [
		'ngRoute'
	])

// python -m SimpleHTTPServer 3000 
// to run server
```

And we create the MainController function we need
```js
function MainController($scope){
	$scope.name = 'Bill';
}

angular
	.module('app')
	.controller('MainController', MainController);
```


