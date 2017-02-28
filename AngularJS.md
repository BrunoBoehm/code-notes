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

## $scope

You might've noticed the magic of `$scope` in the last lesson - an object that we can assign values to and they appear in our view. `$scope` is named by Angular - we can't change the name of it, so make sure you refer to it as `$scope`! 

Angular introduces scopes into our application. Whenever we have a new instance of a controller, that controller has a `$scope`. Everything inside our controller can access this scope, however anything outside cannot. `$scope`s can access their parent scope.

## Using $scope in our views

We can access every property of `$scope` that we set in the controller inside our views. This is done using the `{{ }}` syntax that we used in our first lab. All we need to do is put the property key inside of the double curlys - such as for `$scope.name`, we use `{{ name }}`. We can also use `.` to access properties inside properties (for example, an object).

For example:

```js
function MainController($scope) {
  $scope.contact = {
    name: 'Bill Gates',
    phone: '01234567890'
  };
  
  $scope.year = '2016';
}
```

```html
<div ng-controller="MainController">

  <h2>{{ contact.name }}</h2>
  <h4>{{ contact.phone }}</h4>

  <div>
    Copyright {{ year }}.
  </div>
</div>
```

This would render out:

```html
<div ng-controller="MainController">

  <h2>Bill Gates</h2>
  <h4>01234567890</h4>

  <div>
    Copyright 2016.
  </div>
</div>
```

Neat, huh? 

However, if we tried to access items outside of our controller, such as:

```html
<div ng-controller="MainController">

  <h2>{{ contact.name }}</h2>
  <h4>{{ contact.phone }}</h4>

  <div>
    Copyright {{ year }}.
  </div>
</div>

{{ year }} {{ name }}
```

All we'd get rendered is:
 
 
```html
<div ng-controller="MainController">

  <h2>Bill Gates</h2>
  <h4>01234567890</h4>

  <div>
    Copyright 2016.
  </div>
</div>

{{ year }} {{ name }}
```

![Angular Scopes](https://docs.angularjs.org/img/guide/concepts-scope.png)

Look at the example above - each red rectangle is a scope. If we look at the first rectangle for the HTML element with `ng-controller` - this is currently what we have in our application. From this `$scope`, we'd be able to access the parent `$scope` (the big rectangle around the whole image), but not the `$scope` of the second HTML element with `ng-controller`.

### $rootScope

We also have the `$rootScope` - this is the first scope in our application. All of our `$scope`'s will end up linking to this scope eventually. Not to worry about this yet, you'll come to see it a lot more later on.

# Data-binding
We've talked about data-binding before, but if it's not on the top of your memory you don't need to worry. Data-binding is the term for keeping our model values in sync with our view. Whenever the model changes, our view needs to change too to reflect the change. Whenever our view changes (such as a user typing into an input), we need to update our model with the latest value. This is done via data-binding.

### One-way

The curly braces syntax (`{{ }}`) binds our model value into our view in the form of text. This is one-way bound - the model can update the view, but our view cannot update the model (you can't type into text - only an input!).

We can also bind text into the DOM using a `ng-bind` attribute on the HTML element instead. If we use `ng-bind`, the whole text content of the element is replaced with the value, whereas with the curly braces only the curly braces is replaced. Either can be used, but curly braces are simpler. 

```js
{{ name }}
<span ng-bind="name"></span>
```

These will both display the same thing!

### Two-way

We can two-way bind our data into certain HTML elements - ones that accept input. For example, you guessed it - an `<input />`. This is done with the `ng-model` attribute (more on `ng-model` and `ng-bind` later on).

```js
<input type="text" ng-model="name" />
```

Whenever we type in our `<input />`, our `$scope.name` gets updated, they will both reflect the changes to the value. 

## Watching for updates

We can also watch for updates - how neat! The `$scope` object exposes a few APIs, one of them is `$watch`. `$watch` allows us to pass one of our `$scope` object's keys through, and fires off a callback whenever it updates.

For example:

```js
$scope.$watch('name', function (newValue, oldValue) {
  console.log('name changed from %s to %s', newValue, oldValue);
});
```

We could use this with the two-way data-binding example above. This would fire a `console.log` statement every time the user types in the input!

When would we actually need to use this? For example, in a game we could watch for changes to our name value and broadcast it to everyone else - 

```js
$scope.$watch('name', function (newValue, oldValue) {
  socket.emit('Player ' + oldValue + ' has changed their name to ' + newValue);
});
```

## Overview

We've learned already what `$scope` is and the magic it provides with our view. However, before we start going `$scope` mad, it's important we learn about the best practices when it comes to controllers and our views.

## What is controllerAs?

As you've learned, controllers revolve around the mysterious `$scope` object. However, there is a much cleaner approach to writing our controllers.

Instead of injecting `$scope` into every controller, we can simply just use the `this` keyword and assign items to that instead.
 
There's a slight change in our `ng-controller` syntax too:

```html
<div ng-controller="MainController as main">
</div>
```

Here we tell Angular we'd like to use our controller *as* a variable named `main`. This then creates a new instance of our controller (with all of our values bound to the controller's instance, rather than `$scope`) and assigns the instance to a variable named `main`. This then allows us to access our properties with `{{ main.propertyName }}`.

## Oh no, nested scopes

This helps us when we have nested scopes. If we had a controller inside a controller, and both added the variable `name` to `$scope` - which one would we see?

```html
<div ng-controller="MainController">
  {{ name }} <!-- This is from MainController -->
  <div ng-controller="AnotherController">
    {{ name }} <!-- Where's this from? -->
  </div>
</div>
```

We wouldn't be sure where `name` was coming from in this example. What if we wanted to use `name` from `MainController` instead of `AnotherController` both times? We wouldn't be able to!

That's where controllerAs saves the day - we can assign each controller to different variables and we'd know where `name` is from.

We'd take this:

```js
function MainController($scope) {
  $scope.name = 'Bill Gates';
}
```

And convert it into:

```js
function MainController() {
  this.name = 'Bill Gates';
}
```

Done! That's all it takes. The HTML markup would then look like:
 
```html
<div ng-controller="MainController as main">
  {{ main.name }}
  <div ng-controller="AnotherController as another">
    {{ another.name }} and {{ main.name }}
  </div>
</div>
```

This would render out to be:

```html
<div ng-controller="MainController as main">
  Bill Gates
  <div ng-controller="AnotherController as another">
    Steve Jobs and Bill Gates
  </div>
</div>
```

No more issues with nested scopes!

## What about the rest of $scope

So does this mean we can use `this.$watch` instead of `$scope.$watch`? Unfortunately not - the API for watching value changes still sits with `$scope`, so we still have to use that.

Think of it this way - when we use controllerAs, we're allowing our controllers to assign anything they want our view to access to the `this` keyword. Everything else is as it was before!

# Testing Controllers with Test Driven Development (TDD)

If you haven't done any testing before, your normal coding cycle would be churning out code like there's no tomorrow. However, with Test Driven Development (TDD), you've guessed it - every part of the development we do is driven by tests.

Instead of writing code, fixing bugs, writing more code, blah blah, we start off by writing a unit test. This test will initially fail, as we haven't written any code.

Let's create a function that adds two values together. Instead of going straight into the deep end and writing the function, we write the test first:

```js
describe('Basic Functionality', function () {
  it('should add two numbers', function () {
    expect(addNumbers(1, 3)).toBe(4);
  });
});
```

What's this crazy syntax? When we run our tests, we use syntax that is provided to us by a framework called Jasmine. Jasmine gives us a set of predefined functions (such as `it`, `expect`, etc) that allow us to describe, quite simply, what we want the test to achieve.

Above, we're telling our test runner that in our `Basic Functionality` block, we have a test that expects our number adding function to output `4` when we add `1` and `3` together.

The test will then run, notice we haven't *actually* defined our number adding function, and fail. Right. Let's create the number adding function then!

```js
function addNumbers(numberOne, numberTwo) {
}
```

Great - now we're getting started. Let's run our test again - doh! Failed. We haven't actually done any logic, so the function still won't output `4`, which is what our test is looking for.

Simple - let's just return `4`! Then it passes all of our tests!

```js
function addNumbers(numberOne, numberTwo) {
  return 4;
}
```

Perfect, our tests now pass. Wait! Hold on a minute - when we try and use that function with any other numbers, we're going to always get `4` back! 

How can we test for this? Test it again! Realistically, our tests should always test our functions with many different parameters, expecting correct answers from our functions. That way, if we have a test adding `1` and `3` (to equal `4`), and also adding `50` and `24329` (to equal `24379`), we can, with some confidence, know that our number adding function actually works as we expect.

Let's modify our first test:

```js
describe('Basic Functionality', function () {
  it('should add two numbers', function () {
    expect(addNumbers(1, 3)).toBe(4);
    expect(addNumbers(343, 9283)).toBe(9626);
    expect(addNumbers(1223, 21)).toBe(1244);
    expect(addNumbers(10, 653)).toBe(663);
  });
});
```

Okay, now we've got multiple test cases, let's run our tests again. As expected, our first one passed (as our number adding function is only returning `4`), but the others have failed. Now we need to head back to our number adder and actually implement it correctly.

```js
function addNumbers(numberOne, numberTwo) {
  return numberOne + numberTwo;
}
```

We run our tests again and they all pass - happy days! The code goes into production, but two days later we receive a bug report saying that when people are adding `2` and `2` together, they get `22` instead of `4`. Oh no, what could've gone wrong?

Our function doesn't support strings! In JavaScript, if we were to do `"2" + 2`, we'd get `22` as the result because the `+` operator adds two strings together, as well as actually doing addition between two numbers.

Let's head back to our tests, and add some use-cases for when the function gets called with parameters that don't match what it expects.

```js
describe('Basic Functionality', function () {
  it('should add two numbers', function () {
    expect(addNumbers(1, 3)).toBe(4);
    expect(addNumbers(343, 9283)).toBe(9626);
    expect(addNumbers(1223, 21)).toBe(1244);
    expect(addNumbers(10, 653)).toBe(663);
    expect(addNumbers('99', 1)).toBe(100);
    expect(addNumbers('23', '59')).toBe(82);
  });
});
```

If we run our tests again, they will fail (as we expect). Now, we can head back to our code and make amendments to handle these use cases.
 
```js
function addNumbers(numberOne, numberTwo) {
  return parseFloat(numberOne, 10) + parseFloat(numberTwo, 10);
}
```

Brilliant. Our tests run and pass and we have a well coded, tested piece of code. We can then make changes to this code further, running our tests against it along the way, ensuring no previous expected functionality has been broken.

## Jasmine and Karma

To do TDD, we need to install Jasmine and Karma. Clone this repo completely.

Open your terminal, and run

```bash
sudo npm install -g karma-cli
```

This will install the Karma command line interface tool, so we can run tests using `karma`.

We also need a copy of Karma for our project, as well as Jasmine too.

Karma command line interface starts up the karma server for us, and the local copy for our project allows us to access the API's that karma offers.

```bash
npm install karma --save-dev
npm install jasmine-core --save-dev
npm install karma-spec-reporter --save-dev
npm install karma-jasmine --save-dev
npm install karma-chrome-launcher --save-dev
```

Sorted! Run `karma start` on your command line to see it run (and pass!) our basic test (expecting `'foo'` to equal `'foo'`).

## ngMock - ngWhat?

When we test our code, we need to be able to get deep inside it and see what everything is doing. However, in normal day-to-day coding, we cannot do that - that's where ngMock saves the day.

ngMock is a module that Angular provides us to embed into our tests in order to inject and mock our controllers, directives etc into our tests. ngMock also extends some core services that Angular provides, in order for us to inspect them and control them differently to how they would run in the browser.

To embed this into our tests, all we need to do is tell karma to include it when we specify what files we'd like to test. Take a look at `karma.conf.js` inside this repo - we've done that for you.

## Writing our own unit test

This is where all of the above come in. In this repo (that you should've already cloned!), we've got an example controller called `MainController` inside `js/app/controllers`. We've also got an example test at `tests/MainController.spec.js`. Let's modify this test to inject our example controller.

Let's change the `describe` block to match our controller's name. This way, when we look at the results in the console, we can tell what is actually being tested. (Change "example test" to MainController).

Now, we need to know what app we're using before each test, much like us using `ng-app` in our HTML. To do this, we use `ngMock`'s `module` function.

We can do this like so:

```js
describe('MainController', function() {
  beforeEach(module('app'));
});
```

This tells Jasmine to use the `app` module for every test for our `MainController`.

Now, we need to inject the controller service given to us by `ngMock`. This allows us to call a function with the name of the controller we'd like, as well as specify a custom `$scope` object for the controller to play with.

```js
describe('MainController', function() {
  beforeEach(module('app'));

  var $controller;

  beforeEach(inject(function(_$controller_){
    $controller = _$controller_;
  }));
});
```

Now, inside each of our tests (the `it` should blocks), we can call the `$controller` function, asking for it to return `MainController`.

Let's create a test to check if the name is set correctly inside the controller.

```js
describe('MainController', function() {
  beforeEach(module('app'));

  var $controller;

  beforeEach(inject(function(_$controller_){
    $controller = _$controller_;
  }));

  it('should have Steve Jobs name', function() {
    var $scope = {};

    var controller = $controller('MainController', { $scope: $scope });
    expect(controller.name).toEqual('Steve Jobs');
  });
});
```

If we run `karma start` now, you'll notice our test fails. Go into the `MainController.js` file inside `js/app/controllers` and see if you can figure out why, and see if you can get our unit test to pass (Steve Jobs)!


# What is a Service? Our little helpers

Services are helpers. They're JavaScript functions and are responsible for doing specific tasks only. For example, we might have a `UserService` that communicates with our API for everything user based. Or a `NotificationService` to deal with showing notifications to the user.

Services can be used anywhere and everywhere. It's what makes Angular so useful - we can reuse our code over and over so we don't have to repeat ourselves.

## Where do they come from?

It's all great being able to create services, but how do we get them when we need them? Introducing another piece of Angular magic - Dependency Injection.

Dependency Injection is something you never knew you wanted until it's been given to you - now you won't be able to live without it. Dependency Injection allows us to define *what* we want our controllers to have access to and Angular gives them it!

For instance, we could have 123 different services in our Angular application, but our controller only wants to use three of them. It's important to only include the services we need to use otherwise we may have a massive impact on performance. To do this, all we need to do is specify the three services names as parameters for our controller:

```js
function ContactController(NotificationService, UserService, AuthService) {
  NotificationService.notify('Hello!');
}
```

Angular then, before initiating our controller, looks at what parameters we require and takes a note of them. They are all identified using their names. Then when we initiate our controller, Angular knows exactly what services we need to be "injected".

You'll notice that we've actually done something like this already with `$scope`. We don't have to use `$scope`, but as we specify it as a parameter to our controller, Angular injects it for us. We could have put `$scope` as the second or fifth parameter - it wouldn't matter. 

Our `NotificationService` could look like this: 

```js
function NotificationService() {
  this.notify = function (message) {
    alert(message);
  };
}

angular
  .module('app')
  .service('NotificationService', NotificationService);
```

Notice how we tell Angular about our service in a similar way that we notify it about our controllers - we use `.service` on our modules. We attach all our methods to the `this` keyword as they're properties. Then, we can access all created properties in our controllers. What is important here is the first parameter - this is the name of our service and will be used when we need to inject it elsewhere. The function doesn't need to have the same name, but for debugging purposes, we keep it the same (as it'll be printed in the console if we ever get errors.)


# Injecting a Service

Let's inject one of Angular's built in services into our controller.

For this lab, we're going to make use of `$timeout`. `$timeout` allows us to create timeouts (similar to `setTimeout`) in our applications whilst ensuring that any changes to our model done inside a timeout gets reflected in the view.

`$timeout` accepts the same parameters as `setTimeout` - a function and an integer for how many milliseconds delay.

```js
$timeout(function () {
	// this would be fired after 2 seconds!
}, 2000);
```

Fork and clone this repository to get started. 

Let's create our controller inside `ContactController.js`, this should have two parameters - `$scope` and `$timeout` (it doesn't matter what order you put them in, if you don't believe me - try it!).

Attach the controller to our `app` module.

Now, inside our controller, set `$scope.name` to your name. We previously touched on using controllerAs, but as we need to use two simple services for this lab, we will be using `$scope`.

Below that, set a $timeout for 5 seconds. Inside the function, set `$scope.name` to something else.

Open up `index.html` and wait 5 seconds - you'll see the view get updated with our new name!

# Minification safe Dependency Injection

Minification is great - it reduces our code's file size and allows us to code wonderful things without impacting load times. It strips out all unneeded whitespace and punctuation in order to make our files as small as they can possibly be.

For instance, a minifier might take this:

```js
function add(numberOne, numberTwo) {
	return numberOne + numberTwo;
}
```

And change it into:

```js
function add(a,b){return a+b;}
```

Much smaller! However, it can cause a bit of an issue when mixed with dependency injection.

Most minify-ers change our variable names to single letters - it isn't important what our variables are called, only that they all still refer to the same object/method/function/etc. For example:

```js
function ContactController($scope, $timeout) {
}

angular
  .module('app')
  .controller('ContactController', ContactController);
```

Would become:

```js
function a(b, c) {
}

angular
  .module('app')
  .controller('ContactController', a);
```

Oh no - we don't have services named `b` and `c`. Therefore, when Angular looks at our controller and notices that we require `b` and `c`, it'll throw an error as they don't exist.

How do we overcome this? Well, it's important to know how dependency injection works under the hood first.

## Under the hood

When Angular looks at our function, it takes our parameters and creates an array of what we require. For instance, our controller above would generate the array `['$scope', '$timeout']`. Angular then attaches this array onto a property named `$inject` on our function.
 
```js
function ContactController($scope, $timeout) {
}

angular
  .module('app')
  .controller('ContactController', ContactController);
  
// ContactController.$inject = ['$scope', '$timeout'];
```

Can you see how we can resolve the issues when it comes to minification? We can take over from Angular and specify this `$inject` property ourselves. When the `$inject` property exists, Angular trusts us to know what we want and doesn't look at the controller's parameters - meaning we can name our parameters whatever we want.
 
```js
function ContactController(whatever, weWant) {
  // whatever === $scope
  // weWant === $timeout
}

ContactController.$inject = ['$scope', '$timeout'];

angular
  .module('app')
  .controller('ContactController', ContactController);
``` 

In our example above, the variable `whatever` is still equal to the value of `$scope` - just named differently! The same applies to the variable `weWant`. This means that our minification process will have no side effects to our code.

Another example
```js
function ContactController(a, b) {
	a.name = 'Bill Gates';

	b(function () {
		a.name = 'Steve Jobs';
	}, 5000);
}

ContactController.$inject = ['$scope', '$timeout'];

angular
	.module('app')
	.controller('ContactController', ContactController);
```

# What is a Directive?

Directives are essentially "markers" on a HTML element that tell Angular to attach a specific behavior to the HTML element - either something small like a click event, or actually completely transforming the DOM node to display a list of data.

Directives can be either an actual HTML element or an attribute on a HTML element.

```html
<my-directive></my-directive>

<!-- these are the same -->

<div my-directive></div>
```

However, not all directives can be used in both ways - some are restricted to being just an attribute or just an element. All of the built-in directives from Angular are restricted to being used as attributes. For any other ones (such as custom directives you download online) you will have to check the documentation on how to use it.

## Types of directives

There are two types of directives - event and behavioral. Event directives handle all of the events on a HTML element we might need - for example, `click`, `mouseover`, `keydown`, etc. These are the same as normal JavaScript events that we would add event listeners for.

Behavioral directives are directives that manipulate the DOM. For example, we might have a list that repeats our DOM node for every item in a list. We could even have a directive for hiding and showing content depending on one of our variables' value.

We'll have more on these in the next couple of lessons.

## Built-in directives

Luckily for us, Angular comes with a ton of built-in directives that do what we mentioned above and more. We'll be looking into the built-in ones in the other sections of this unit. Angular's built-in directives are prefixed with `ng-` (you might have noticed we're already using `ng-app` and `ng-controller` - these are just directives!) `ng-app` tells Angular what DOM node to put our application inside of. `ng-controller` tells Angular to attach our controller to that DOM node so we can access the controller inside of the element. In case you hadn't guessed, the `ng-` stands for Angular and it's best practice NOT to use this prefix on our own directives. 

# Event based directives

If you've ever done a lot of JavaScript before, you would have come into contact with event listeners. These are functions that get called whenever a specific behaviour happens that we are looking out for. Either of the following might be familiar to you:

```js
input.addEventListener('click', function (e) {
});

// or

input.onclick = function (e) {
};
```
```html
<!-- or this -->

<input onclick="myFunction()" />
```

These would all fire off of a function when a key was pressed inside of our input.

## Angular's equivalent

Instead, in Angular, we use the built-in `ng-click` directive, and pass in the function we want to be called.

```html
<input ng-click="vm.myFunction()" />
```

## Why do we do this?

We already have a way to do events in JavaScript without any plugins. Why do we have to do things differently inside Angular?

Angular allows us to pass all the event handling over to them. We don't have to worry about browser compatibility or even unbinding our events at the sacrifice of using the built-in directives for event handling instead. A small price to pay for consistent event handling in all browsers. 

Angular also has the concept of a "digest cycle". This, at a high level, is a function that keeps all of our view and model in sync with each other (we will be going into this in a lot more detail later on). The built-in directives ensure that the digest cycle is ran, keeping our view and model synchronized if we were to update our model in response to an event.

## Calling our own functions

As you can see in our example above, we are calling `vm.myFunction()`. An example setup of the controller might look like this:

```js
function ExampleController() {
  this.myFunction = function () {
    console.log('Hey!');
  };
}
```

Which would result in `Hey!` being printed to the console whenever the user types in the input.

As the function is defined in the controller, we also have complete access to all services/model items, allowing us to manipulate them. For example, we might want a button that increments a counter every time it's clicked.

First of all, we'd use the `ng-click` directive provided to us by Angular.

Our HTML would look like this:

```html
<button ng-click="vm.incrementCounter()">Increment the counter!</button>

{{ vm.counter }}
```

And our controller would look like this:

```js
function CounterController() {
  this.counter = 0;
  
  this.incrementCounter = function () {
    this.counter++;
  };
}
```

What's happening here? Well, whenever the user clicks on our button, our `incrementCounter` function gets called. We can then access our model value (`this.counter`) inside that function, and update it itself plus one. Angular then notices that the model has changed, and will update our view to reflect this.

# Behavioral Directives

Behavioral directives are directives that allow us to manipulate the DOM. For example, we could have a repeating list of items or we could hide/show a DOM node depending on a variables value.

Behavioral directives are commonly just used as attributes, and quite a lot are provided by Angular itself.

Imagine we're not using Angular. If we wanted to loop through an array of objects and display them, we'd have to do something like this:


```js
var todos = [{
  title: 'Learn Angular',
  complete: false,
},{
  title: 'Create GitHub profile',
  complete: false
},{
  title: 'Brush teeth',
  complete: true
}];

var list = document.querySelector('ul.todos');

todos.forEach(function (todo) {
  var item = document.createElement('li');
  
  var wording = todo.complete ? 'Completed!' : 'Not completed :(';
  
  item.innerHTML = '<h4>' + todo.title + '</h4> ' + wording;
  
  list.appendChild(item);
});
```

This worked fine but what if our `todos` array was to get updated? We'd have to make sure to only insert the new items into our list, and only remove the correct elements when an item was removed from our array. This could get extremely messy and out of sync with our data if we were not really careful.

## Angular saves the day!

Try and contain your excitement when you see how this is done in Angular.

The HTML:

```html
<ul>
  <li ng-repeat="todo in vm.todos">
    <h4>{{ todo.title }}</h4>
    
    {{ todo.complete && 'Completed!' || 'Not completed :(' }}
  </li>
</ul>
```

Our controller: 

```js
function TodosController() {
  this.todos = [{
     title: 'Learn Angular',
     complete: false,
   },{
     title: 'Create GitHub profile',
     complete: false
   },{
     title: 'Brush teeth',
     complete: true
   }];
}
```

That's everything that we need. Not even joking.

This handles everything - whenever our `todos` array is updated, Angular will update our view to match, removing and adding only the relevant nodes.

## ng-model

Along with `ng-repeat`, `ng-model` is one of the most commonly used directives provided by Angular. `ng-model` works its magic by setting an input's value to a variable set in the controller, as well as updating said variable whenever the user updates the `<input />`'s value.

```html
<input ng-model="vm.username" />
```

This will populate our `<input />` with the value of `vm.username` (which could be empty), and then when the user types in the input, updates `vm.username` to match.

[Check out this JSFiddle](https://jsfiddle.net/pjrfbkku/) showing the power of `ng-model`.

## ng-class

Another task that's quite common in modern applications is dynamically adding and removing classes depending on different variables. For example, we might want to add a complete class on our example above if the todo is complete.

We can do this using the power of `ng-class`. `ng-class` takes an object, with the keys being the class you want to dynamically add, and the value being an expression - when the expression evaluates to `true`, the class is added. When it's `false`, the class will be removed.

For example:

```html
<div ng-class="{'classNameHere': true, 'otherClassName': false}">
</div>
```

In the example above, `classNameHere` would be added to our `<div />` (as the object's value is equal to true), whereas `otherClassName` wouldn't be added as the value is equal to false.

We can swap out `true` and `false` for different values, such as a variable's value. Let' do this for the todo items above.

```html
<ul>
  <li ng-repeat="todo in vm.todos" ng-class="{'complete': todo.complete}">
    <h4>{{ todo.title }}</h4>
    
    {{ todo.complete && 'Completed!' || 'Not completed :(' }}
  </li>
</ul>
```

What is `ng-repeat="todo in vm.todos"` doing for us? We tell Angular what we want to loop over (`vm.todos`) and what variable to put each items properties in (`todo`). We could do `ng-repeat="item in vm.todos"` for example.

`{{ todo.complete && 'Completed!' || 'Not completed :(' }}` looks a bit complicated too, but this is just similar to an expression that we would do in regular JavaScript. This checks the truthy value of `todo.complete`, and if `true` it will display "Completed!". If it's false, it will show "Not completed :(".

Notice that all we have added there is `ng-class="{'complete': todo.complete}"` - this checks our todo item and adds the `complete` class if `todo.complete` is equal to true. It is really that simple!

## Angular Controller and ng Directives Video 

In this video we are going to build a simple CRUD app using controller and built-in `ng` directives.

<iframe width="100%" height="720" src="https://www.youtube.com/embed/2YtGsacxiXE" frameborder="0" allowfullscreen></iframe>

Setting up a dev environment with a live server we're now able to launch the server by `npm run live`.
```
{
  "name": "todo-list-angular",
  "version": "1.0.0",
  "description": "simple todo list with angular",
  "scripts": {
    "live": "live-server",
    "start": "npm run live"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
  	"angular": "^1.5.8"
  },
  "devDependencies": {
  	"live-server": "^0.9.2"
  }
}
```

Our main html file looks like
```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>ToDO App</title>
</head>

<body ng-app="todoApp">
	<h1>Welcome</h1>
	<div ng-controller="ListController as vm">
		<h2>{{ vm.list.name }}</h2>

		<!-- add a task -->

		<button ng-click="vm.startAdd()"
			ng-show="!vm.isInAddMode()">
			Add a Task
		</button>

		<div ng-show="vm.isInAddMode()">
			
			<input placeholder="Task Name"
				ng-model="vm.currentTask.name">
			<button ng-click="vm.add()">Add</button>
			<button ng-click="vm.cancel()">Cancel</button>	

		</div>

		<!-- list of tasks -->

		<div ng-repeat="task in vm.list.tasks | orderBy: '$index' ">

			<!-- normal -read mode version of the task -->

			<div ng-show="vm.isInReadMode(task.id)">
				<p>
					{{ $index +1 }}: {{ task.name }} - completed: {{ task.complete }}
				</p>
				<!-- task options -->
				<button ng-click="vm.startEdit(task.id)">Edit</button>
				<button ng-click="vm.startRemove(task.id)">Delete</button>				
			</div>

			<!-- edit view of the task -->
			
			<div ng-show="vm.isInEditMode(task.id)">
				<input placeholder="task name"
					ng-model="vm.currentTask.name">
				<input type="checkbox" 
					ng-true-value="true"
					ng-false-value="false"
					ng-model="vm.currentTask.complete">
				<button ng-click="vm.save()">Save</button>
				<button ng-click="vm.cancel()">Cancel</button>	
			</div>

			<!-- delete task -->

			<div ng-show="vm.isInRemoveMode(task.id)">
				<p>
					{{ task.name }} - completed: {{ task.complete }}
				</p>
				<button ng-click="vm.remove(task.id)">Remove</button>
				<button ng-click="vm.cancel()">Cancel</button>
			</div>

		</div>

	</div>

	<script src="./js/angular/angular.js"></script>
	<script src="./js/app/app.js"></script>
	<script src="./js/app/list/list_controller.js"></script>
</body>

</html>
```

And our **js/app/app.js**
```js
angular
	.module('todoApp', [])
```

And our controller **js/app/list/list_controller.js**
```js
angular
	.module('todoApp')
	.controller('ListController', ListController);
	// setter, getter

// constructor function
function ListController() {

	// controllerAs syntax
	var vm = this;
	var selectedId = -1; // none of the list tasks
	var addFlag = false;
	var editFlag = false;
	var removeFlag = false;

	vm.currentTask = {};
	vm.startAdd = startAdd;
	vm.startEdit = startEdit;
	vm.isInReadMode = isInReadMode;
	vm.isInEditMode = isInEditMode;
	vm.isInRemoveMode = isInRemoveMode;
	vm.save = save;
	vm.isInAddMode = isInAddMode;
	vm.add = add;
	vm.startRemove = startRemove;
	vm.cancel = cancel;
	vm.remove = remove;
	// we call our protected function

	// kindda like a ruby class method
	vm.list = {
		name: "Main Todo List",
		tasks: [

			{
				id: 1,
				name: "Finish flatiron school",
				complete: false
			},
			{
				id: 2,
				name: "Watch JS tuts",
				complete: false
			},
			{
				id: 3,
				name: "Take some holidays",
				complete: false
			}	

		]		
	};

	// make sure no one is selected and we're in default state
	function reset() {
		selectedId = -1;
		addFlag = false;
		editFlag = false;
		removeFlag = false;		
	}

	function startAdd() {
		reset();
		addFlag = true; // switched on 'isInAddMode' and the div appears
		vm.currentTask = {}; // instantiates a new object
	}

	// shows the form, hides the button
	function isInAddMode() {
		return addFlag;
	}


	// function gets called when 'create task button' is clicked
	// creates the task with the name and complete as false
	function add() {
		// vm.currentTask.name = ... thanks to ng-model
		vm.currentTask.complete = false;
		vm.list.tasks.push(vm.currentTask);
		reset(); // makes the add form disappear
	}

	// tells each task if it is in read (or then in edit mode)
	function isInReadMode(id) {
		return selectedId < 0 || selectedId != id;
	}

	// not in read mode anymore: editmode!
	function isInEditMode(id) {
		return selectedId == id && editFlag
	}	

	function startEdit(id) {
		reset();
		selectedId = id;
		editFlag = true;

		// we search for the current task to populate name and complete
		// like find(id) in ruby
		for ( var i = 0; i < vm.list.tasks.length; i++) {
			var task = vm.list.tasks[i];
			if (task.id == id) {
				vm.currentTask.name = task.name
				vm.currentTask.complete = task.complete
			}
		}
	}

	// will show the delete view template
	function startRemove(id) {
		reset();
		selectedId = id;
		removeFlag = true;
	}

	function save() {
		for (var i = 0; i < vm.list.tasks.length; i++) {
			if (vm.list.tasks[i].id == selectedId) {
				vm.list.tasks[i].name = vm.currentTask.name;
				vm.list.tasks[i].complete = vm.currentTask.complete;
				reset();
			};
		}
	}

	function isInRemoveMode(id) {
		return selectedId == id && removeFlag
	}

	function cancel() {
		reset()
	}

	function remove(id) {
		for (var i = 0; i < vm.list.tasks.length; i++) {
			if (vm.list.tasks[i].id == id) {
				vm.list.tasks.splice(i, 1);
				reset();
			};
		};
	}

}	
```

## Building a simple add button
HTML like
```html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">

    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>Angular Application</title>
</head>
<body>

    <h1>Welcome</h1>
    <div ng-app="app">
        <div ng-controller="ContactController as vm">
            <div>
                <input ng-model="vm.currentTask.name" placeholder="Name">
                <input ng-model="vm.currentTask.phone" placeholder="Phone">
                <button ng-click="vm.addContact()">Add</button>
            </div>            
            <ul>
                <li>
                    <h4>{{ vm.currentTask.name }}</h4>
                    <h6>{{ vm.currentTask.phone }}</h6>
                </li>
                <li ng-repeat="contact in vm.contacts">
                    <h4>{{ contact.name }}</h4>
                    <h6>{{ contact.phone }}</h6>
                </li>
            </ul>
        </div>
    </div>

    <script src="js/angular.js"></script>
    <script src="js/app/app.js"></script>
    <script src="js/app/controllers/ContactController.js"></script>

</body>
</html>

```

And controller
```js
function ContactController() {
    var vm = this;

    vm.currentTask = {};

    vm.contacts = [
        {
            name: 'Bob',
            phone: '0123458690'
        },{
            name: 'Tim',
            phone: '3934203242'
        },{
            name: 'Ross',
            phone: '0684059433'
        }
    ];

    vm.addContact = function () {
        vm.contacts.push(vm.currentTask);
        vm.currentTask = {}; // reinit the currentTask    
    }
}

angular
    .module('app')
    .controller('ContactController', ContactController);
```

# Testing with Protractor

First, let's install Protractor, a library for end-to-end feature testing in Angular. As opposed to Karma and Jasmine, we use Protractor to actually test the HTML side of things. Jasmine only let's us test what's happening programatically. With Protractor, we can click on buttons and enter text into inputs, etc. This way, we're interacting with our site the same way our users will and testing the results that they'll get back.

 To install Protractor, go into your command line and enter:

```bash
npm install -g protractor
```

Make sure this is working by entering:

```bash
protractor --version
```

We then need to update `webdriver`, which is provided to us when we install protractor.

```bash
webdriver-manager update
```

Now, we need to start the Selenium Server. This is a webserver that protractor will communicate with to run our tests.

```bash
webdriver-manager start
```

### Writing a test

Let's write a protractor test.

Protractor lets us grab web pages and interact with elements on the page.

To start, we need to load a page to interact with. For this example, we're going to grab Angular's website. To do this, we'll call the `get` method on our `browser` object and pass in a URL. 

```js
describe('Angular Website', function() {
	it('should add a todo', function() {
		browser.get('https://angularjs.org');
	});
});
```

Now, we can select an element on the page using the `element` function. We can select it via multiple different ways. One way to grab an element is using the `ng-model` selector.

Now, the Angular website features a todo example. The `ng-model` value for the input to add a new todo is `todoList.todoText`. Let's grab the element.

```js
describe('angularjs homepage todo list', function() {
	it('should add a todo', function() {
		browser.get('https://angularjs.org');

		element(by.model('todoList.todoText'))
	});
});
```

Now that we've got the element we can actually write inside it, using `.sendKeys`.

```js
describe('angularjs homepage todo list', function() {
	it('should add a todo', function() {
		browser.get('https://angularjs.org');

		element(by.model('todoList.todoText')).sendKeys('Writing tests!!');
	});
});
```

This will type in the element just as if a user was typing!

Now, we need to actually press the `add` button.

```js
describe('angularjs homepage todo list', function() {
	it('should add a todo', function() {
		browser.get('https://angularjs.org');

		element(by.model('todoList.todoText')).sendKeys('Writing tests!!');
		element(by.css('[value="add"]')).click();
	});
});
```

Here, we're grabbing the add button by its CSS selector and then clicking it - so simple!

Now, that was completely useless unless we can check that the todo actually gets added.

Much like the model selector we used above, we can select elements by their `ng-repeat` value.

```js
describe('angularjs homepage todo list', function() {
	it('should add a todo', function() {
		browser.get('https://angularjs.org');

		element(by.model('todoList.todoText')).sendKeys('Writing tests!!');
		element(by.css('[value="add"]')).click();

		var todoList = element.all(by.repeater('todo in todoList.todos'));
	});
});
```

Simple! As multiple items have the `ng-repeat` on them, we need to use `element.all()` instead of `element()` so we get an array of all of the elements.

If you take a look at the todo list, we originally had two todos and now we should have three. We can test the count of items much like our previous tests, by using `expect`.

```js
describe('angularjs homepage todo list', function() {
	it('should add a todo', function() {
		browser.get('https://angularjs.org');

		element(by.model('todoList.todoText')).sendKeys('Writing tests!!');
		element(by.css('[value="add"]')).click();

		var todoList = element.all(by.repeater('todo in todoList.todos'));
		expect(todoList.count()).toEqual(3);
	});
});
```

Sorted! We are now checking that there are three elements. Let's make sure that the new todo matches the text we put into the input.

```js
describe('angularjs homepage todo list', function() {
	it('should add a todo', function() {
		browser.get('https://angularjs.org');

		element(by.model('todoList.todoText')).sendKeys('Writing tests!!');
		element(by.css('[value="add"]')).click();

		var todoList = element.all(by.repeater('todo in todoList.todos'));
		expect(todoList.count()).toEqual(3);
		expect(todoList.get(2).getText()).toEqual('Writing tests!!');
	});
});
```

We can run this test by running `protractor conf.js`. It will then launch a Chrome instance, go to the Angular website and run our tests.

This simplistic API allows us to click and interact with a lot of different items on our pages, so we can test all of our directives functionality.

Another example
```js
describe('angularjs homepage todo list', function() {
	it('should add a new contact', function() {
		browser.get('http://localhost:8080');

		element(by.model('contact.name')).sendKeys('Tim');
		element(by.model('contact.phone')).sendKeys('3934203242');

		element(by.css('[class="button"]')).click();

		var contacts = element.all(by.repeater('contact in vm.contacts'));
		
		expect(contacts.count()).toEqual(4);
		expect(contacts.get(1).element(by.model('contact.name')).getAttribute('value')).toEqual('Tim');
		expect(contacts.get(1).element(by.model('contact.phone')).getAttribute('value')).toEqual('3934203242');

	});
});
```

# Filtering our datasets

Filters are simple functions that we can use to manipulate (or filter) data in Angular.

We've touched on the `ng-repeat` directive for repeating a list of data - wouldn't it be neat if we could filter on that data too? This comes in handy when we have a large dataset and would like to quickly search through it to find the relevant data.

We can do this using the pipe operator (`|`) after the ng-repeat. The syntax for this is `| filterName: value`. We can use different filters (by changing the `filterName` part) and change what the filter criteria is by changing the `value` part.

The filter that we'd use to filter an ng-repeat by a value is simply named `filter`! We can pass a variable to this filter and it will search the whole dataset for us.

```html
<input ng-model="ctrl.search" />

<ul>
	<li ng-repeat="data in ctrl.data | filter: ctrl.search">
	</li>
</ul>
```

This will filter our dataset by the value of the input data - providing us with a really simple yet powerful search filter! As we type in the input, `ctrl.search` gets updated. Angular notices that we're using `ctrl.search` as a filter and filters the repeat by the value. This means that if we type in "bob", only results containing "bob" will be displayed!

## Display values

There are a few other filters that Angular provides us with, that we don't necessarily use for `ng-repeat`s.

Another powerful filter is `date`. This can take a unix timestamp and returns it nicely formatted for us. We use this with the same syntax as above, passing through a string as the value. The string will change how the date is displayed. Check out the docs to see all the ways we can display a date - there's loads!

```html
<abbr>
	{{ ctrl.date | date:'medium' }} <!-- Dec 25, 2016 2:23:56 PM -->
</abbr>
```

## Filtering in Controllers

We can also filter in our controllers, using the `$filter` service. This is the preferred method for filtering on big sets of data as it helps increase performance.

The syntax to use `$filter` is close to what we used above:

```js
$filter('filterName')(data, 'value');
```

This will then return the filtered data.

If we were to use this instead of the filter in an `ng-repeat`, it would look like the following:

```js
function SomeController($filter) {
	this.list = [{
		name: 'Bob'
	}, {
		name: 'Tom'
	}];

	this.search = 'B';

	this.filteredList = $filter('filter')(this.list, this.search);
}
```

We would then use `filteredList` like normal in our `ng-repeat`:

```html
<ul>
	<li ng-repeat="item in ctrl.filteredList">
		{{ item.name }}
	</li>
</ul>
```

# Testing filters

As we learned earlier, we can use either filters in the DOM or in our controllers.

We're going to be writing our filter's tests using Karma and Jasmine - this means we will have to be using the filters in our controllers as we cannot test our DOM. We do this because our filters directly manipulate data, and that's all we need to test. If they manipulated the DOM, we'd test the DOM.

The tests we are about to write are quite similar to tests we've done before. We will be using a controller that filters a list, and we'll be ensuring that filtered list is correct.

Let's take our basic controller. We've got a list of people, a search term and our filtered list. We've got a function to call to re-filter our list when the search term changes (this could be called when a user presses search or by using `ng-change` on the input to fire it whenever the input changes).

```js
function ContactController($filter) {
    this.list = [{
        name: 'Bob'
    }, {
        name: 'Tom'
    }];

    this.search = 'B';

    this.filteredList = $filter('filter')(this.list, this.search);

    this.changeFilter = function () {
        this.filteredList = $filter('filter')(this.list, this.search);
    };
}

angular
    .module('app')
    .controller('ContactController', ContactController);
```

We can now inject this into our tests and checkout the result of `this.filteredList`.

```js
describe('ContactController', function () {
    var $controller;

    beforeEach(module('app'));

    beforeEach(inject(function (_$controller_) {
        $controller = _$controller_;
    }));


    it('should filter the results correctly', function () {
        var $scope = {};
        $controller('ContactController as vm', {$scope: $scope});

        // $scope.vm holds all of our values
    });
});
```

We can access the filtered list via `$scope.vm.filteredList`. Now, our search term is simply `"B"` - meaning we should only receive an array with Bob in it back from the filter. Let's test this out:

```js
describe('ContactController', function () {
    var $controller;

    beforeEach(module('app'));

    beforeEach(inject(function (_$controller_) {
        $controller = _$controller_;
    }));


    it('should filter the results correctly', function () {
        var $scope = {};
        $controller('ContactController as vm', {$scope: $scope});

        expect($scope.vm.filteredList[0]).toEqual({name: 'Bob'});
    });
});
```

If you run the tests now, they will pass!

Now, we need to test that we can change our filter too. To do this, we're going to change `$scope.vm.search` (our search critera) to `"T"`. We can then call our function to re-filter our list, and check the first result again.

```js
it('should re-filter the results correctly when changing search term', function () {
    var $scope = {};
    $controller('ContactController as vm', {$scope: $scope});

    $scope.vm.search = 'T';

    $scope.vm.changeFilter();

    expect($scope.vm.filteredList[0]).toEqual({name: 'Tom'});
});
```

Another example 
```js
describe('ContactController', function () {
    var $controller;

    beforeEach(module('app'));

    beforeEach(inject(function (_$controller_) {
        $controller = _$controller_;
    }));


    it('should filter by gender', function () {
        var $scope = {};
        $controller('ContactController as vm', {$scope: $scope});

        $scope.vm.search = 'female';
        $scope.vm.changeFilter();

        expect($scope.vm.filteredList.length).toBe(71);
    });

    it('should filter by geography', function() {
    	var $scope = {};
    	$controller('ContactController as vm', {$scope: $scope});

    	$scope.vm.search = 'Manchester';
    	$scope.vm.changeFilter();

    	expect($scope.vm.filteredList.length).toEqual(8);
    });

});
```

# Expressions

Anything in between the `{{}}` braces we've been using in our templates is an "expression". We've only been doing *simple* expressions up to this point, just referencing a variable.

However, expressions can be a lot more powerful. We can add, look up indexes/properties in arrays/objects or even use ternary operators (or do most of what we can do in JavaScript, such as `{{ 1 + 1 }}` outputting 2).

### How long?

One common expression that we may use on a day-to-day basis is the length of an array. For instance, we might want to display how many notifications a user has or how many emails they've got. We can do this purely with the `.length` property - much like how we do it in JavaScript!

```html
{{ someArray.length }}
```

It's as simple as that!

### If this.. or that

We can also use a ternary expression. This takes a boolean value and switches what it displays depending if the boolean is true or false.

```html
{{ someArray.length === 0 && 'No emails!' || 'Lots of emails' }}

<h6>You have {{ ctrl.emails.length == 0 && 'no' || ctrl.emails.length }} emails</h6>
```

`No emails!` will be displayed if `someArray.length === 0`. If it isn't equal to zero, `Lots of emails` will be displayed

# Custom Services

Angular allows us, much like controllers, to create custom services. These are very powerful and can do a lot for us, such as communicating with APIs or manipulating data. We can inject these into any controller we'd like to, meaning we won't be repeating ourselves.

This fits perfectly into our "MVVM" pattern. We can create services (our helpers) to do all the "dirty" work for us (communicate with APIs, etc), and we can then utilise them in our controllers. This keeps our controllers thin and all the business logic inside our services.

## .service()

Our services follow the same setup as our custom controllers. We use `.service` to create them! This is the basic setup of a service:

```js
function SomeService() {
	this.someFunction = function () {

	};
}

angular
	.module('app')
	.service('SomeService', SomeService);
```

Very easy! With services, much like our controllers (when using controllerAs), we attach all of our functions to `this`. These will be publicly accessible by anyone who injects the service. We can also create private functions by using normal functions, as such:

```js
function SomeService() {
	function privateMethod() {

	}

	this.publicMethod = function () {
		privateMethod();
	};
}

angular
	.module('app')
	.service('SomeService', SomeService);
```

## Injecting our services

Much like when we used `$timeout`, we can inject our custom services into our controllers by using their name. To inject our `SomeService` above, we just add `SomeService` as an argument to our controller:

```js
function SomeController(SomeService) {
	SomeService.publicMethod();
}

angular
	.module('app')
	.controller('SomeController', SomeController);
```

# Making HTTP requests

In JavaScript, if we need to make HTTP requests, we'd use `XMLHTTPRequest`. This is quite an outdated aspect of JavaScript, and it didn't provide the most simplistic API:

```js
var request = new XMLHttpRequest();

request.onreadystatechange = function() {
	if (request.readyState === 4 && request.status === 200) {
	  console.log('data loaded!');
	}
};

request.open('GET', 'http://api.com/api/method', true);
request.send();
```
That's a lot of work just to make one HTTP request. Luckily, Angular gives us a better way - enter `$http`.  

## What is $http?

$http is a core Angular service that provides a simplistic API to allow us to communicate with HTTP endpoints with ease. It is a wrapper for `XMLHTTPRequest` to allow us to use a simplistic, easy API.

There are a few ways to do requests - let's take a look at them

### $http()

We can use `$http` as a function, passing through a configuration object with it, as such:

```js
$http({
	method: 'GET',
	url: '/someURL'
});

$http({
	method: 'POST',
	data: {
		username: 'Bill'
	},
	url: '/someOtherURL'
});
```

This will return a promise with the data - we call the `.then` function that the function call returns - passing through a callback function that will get called when the request has finished.

A "promise" is just a specification on implementing certain methods. The $http function returns an object of methods, one of them being `then`. We use `then` to execute a callback whenever the "promise" is resolved (the request has finished loading).

```js
$http({
	method: 'GET',
	url: '/someURL'
})
	.then(function (data) {
		console.log(data);
	});
```

This will load the response from our URL, and allow us to consume the data somewhere after it's returned the response.

### $http.get() and $http.post();

$http also gives us helper functions instead of calling it as a function itself.

They act similar to the example above, but as we already have the method in the name we can just pass through the URL we would like as a string.

```js
$http.get('/someURL');

$http.post('/someOtherURL', { username: 'Bill' });
```

### Using these in our services

Now, any usage of `$http` should be done in a custom service that we create. This makes our controllers nice a thin, and allows us to call our API from anywhere in the application. If we didn't abstract our API calls out into a service, we might end up having two calls to the same endpoint somewhere in our application. If that endpoint then changed, we'd have to look around the application for any usage of the endpoint, instead of just changing it once in one file.

This fits into our `MVVM` architecture - thin controllers and all the logic being done by helpers. It means we have code that can be shared between the whole application.

Let's have a look at how we'd put our `$http` usage into our services.

```js
function UserService($http) {
	this.getLoggedInUser = function () {
		return $http.get('/rest/user');
	}
}

angular
	.module('app')
	.service('UserService', UserService);
```

Here we have a function that we can call in our controllers to get information on the currently logged in user. We're returning the `$http` call so we can then use the `.then` method in our controllers to then update our data. You'd consume this data in a controller as follows:

```js
function HeaderController(UserService) {
	var ctrl = this;

	ctrl.user = '';

	UserService
		.getLoggedInUser()
		.then(function (res) {
			ctrl.user = res.data.username;
		});
}

angular
	.module('app')
	.controller('HeaderController', HeaderController);
```

We call our `UserService.getLoggedInUser()` function and then update our controller's values with the response that's returned. Awesome!

Now, we might have a form to update the user's email address. We'd then use `$http.post()` in our service:

```js
function UserService($http) {
	this.getLoggedInUser = function () {
		return $http.get('/rest/user');
	};

	this.updateEmail = function (emailAddress) {
		return $http.post('/rest/user/email', {email: emailAddress});
	};
}

angular
	.module('app')
	.service('UserService', UserService);
```

We could then call this in our controller, after a button is clicked for example:

```js
function SettingsController(UserService) {

	this.emailAddress = ''; // this is bound to an input via `ng-model`

	this.submitForm = function () {
		UserService
	        .updateEmail(this.emailAddress)
	        .then(function () {
	            alert('Email updated!');
	        });
	};
}

angular
	.module('app')
	.controller('SettingsController', SettingsController);
```

# What is $resource?

$resource is a service that creates a resource object for us to communicate with APIs. We pass through a URL and it gives us an object back that looks like this:

```js
var User = $resource('/user/:userId');

/**
 * User = { get: function (),
 *          save: function (),
 *          remove: function ()
 *        }
 */
```

Here, we've defined our URL. Let's break that down:

`/user/:userId`

We're defining our URL as `/user/:userId` - but hold on, we can't have colons in our URLs!?

Well, with $resource, using a colon in a URL means that section is *actually* a variable. `:userId` means that we aren't actually going to put `:userId` into our URL, rather that we will be replacing `:userId` with an actual user ID. This allows us to put our data into our URL - for instance, we would end up querying `/user/1` or `/user/1231938`. All requests will go to the same URL, unless we don't specify a userId. If we don't specify a userId, it will just go to `/rest/user`.

Now, our `User` variable is equal to an object containing multiple functions. We can call these to make the appropriate requests.

### Usage in a service

To use this in our service, we'd define our `$resource` at the top, and then different methods will use that resource.

```js
function UserService($resource) {
	var User = $resource('/user/:userId');

	this.getUser = function (userId, callback) {
		User.get({userId: userId}, callback);
	};
}
```

And then we can use that as follows in our controllers:

```js
function MyController(UserService) {
	UserService.getUser(3, function (user) {
		console.log(user);
	});
}
```

### Reading

When we call `.get()`, you've guessed it - it makes a GET request for us. As the first argument, we pass in an object with the data we need for request. In the case of our endpoint above, we need to pass in an object with a `userId` property, in order for Angular to replace `:userId` with the actual user's ID.

```js
User.get({userId: 3}, function (user) {
	console.log(user);
});
```

Our second argument is a callback that gets fired when the request completes. This will make a GET request to `/user/3` (as we've passed in 3 as the `userId`).

If we were to still be using `$http`, it would look like this:

```js
$http
	.get('/rest/user/3')
	.then(function (user) {
		console.log(user)
	});
```

### Updating and Creating

Now, we can create new users and also update previous users using the `.$save()` function. This will issue a POST request to the same URL as above.

#### Updating
For existing users, we'd do any modifications inside the `.get` callback, as we already have received the user. Note: we use `$save()` inside the `.get` callback as we may have a property named `save` on the user.

```js
User.get({userId: 3}, function (user) {
	console.log(user);

	user.email = newEmailAddress;

	user.$save();
});
```

#### Creating
When we want to create a new user, we just simply create a new instance of `User`, and then call `.$save()` again.

```js
var user = new User();

user.name = 'Bill Gates';
user.email = 'bill@microsoft.com';

user.$save();
```

This will issue a POST request again, with details about our new user.

The `$http` equivalent is:

```js
$http
	.post('/rest/user', {name: 'Bill Gates', email: 'bill@microsoft.com'})
	.then(function (user) {
		console.log(user)
	});
```

### Removing

We can also delete users, using `.$delete()`.

```js
User.get({userId: 3}, function (user) {
	console.log(user);

	user.email = newEmailAddress;

	user.$delete();
});
```

```js
$http
	.delete('/rest/user/3')
	.then(function (user) {
		console.log(user)
	});
```

# What is a HTTP interceptor?

Now, using `$http` or `$resource` means we're going to end up doing a lot of HTTP requests in our applications.

Eventually, we may need to manipulate all of the requests, or even take a wide-look at all the responses and handle any errors globally instead of on an individual case - we can do this with HTTP interceptors.

We might also want to retry requests if they fail. We can do this with HTTP interceptors!

HTTP interceptors allow us to define functionality that will happen before every request is made, or just after every request has come back from the server. This means we can append things to every request we make, or handle errors that may arise from the backend (it's better to do this in an interceptor and have application-wide error handling, rather than doing it for every possible request we make).

HTTP interceptors are just simple services that we then push into an interceptors array in the `$httpProvider` service.

First, let's look at how we'd make the service:

```
function MyInterceptor() {

}

angular
	.module('app')
	.service('MyInterceptor', MyInterceptor)
	.config(function ($httpProvider) {
		$httpProvider.interceptors.push('MyInterceptor');
	});
	// This last call pushes our new interceptor into the $httpProvider interceptor array
```

If this looks familiar - it is! Now you might be wondering how we actually intercept things.

There are certain functions we can attach to our interceptor service - Angular will look if they exist, and then run them if they do.

### Before a request

Now, to intercept the request before it is actually made, we need to define the function `request` on `this`.

```
function MyInterceptor() {
	this.request = function (config) {
		// this will be fired before each request!
	};
}

angular
	.module('app')
	.service('MyInterceptor', MyInterceptor)
	.config(function ($httpProvider) {
		$httpProvider.interceptors.push('MyInterceptor');
	});
```

We can now manipulate our request by editing the `config` variable passed through. For instance, if we want to add another header to our request, we just modify `config.headers`.

```
function MyInterceptor() {
	this.request = function (config) {
		config.headers['X-Requested-From'] = 'Angular';
		return config;
	};
}

angular
	.module('app')
	.service('MyInterceptor', MyInterceptor)
	.config(function ($httpProvider) {
		$httpProvider.interceptors.push('MyInterceptor');
	});
```

This will append the `X-Requested-From` header to every single request going through `$http`!

### After a request

We can also intercept after a request has been completed (but before we return the results to our controllers/services) by attaching the function `response`.

We might want to log what time the request came back:

```
function MyInterceptor() {
	this.response = function (config) {
		config.config.responseTime = Date.now();
		return config;
	};
}

angular
	.module('app')
	.service('MyInterceptor', MyInterceptor)
	.config(function ($httpProvider) {
		$httpProvider.interceptors.push('MyInterceptor');
	});
```

We can then check when the request was completed in our services!

### After an error

We can just purely kick in our interceptor after an error too. We could have some service to display notifications to the user, and we can invoke this service in our interceptor if there was an error. We can do this by the `responseError` function.

```
function MyInterceptor(NotificationService) {
	this.responseError = function (config) {
		NotificationService
			.showError(config);
	};
}

angular
	.module('app')
	.service('MyInterceptor', MyInterceptor)
	.config(function ($httpProvider) {
		$httpProvider.interceptors.push('MyInterceptor');
	});
```

(NotificationService will be a custom service that we have made, alerting the user when we call `.showError`)

# Testing Services

Our services are grabbing and manipulating data all over the place, and as we may be using them all over our application, it is important that we test our services to ensure that when we change them, we don't break our application.

## Grabbing our services

Previously, we've been using `$controller` to get our controllers, so logically we should be using `$service` to grab our services. Unfortunately, it's not that simple. We use `$controller` to both grab and instantiate our controllers. We don't need to instantiate our services, so we use a nice service named `$injector`!

Let's take a look at how we'd grab our service:

```js
describe('OurService', function () {
    beforeEach(module('app'));

    var OurService;

    beforeEach(inject(function ($injector) {
        OurService = $injector.get('OurService');
    }));


    it('should test our service', function () {
        // We can use OurService here
    });

});
```

It's a little bit different from before, but not an issue! Still really simple.

We can now access all of our public methods our service gives us, and then test the results.

For instance, for our `MathService` that we used earlier on in the series, we can test it as follows -

```js
describe('MathService', function () {
    beforeEach(module('app'));

    var MathService;

    beforeEach(inject(function ($injector) {
        MathService = $injector.get('MathService');
    }));


    it('should add up correctly', function () {
        expect(MathService.sum([1,23]).toEqual(24);
    });
});
```

Yes, it's that simple!

## Testing $http calls

Now, our services are also going to be calling `$http` a lot, and it would be a lot of effort to mock an entire backend. Luckily, ngMocks provides us with a nice little tool called `$httpBackend`. This allows us to mock our API calls.

There are three parts to `$httpBackend`:

### $httpBackend.when

We use `$httpBackend` to setup the responses to our HTTP calls. It accepts a method and a URL, and allows us to define the response that we'd get back.

If we have a backend endpoint at `/rest/user` that responds with the current user's information, we can mock it as follows:

```js
describe('UserService', function () {
    beforeEach(module('app'));

    var UserService, $httpBackend;

    beforeEach(inject(function ($injector) {
        UserService = $injector.get('UserService');
        $httpBackend = $injector.get('$httpBackend');

        $httpBackend.when('GET', '/rest/user').respond({user: 'Bill Gates', email: 'bill@microsoft.com'});
    }));
});
```

### $httpBackend.expectGET

Now, when we actually test the service's function that calls that endpoint, we need to tell ngMocks that we're expecting the request to occur. We do this by calling `$httpBackend.expectGET` with the endpoint we're expecting to have a request to.

```js
describe('UserService', function () {
    beforeEach(module('app'));

    var UserService, $httpBackend;

    beforeEach(inject(function ($injector) {
        UserService = $injector.get('UserService');
        $httpBackend = $injector.get('$httpBackend');

        $httpBackend.when('GET', '/rest/user').respond({user: 'Bill Gates', email: 'bill@microsoft.com'});
    }));

    it('should get the current users information', function (done) {
        $httpBackend.expectGET('/rest/user');
    });
});
```

We're now setup to receive the mocked backend response. Our response will have several properties, with the `data` property referring to the body of our response. In this case, It'll be an object with `name` equal to `Bill Gates` and his email too.

```js
describe('UserService', function () {
    beforeEach(module('app'));

    var UserService, $httpBackend;

    beforeEach(inject(function ($injector) {
        UserService = $injector.get('UserService');
        $httpBackend = $injector.get('$httpBackend');

        $httpBackend.when('GET', '/rest/user').respond({user: 'Bill Gates', email: 'bill@microsoft.com'});
    }));

    it('should get the current users information', function (done) {
        $httpBackend.expectGET('/rest/user');

        UserService
          .getUserInfo()
          .then(function (res) {
            var data = res.data;
            if (data.email === 'bill@microsoft.com' && data.user === 'Bill Gates') {
              done();
            }
          });
	});
});
```

Here, instead of using `expect().toBe()`, we call a function named `done()` if our response is correct. This means that we can do asynchronous tests in Jasmine. An important item to note here is that we must pass the `done` function in as a argument to this test for this feature to work. 

### $httpBackend.flush()

We then need to call `$httpBackend.flush()` to immediately execute all pending requests (which then fires off our request, calling our callback with the returned data and runs the test). If we don't call `$httpBackend.flush()` we'll get a timeout on Karma's test page, the tests waiting to be launched.

```js
describe('UserService', function () {
    beforeEach(module('app'));

    var UserService, $httpBackend;

    beforeEach(inject(function ($injector) {
        UserService = $injector.get('UserService');
        $httpBackend = $injector.get('$httpBackend');

        $httpBackend.when('GET', '/rest/user').respond({user: 'Bill Gates', email: 'bill@microsoft.com'});
    }));

    it('should get the current users information', function (done) {
        $httpBackend.expectGET('/rest/user');

        UserService
          .getUserInfo()
          .then(function (res) {
            if (res.email === 'bill@microsoft.com' && res.user === 'Bill Gates') {
              done();
            }
          });

        $httpBackend.flush();
	});
});
```

All done! We've now got a fully tested `UserService`, including mocked HTTP requests.

Another Example with a UserService
```js
function UserService($http) {
	this.getUser = function () {
		return $http.get('/rest/user');
	};

	this.createFullName = function (user) {
		return user.first_name + ' ' + user.last_name
	};
}

angular
	.module('app')
	.service('UserService', UserService);
```

and the test
```js
describe('UserService', function () {

	beforeEach(module('app'));

	var UserService, $httpBackend;

	beforeEach(inject( function($injector){
		UserService = $injector.get('UserService');
		$httpBackend = $injector.get('$httpBackend');

		$httpBackend.when('GET', '/rest/user').respond({first_name: 'Bill', last_name: 'Gates'});
	}));

	it('Should get the user first and last names', function(done) {
		$httpBackend.expectGET('/rest/user');

		UserService
			.getUser()
			.then(function(res){
				var user = res.data;
				if (user.first_name === 'Bill' && user.last_name === 'Gates') {
					done();
				}
			});

		$httpBackend.flush();	
	});

	it('should add name and last name together', function(){
		var user = {first_name: 'Bill', last_name: 'Gates'};
		expect(UserService.createFullName(user)).toEqual('Bill Gates');
	});
});
```

# Custom Directives

Nowadays, most frontend applications are made up of small, reusable components. If we imagine a car, it would be made up with components such as wheels, windows, engine, etc. In our case, the components that make up our website would be a header, footer, dropdown menu, toggles, etc.

We can create these components using directives. We've already used directives that Angular gives us, but we can also make our own.

Directives can both be HTML attributes and HTML elements. Most of our directives will be HTML elements, as they will have their own template inside of themselves instead of modifying the behaviour of existing elements.

We might create a custom directive called "dropdown". This dropdown directive will have it's own template that will get put into the DOM, so we create it as a HTML element instead.

In plain JavaScript, our dropdown might look [something like this](https://jsfiddle.net/ogyzmx7r). There's a lot of code here that will be common to other elements, such as selecting and replacing elements from the page. Now let's check out [what it is like in Angular](https://jsfiddle.net/ogyzmx7r/1/). You'll notice how we use a custom HTML element (`<dropdown>`) and the code we need is a lot simpler and easier to read! Let's learn all about them.

## Creating a custom directive

Much like services and controllers, we can use the `.directive` function to create our directives.

Our directive names are what we then use to reference them in the DOM. However, as the DOM is case-insensitive, we change the capital letters in our name to hypens. For instance, the directive name `myDirective` becomes `<my-directive></my-directive>`. `userDropdownList` will become `<user-dropdown-list></user-dropdown-list>`.

Let's look at a custom directive:

```js
function MyDirective() {
	return {
		template: '<div>Hello world!</div>'
	};
}

angular
	.module('app')
	.directive('myDirective', MyDirective);
```

You'll notice that in our directive function we return an object - this describes all the functionality and configuration of our directive. The function name is a capital letter but the directive name is always camel case - this is to keep function names consistent when compared to our custom services, filters, etc.

We can then use `myDirective` in the DOM, and it will be replaced by the template you see above.

```
<my-directive></my-directive>
```

Would then be transformed into

```
<my-directive>
	<div>
		Hello world!
	</div>
</my-directive>
```

When rendered by Angular! This is an extremely basic directive, we're going to look into making them much more advanced in the next few readmes!

# Restricting Directive usage

Angular allows us to set a property named `restrict` on the object we return on our directive definition. We can pass through a string with certain letters letting Angular know how our directive can be used.

```js
function MyDirective() {
	return {
		restrict: 'E',
		template: '<div>Hello world!</div>'
	};
}

angular
	.module('app')
	.directive('myDirective', MyDirective);
```

Above, we've got `restrict` set to `E`. This stands for element, meaning our directive can only be used as an element.

The letters available are:

`A` - used as an attribute
`E` - used as an element
`C` - used as a class name
`M` - used as a comment

They would be used as the following:

```html
Attribute - <div my-directive></div>
Element - <my-directive></my-directive>
Class: <div class="my-directive"></div>
Comment - <!-- directive: my-directive -->
```

You might've noticed that we can use directives in class names and comments too. This isn't recommended however, as it is unclear if we're just doing a normal HTML comment or invoking a directive. It also isn't clear if we're adding a class to an element or invoking a directive too.

You can use the individual letters or put them together - `'EA'` will allow the directive to be used as an element and an attribute.

By default, restrict is set to 'EA'.

# Template and templateUrl

## Overview

We've created a directive with a template - let's take a deeper look into that. You might've noticed that we've created a template item in our directive's object. This will be put into the DOM where the directive is invoked. Templates can only contain one root element! If we use more, Angular will error because it can't keep track of the elements properly.

This is okay:
```html
<div>
	Woo our directive goes here!
</div>
```

This will not work:

```html
<div>
	Woo our directive goes here!
</div>
<span>What about here - oh no!</span>
```
## templateUrl

As our templates get more complex, they can become difficult to read inside of our controller function. In the example below, we're saving each line of our template as a seperate element in an array, then joining the elements together. 

```js
function MyDirective() {
	return {
		template: [
        '<div class="dropdown">',
			'<button class="dropdown__title">',
				'Options',
			'</button>',
			'<ul class="dropdown__list">',
				'<li class="dropdown__item">',
					'<a class="dropdown__link" href="#">',
						'Edit',
					'</a>',
				'</li>',
				'<li class="dropdown__item">',
					'<a class="dropdown__link" href="#">',
						'Transfer',
					'</a>',
				'</li>',
				'<li class="dropdown__item">',
					'<a class="dropdown__link dropdown__link--delete" href="#">',
						'Delete',
					'</a>',
				'</li>',
			'</ul>',
        '</div>'
      ].join('')
	};
}

angular
	.module('app')
	.directive('myDirective', MyDirective);
```

To avoid our directives getting too big like above, we can also use `templateUrl` instead, pointing to a HTML file instead. This can be incredibly useful for any large directives.

```js
function MyDirective() {
	return {
		templateUrl: 'directive.html'
	};
}

angular
	.module('app')
	.directive('myDirective', MyDirective);
```

directive.html:

```html
<div>
	Wow, our directive in another file!
</div>
```

This would work the same as if we had it as a string in our directive - neat! 

Normally we'd store our views in a folder inside our module folder `js/moduleName` inside a folder named `views`. This keeps everything all together!

# Replacing base markup

When we put our directive into the DOM, you'll notice that what will happen is this:

```html
<our-directive>
	<div>
		Directive contents here!
	</div>
</our-directive>
```

However, you may not want the `our-directive` HTML element to be there - you might prefer only semantic, proper DOM nodes to be in the DOM. It's completely your choice - there's no benefit to either way.

To get rid of this, we can set the replace property on our object to `true`.

```js
function OurDirective() {
	return {
		replace: true,
		template: '<div>Hello world!</div>'
	};
}

angular
	.module('app')
	.directive('ourDirective', OurDirective);
```

When this is rendered, we won't see `our-directive` in the HTML!


# Inheriting or isolating scope
Now, one cool thing about our directives is that they can display dynamic data. For instance, we might want to have a Twitter card to display a users Twitter handle, along with a link to follow said user.

We can do this exactly like we've done in our views so far - using `{{}}`.

Note: now that we've started using more HTML in our directives, instead of just passing a string, we pass in a multi-line array, joined by an empty string. This means we can have a multi-lined template without having to worry about string concatenation. This is great for small templates, but as our templates get larger we should move out into a separate file.

Lets create the Twitter card.

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/">Follow @username on twitter!</a>',
			'</div>'
		].join(''),
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

Now that's cool, but how do we *actually* get our data? Well, we can actually inherit it from the scope above.

Let's assume we've got a controller, that has a property on it's scope named `twitter`. We would display this in our view as follows:

```html
<div ng-controller="SomeController">
	{{ twitter }}
</div>
```

If we were to use our Twitter card directive instead of displaying `{{ twitter }}`, we could actually use `{{ twitter }}` in our directives template.

```html
<div ng-controller="SomeController">
	<twitter-card></twitter-card>
</div>
```

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ twitter }}">Follow @{{ twitter }} on Twitter!</a>',
			'</div>'
		].join(''),
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

This will result in this HTML being put into the DOM (we're assuming `$scope.twitter` equals `billgates`.)

```html
<div ng-controller="SomeController">
	<twitter-card>
		<div class="twitter">
			<a href="https://twitter.com/billgates">Follow @billgates on Twitter!</a>
		</div>
	</twitter-card>
</div>
```

This is awesome, but what if we want to use our Twitter card twice? Isn't that the point of directives, the ability to reuse them again and again?

One ridiculous way of doing this is to create a new controller for every Twitter card we want to show, assigning all of the different usernames we want to display to their own controller - surely there must be a better way?

## Isolate Scope

Aha! There is! It's called isolate scope. What this does is creates a new scope for our directive - it can either be a completely brand new one or copied from our parent.

In our directive's object, we can attach a property named `scope` to it. This can have a few different values:

### scope: false

If `scope` is equal to `false`, then no new scope is created. This is the same as not having `scope` set (like in our examples above).

### scope: true

If `scope` is equal to `true`, we create a new scope for our directive. This is copied over from the parent scope. If we put `scope` as `true` in our examples above, nothing would change *visually*. However, if we changed the twitter handle, it would update in the controller's scope, but not the directive's.

If we imagine this:

```html
<div ng-controller="SomeController">
	{{ twitter }}
	<twitter-card></twitter-card>
</div>
```

We'd end up with this being rendered:

```html
<div ng-controller="SomeController">
	billgates
	<twitter-card>
		<div class="twitter">
			<a href="https://twitter.com/billgates">Follow @billgates on Twitter!</a>
		</div>
	</twitter-card>
</div>
```

If we were to then update `$scope.twitter` in `SomeController`, we'd end up with:

```html
<div ng-controller="SomeController">
	new value!
	<twitter-card>
		<div class="twitter">
			<a href="https://twitter.com/billgates">Follow @billgates on Twitter!</a>
		</div>
	</twitter-card>
</div>
```

The Twitter handle inside `<twitter-card>` hasn't changed. This is because we've created a new scope (initially based off of the parent scope) - they are no longer linked!

### scope: {}

Now, this is where things get funky. We can pass through an object to our scope object - but why?

When we use normal HTML elements, such as `<input />`, we configure it via attributes. For instance, we might give it a name - we'd do this like such:

```html
<input name="ourInputName" />
```

Hold on a minute.. Can we configure our directives by passing through attributes?

Yes we can!

To do this, we need to specify *what* attributes we want to be configured by. Using our Twitter card above, the only thing we need to configure it is the Twitter handle.

To grab this value out of the attributes, we can specify the attribute name in the object. For instance, if we wanted to utilise the Twitter card as such:

```html
<twitter-card handle="billgates"></twitter-card>
<twitter-card handle="bob"></twitter-card>
```

We'd put the property `handle` into our scope object.

Now, what do we put as the value? There are two that we can use:

#### @ (at)

If we pass through `@` as the value, as so:

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
			'</div>'
		].join(''),
		scope: {
			handle: '@'
		},
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

What `@` tells Angular is to just copy the value as it is. This will literally copy whatever we put in the `handle` attribute and put it into our scope.

#### = (equals)

However, if we want to be able to receive variables through, we can use `=`.

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
			'</div>'
		].join(''),
		scope: {
			handle: '='
		},
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

What this means is that if we have a controller with two properties on it, such as `twitterHandle1` and `twitterHandle2`, we can do this:

```html
<twitter-card handle="twitterHandle1"></twitter-card>
<twitter-card handle="twitterHandle2"></twitter-card>
```

Angular will then change the values over to their actual scope values before passing the value through to our directive. This also means that if we were to update either of them, Angular will automatically update our directive to match it too. Awesome! There's not really any reason why you wouldn't want the directive to update with the latest values, but if you ever do, the option is baked in.

#### Changing attribute names

Notice how we've updated our template to use `{{ handle }}`? This is because our `$scope` has the property `handle` on it now, because of the object.

However, you might not like this - you might have wanted to stick with `{{ twitter }}` but *also* have it configured by using the attribute `handle`. To do this, we can change the object property to `twitter` and we put `handle` after the `@` or `=`, like follows:

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ twitter }}">Follow @{{ twitter }} on Twitter!</a>',
			'</div>'
		].join(''),
		scope: {
            twitter: '@handle'
        },
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

This means we can do this still:

```html
<twitter-card handle="billgates"></twitter-card>
```

Whilst populating `$scope.twitter` with the value. There's no advantage to this or real reason as to why you'd need to use it, but the flexibility is there if you need it.

Another exxample
```js
function ContactCard(){
	return {
		scope: {
			name: '=',
			email: '=',
			phone: '='
		},		
		template: [
			'<div>',
			    '<h4>Contact Card</h4>',
			    '<label>Name:</label> {{ name }}',
			    '<label>Email:</label> {{ email }}',
			    '<label>Phone:</label> {{ phone }}',
			'</div>'
		].join(''),
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('contactCard', ContactCard);
```

And in the view
```html
<div ng-app="app" class="app">
    <div ng-controller="ContactController as ctrl">
        <ul>
            <li ng-repeat="contact in ctrl.contacts">
                <contact-card name="contact.name" email="contact.email" phone="contact.phone"></contact-card>
            </li>
        </ul>
    </div>
</div>
```

## Directive Controllers

We can create controllers for our directives - awesome! We'd generally use these for functions to change data or retrieve data from a service (we might have a list of contacts), or manipulate the data given us (we might want to verify an email address or phone number for a contact).

Our controllers, much like the controllers we've created before, allow us to directly access `$scope` (the values passed through to us from attributes), as well as all the services that we have created, as well as built-in ones too, such as `$timeout`.

## But, how?

You've probably guessed how we can do this in your head. We attach a property named `controller` to our directives object!

This, for now, will take a function that will become our directive's controller:

```html
<twitter-card handle="billgates"></twitter-card>
```

This will be the code to initiate all of our examples. Below, we'll define our controller function and inject `$scope`. Our `$scope` will have a property called `handle` that will be set to `"billgates"`!

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
			'</div>'
		].join(''),
		scope: {
            handle: '@'
        },
        controller: function ($scope) {
            // $scope.handle returns 'billgates' 
        },
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

Now we can use our controller exactly like what we've done before. We can manipulate data, or call a `$timeout`. For example, we could do this:

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ twitter }}">Follow @{{ twitter }} on Twitter!</a>',
			'</div>'
		].join(''),
		scope: {
            handle: '@'
        },
        controller: function ($scope, $timeout) {
            $timeout(function () {
                $scope.handle = 'angularjs'
            }, 5000);
        },
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

This will set our Twitter handle to "angularjs" after 5 seconds.

## controllerAs

As you've learned already, the best practice is in fact to use the `controllerAs` syntax. But we're passing a function through as our controller, so how are we going to tell Angular that we want to use `controllerAs`? You guessed it - the `controllerAs` property.

Much like our `ng-controller`, where we use `SomeController as some`, we use `controllerAs` and put it's value as `some` (or whatever you want to call it).

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
				'<button ng-click="ctrl.changeHandle()">Change Handle</button>',
			'</div>'
		].join(''),
		scope: {
            handle: '@'
        },
        controller: function ($scope) {
            // $scope.handle === 'billgates'

            this.changeHandle = function () {
                $scope.handle = 'angularjs';
            };
        },
        controllerAs: 'ctrl',
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

As you can see here, we're setting our controller as `ctrl`. This means that anything we attach to `this` in the controller will be accessible via the `ctrl` object in our template - exactly like our controllers before!

You might have noticed how `handle` is still on our `$scope` object - this is because it's passed through as a scope property. We'll cover how to get that into `this` soon.

We've got a function to change our Twitter handle. This updates `$scope` - where our data is stored. As we trigger this event using `ng-click`, any changes that we do to `$scope` inside the function is automatically reflected in the view.

## Existing Controllers

Instead of a function, we can pass through a string to the `controller` property as well - much like what we do with `ng-controller`. This means we can use an existing controller as our directive's controller. It also means that we can remove the `controllerAs` property if we're using a string, as we can do `'SomeController as some'`.

```html
<twitter-card handle="billgates"></twitter-card>
```

```js
function TwitterController($scope) {
	// $scope.handle === 'billgates';
}

angular
	.module('app')
	.controller('TwitterController', TwitterController);

function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="https://twitter.com/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
				'<button ng-click="ctrl.changeHandle()">Change Handle</button>',
			'</div>'
		].join(''),
		scope: {
            handle: '@'
        },
        controller: 'TwitterController as ctrl',
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

Here we've got our controller initiated with our scope data, ready for us to manipulate it.

Now we can manipulate our data to our hearts content. Our controllers can also have our services (such as `$timeout` or a custom service) injected so we can utilise the power of them in our directives too!

Example: let's put the username to lowercase
```js
function ContactCard() {
	return {
		scope: {
			name: '=',
			email: '=',
			phone: '=',
			username: '='
		},
		template: [
			'<div>',
				'<h4>Contact Card</h4>',
				'<label>Name:</label>',
				'{{ name }}',
				'<label>Email:</label>',
				'{{ email }}',
				'<label>Phone:</label>',
				'{{ phone }}',
				'<label>Username:</label>',
				'<span class="username">{{ username }}</span>',
			'</div>'
		].join(''),
		controller: function($scope){
			$scope.username = $scope.username.toLowerCase();
		},
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('contactCard', ContactCard);
```

# bindToController

`bindToController` works exactly like our `scope` property, but puts the items into `this` instead of `$scope`. This is awesome - as we're going to be using `controllerAs`, we should have all of our values assigned the same variable.

Let's take a look at how we'd convert our directives over to using `bindToController`:

### Before

```js
function TwitterCard() {
	return {
    		template: [
    			'<div class="twitter">',
    				'<a href="{{ twitter.twitterLink }}/{{ handle }}">Follow @{{ handle }} on Twitter!</a>',
    			'</div>'
    		].join(''),
    		scope: {
    		    handle: '='
    		},
    		controller: function ($scope) {
    			// $scope.handle === 'billgates'

    			this.twitterLink = 'https://twitter.com';
    		},
    		controllerAs: 'twitter',
    		restrict: 'E'
    	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

You'll notice the inconsistency - we're using `{{ handle }}` as well as `{{ twitter.twitterLink }}`. This is data from our scope (passed through to the directive) as well as directive from our controller (using `this` to match best practices). Mixing the two isn't good!

### After

To fix this, we'll add a property called `bindToController` to our directive containing the values we want to have passed through. 

```html
<twitter-card handle="billgates"></twitter-card>
```

```js
function TwitterCard() {
	return {
		template: [
			'<div class="twitter">',
				'<a href="{{ twitter.twitterLink }}/{{ twitter.handle }}">Follow @{{ twitter.handle }} on Twitter!</a>',
			'</div>'
		].join(''),
		scope: {},
		controller: function () {
			// this.handle === 'billgates'

			this.twitterLink = 'https://twitter.com';
		},
		controllerAs: 'twitter',
        bindToController: {
            handle: '='
        },
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('twitterCard', TwitterCard);
```

Order is restored. We now have consistency! All of our values are now added to our controller's `this` object - meaning we're only accessing our data from our controller now!

You might have noticed how we've still got our `scope` property - this is to tell Angular that we do still want a brand new scope - we're just not attaching anything to it.

Another example
```js
function ContactCard() {
	return {
		scope: {},
		template: [
			'<div>',
				'<h4>Contact Card</h4>',
				'<label>Name:</label>',
				'{{ contact.name }}',
				'<label>Email:</label>',
				'{{ contact.email }}',
				'<label>Phone:</label>',
				'{{ contact.phone }}',
				'<label>Username:</label>',
				'<span class="username">{{ contact.username }}</span>',
			'</div>'
		].join(''),
		controller: function(){
			
		},
		controllerAs: 'contact',
		bindToController: {
			name: '=',
			email: '=',
			phone: '=',
			username: '='			
		},
		restrict: 'E'
	};
}

angular
	.module('app')
	.directive('contactCard', ContactCard);
```

And the HTML
```html
<div ng-app="app" class="app">
    <div ng-controller="ContactController as ctrl">
        <ul>
            <li ng-repeat="contact in ctrl.contacts">
                <contact-card
                        name="contact.name"
                        email="contact.email"
                        phone="contact.phone"
                        username="contact.username" />
            </li>
        </ul>
    </div>
</div>
```

## Sharing directive controllers with require

We can require the parent controller for a certain directive by using the `require` property in our directive. We pass in a string, with the directives name that we want.

Following on from our example above, we might have the following setup:

```html
<tabs>
  <tab label="Tab 1">
    Tab 1 contents!
   </tab>
   <tab label="Tab 2">
    Tab 2 contents!
   </tab>
   <tab label="Tab 3">
    Tab 3 contents!
   </tab>
</tabs>
```

If we ran this now, our `tabs` component wouldn't know about the child tabs, and there is no list of tabs to choose from when we want to change what tab is active. This is where require comes in! Require allows us to notify the parent that it exists, so we can have a list of tabs at the top to change the active tab.

Imagine our `tabs` directive looks like this:

```js
function tabs() {
  return {
    restrict: 'E',
    scope: {},
    transclude: true,
    controller: function () {
      this.tabs = [];
    },
    controllerAs: 'tabs',
    template: [
      '<div class="tabs">',
        '<ul class="tabs__list"></ul>',
        '<div class="tabs__content" ng-transclude></div>',
      '</div>'
    ].join('')
  };
}

angular
  .module('app', [])
  .directive('tabs', tabs);
```

You might notice a property we haven't mentioned before - `transclude`. Don't worry about this yet, we're going to learn this very shortly!

In each tab, we've got a label for the tab. We're going to want to put this inside our `tabs__list` list. However, inside the `tabs` component we don't actually know what tabs we have inside the element.

One easy way to populate this list is if each of our `tab` directives notify our `tabs` directive that they exist. In our example, we've got three `tab` elements, so each of these will notify the parent controller.

Imagine our `tab` directive looked like this:

```js
function tab() {
  return {
    restrict: 'E',
    scope: {
      label: '@'
    },
    require: '^tabs',
    transclude: true,
    template: `
      <div class="tabs__content" ng-if="tab.selected">
        <div ng-transclude></div>
      </div>
    `,
    link: function ($scope, $element, $attrs) {

    }
  };
}

angular
  .module('app', [])
  .directive('tab', tab)
  .directive('tabs', tabs);
```

You'll notice we've added `require` with the value `^tabs` - this is telling Angular to require the parent controller from our `tabs` component.

Here, we have a simple sort-of "transparent" directive (our content is passed into our directive, not replaced). What this does (and again, more on the whole tranclusion concept later) is wrap our contents in a new `<div />`. For instance:

```html
<tab label="Tab 3">
	Tab 3 contents!
</tab>
```

Will become:

```html
<tab label="Tab 3">
	<div class="tabs__content" ng-if="tab.selected">
		<div ng-transclude>
			Tab 3 contents!
		</div>
	</div>
</tab>
```

You'll notice we've also got a property named `link`. We're going to learn more about this later, but for now, assume that it is magic and we're going to be using it in this example.

Normally, our link function has three parameters - scope (`$scope`), element (the mounted DOM element) and attrs (the attributes passed through to the directive). However, when we use `require`, we get a fourth - ctrl. This will be equal to the parents controller, allowing us to access everything to do with it.

Let's add an `addTab` method to our `tabs` directives controller. This will add a tab to the list so we can repeat and display the tabs labels at the top of the directive, so the user can click on them to change the active tab.

```js
function tabs() {
  return {
    restrict: 'E',
    scope: {},
    transclude: true,
    controller: function () {
      this.tabs = [];

      this.addTab = function (tab) {
        this.tabs.push(tab);
      };
    },
    controllerAs: 'tabs',
    template: [
      '<div class="tabs">',
        '<ul class="tabs__list"></ul>',
        '<div class="tabs__content" ng-transclude></div>',
      '</div>'
    ].join('')
  };
}

angular
  .module('app', [])
  .directive('tabs', tabs);
```

Now, if we access the fourth argument in our link function inside our `tab` directive:

```js
function tab() {
  return {
    restrict: 'E',
    scope: {
      label: '@'
    },
    require: '^tabs',
    transclude: true,
		controller: function () {
      this.tabs = [];

      this.addTab = function (tab) {
        this.tabs.push(tab);
      };
    },
    controllerAs: 'tabs',
    template: [
      '<div class="tabs__content" ng-if="tab.selected">',
        '<div ng-transclude></div>',
      '</div>'
    ].join(''),
    link: function ($scope, $element, $attrs, $ctrl) {
        // $ctrl = { tabs: [], addTab: func(){} }
    }
  };
}

angular
  .module('app', [])
  .directive('tab', tab)
  .directive('tabs', tabs);
```

You'll see that we can now access the parent's controller and methods! From here, we can add the tab information in to our parent's tab array.

#### Example
```js
function Tab() {
	return {
		restrict: 'E',
		scope: {
			label: '@'
			// from the view <tab label="Tab 1">
		},
		require: '^tabs',
		transclude: true,
		template: [
    	    '<div class="tabs__content" ng-if="tab.selected">',
      	        '<div ng-transclude></div>',
            '</div>'
            // will go inside of <tab label="Tab 1">
		].join(''),
		link: function ($scope, $element, $attrs, $ctrl) {
			$scope.tab = {
				label: $scope.label,
				selected: false
			};
			$ctrl.addTab($scope.tab);
			// $ctrl is the 4th argument, given because we use 'require'
		}
	}
}

angular
	.module('app')
	.directive('tab', Tab);
```

and 
```js
function Tabs() {
	return {
		restrict: 'E',
		scope: {},
		transclude: true,
		controller: function () {
			this.tabs = [];

			this.addTab = function addTab(tab) {
				this.tabs.push(tab);
			};
			
			this.selectTab = function selectTab(index) {
				for (var i = 0; i < this.tabs.length; i++) {
					this.tabs[i].selected = false;
				}
				this.tabs[index].selected = true;
			};
		},
		controllerAs: 'tabs',
		link: function ($scope, $element, $attrs, $ctrl) {
			$ctrl.selectTab($attrs.active || 0);
		},
		template: [
      	'<div class="tabs">',
        	'<ul class="tabs__list">',
          	    '<li ng-repeat="tab in tabs.tabs">',
            	    '<a href="" ng-bind="tab.label" ng-click="tabs.selectTab($index);"></a>',
                '</li>',
            '</ul>',
        	'<div class="tabs__content" ng-transclude></div>',
        '</div>'
		].join('')
	};
}

angular
	.module('app')
	.directive('tabs', Tabs);
```

In the view
```html
<div ng-app="app" class="app">
    <tabs>
        <tab label="Tab 1">
            Tab 1 contents!
        </tab>
        <tab label="Tab 2">
            Tab 2 contents!
        </tab>
        <tab label="Tab 3">
            Tab 3 contents!
        </tab>
    </tabs>
</div>
```

