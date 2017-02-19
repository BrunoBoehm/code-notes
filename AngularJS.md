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

