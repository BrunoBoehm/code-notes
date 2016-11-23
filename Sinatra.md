# Sinatra
Sinatra is a web framework.
A web application framework (WAF) is a software framework that is designed to support the development of dynamic websites, web applications, web services and web resources. The framework aims to alleviate the overhead associated with common activities performed in web development. 

Building dynamic web applications in any language is a complex job requiring intimate knowledge of hundreds of technologies and specifications. The good news, however, is that many of these requirements are universal and every web application must conform to these standards.

For example, any robust web application will need to handle request routing and provide a mechanism for the application to respond to different URLs with the appropriate response. A GET request to /posts/1 must render the HTML for the first post just as a request to GET '/posts/2' will render identically structured HTML but with content for the second post. This is possible because of templates.

Frameworks provide structure and libraries that allow you to focus on your application and not applications in general.

The bigger the framework, the more you can rely on it to provide you with common needs. The smaller the framework, the more you'll have to build things yourself.

Sinatra is a Domain Specific Language implemented in Ruby that's used for writing web applications. Sinatra is Rack-based, which means it can fit into any Rack-based application stack, including Rails. It's used by companies such as Apple, BBC, GitHub, LinkedIn, and more.

Unlike Ruby on Rails, which is a Full Stack Web Development Framework that provides everything needed from front to back, Sinatra is designed to be lightweight and flexible. Sinatra is designed to provide you with the bare minimum requirements and abstractions for building simple and dynamic Ruby web applications. 

It's important to understand the basic concepts of Rails before diving into Rails itself. Sinatra doesn't give you a lot to get started with. There is no way to auto-generate files and directories, no way for the app to make assumptions about routes, or "Sinatra magic".

Sinatra is what is known as a Domain Specific Language, or DSL. A DSL is a specialized, situation-specific language. Sinatra was built expressly for the purpose of creating web applications with Ruby. It is written with Ruby and the code we'll be using to build our Sinatra apps is Ruby code.

## Basic Sinatra Application
First, make sure Sinatra is installed by running `gem install sinatra` in your terminal.
The simple Sinatra app would be and `app.rb` file
```ruby
require 'sinatra'

get '/' do
    "Hello, World!"
end    
```
To check it out type in your terminal `ruby app.rb`
```
$ ruby app.rb
== Sinatra (v1.4.6) has taken the stage on 4567 for development with backup from Thin
Thin web server (v1.6.3 codename Protein Powder)
Maximum connections set to 1024
Listening on localhost:4567, CTRL+C to stop
```
More commonly, Sinatra is used in a modular style encapsulated by Controller Classes and booted via the `config.ru` **Rack convention**.

## Sinatra from Scratch
It's important to note that Sinatra is just a `gem`. It's a library of code that developers wrote to allow us to build light-weight web applications quickly.

```ruby
source "https://rubygems.org"

gem 'sinatra'
```

The first thing you need to do is enter in terminal `bundle install`. Just like software has different versions that require you to update your mobile apps, gems have newer versions. bundle install will **lock in the current versions of the gems for your application**, that way if any updates happen, your app won't break. It keeps the versions locked in a file called `Gemfile.lock` that is created for you.

The `app.rb` file is the heart and soul of a Sinatra application. This is our application controller. The `application controller` handles all incoming requests to our app, and sends back the appropriate responses to the client.

```ruby
require 'sinatra'
class App < Sinatra::Base

  get '/' do 
    "Hello, world!"
  end

end
```
The first line of app.rb is just requiring the Sinatra gem so that we can incorporate its functionality.
Notice we define a class `App` and have it inherit from `Sinatra::Base`. This way, any instance of our class App will have all the functionality of the Sinatra class.

To actually check if our app is working in the browser, enter rackup `app.rb` in your terminal.
Sinatra relies on Rack for its middleware. Because we have the Sinatra gem listed in our Gemfile, we automatically have the Rack middleware setup.

## Modular Sinatra Applications
Web applications, even simple Sinatra ones, tend to require a certain degree of complexity. For example, your application might have multiple routes, route-handlers, and configuration. To handle this, Sinatra is more commonly used through the Modular Sinatra Pattern (over the classical, single file app.rb pattern).

### Config.ru
The first new convention this pattern introduces is a `config.ru` file. The purpose of `config.ru` is to detail to Rack the environment requirements of the application and start the application.
```ruby
# config.ru
require 'sinatra'
require_relative './app.rb'
 
run Application
```
Mounting a controller means telling Rack that part of your web application is defined within the following class. We do this in `config.ru` by using `run Application` where `run` is the mounting method and `Application` is the controller class that inherits from `Sinatra::Base` and is defined in a previously required file.

### Sinatra Controller
`config.ru` requires a valid Sinatra Controller to `run`. A Sinatra Controller is simply a ruby class that inherits from `Sinatra::Base`, here we've called it `Application` but it can be anything relevant to your app. 

The classes that inherit from Sinatra::Base and define the HTTP interface for our application are called Controllers. This inheritance transforms into a web application by giving it a Rack-compatible interface behind the scenes via the Sinatra framework.
```ruby
# app.rb
class Application < Sinatra::Base
 
  get '/' do
    "Hello, World!"
  end
 
end
```
Note our class constant could have been anything descriptive of the functionality provided by the class.

Controllers define an HTTP method using the sinatra routing DSL provided by methods like `get` and `post`. When you enclose these methods within a ruby class that is a Sinatra Controller, these HTTP routes are scoped and attached to the controller.

### Shotgun web server
Normally when we develop simple Rack applications like Sinatra applications, we start our application server with `rackup` and load `config.ru`.

When starting an application with `rackup`, our application code is read once on boot and never again. Once we start the application locally, if we make changes to our code, our running application server will not read those changes until it is stopped and restarted.

`Shotgun` is a small Ruby gem that makes it easier to develop Rack-based Ruby web applications locally by starting Rack with automatic code reloading. That means if you change anything in your code and save it, when you hit 'Refresh' in your browser, your application will respond with the latest version of your code.

A port is just an endpoint on the server that is open for communication. It's typical for a server to regulate the open ports to make sure it can monitor requests appropriately. You'll notice that with rackup you visited the port 9292 but with shotgun 9393.

## Routes
Every time a person clicks on a link, types in a URL, or submits a form, they are making HTTP requests to a specific URL on your application.

Routes are the part of an application that connect HTTP requests to a specific method in your application code built to handle responding to such a request (that part of code is called a Controller Action).

Let's say we have a web application that helps doctors track the medications they've prescribed. The doctor would need an easy way to see all the medicines she's prescribed.
In the application's navigation is a link "All Medicines". When the user clicks on that link, it directs the browser via the link's HREF attribute to the URL "/medicines" on the domain. The URL "/medicines" must trigger the method in your application built to load the appropriate medicines and send an HTML response with that list of medicines.

Also in the application's navigation is a link "All Patients". When the user clicks on that link, it directs the browser via the link's HREF attribute to the URL "/patients" on the domain. The URL "/patients" must trigger the method built to load the appropriate patients and send an HTML response with a list of patients.

These mappings are called Routes.
Mapping http://localhost:9393/medicines to a Sinatra Action for a response:
![route for medicine](https://dl.dropboxusercontent.com/s/unlxkqbg841b1xh/2015-09-15%20at%209.46%20PM.png)
When our doctor types into the browser, http://localhost:9393/medicines, our application gets a request: GET /medicines.
```ruby
get '/medicines' do
    # some code to get all the medicines
    # some code to render the correct HTML page
end
 
# could be written like
get('/medicines') { some code }
```
Once the request has been matched to the route in the controller, called the controller action, then it executes the code inside of the controller action's block, as shown below:
```ruby
# medicines_controller.rb
 
get '/medicines' do
  @medicines = Medicine.all
 
  erb :'medicines/index.html.erb'
end
```
Finally, the `@medicines` collection of objects is rendered via the `index.html.erb` file: views/medicines/index.html.erb. This file is the HTML (+ some Ruby code––more on that later) that we want our users to see when they request to see all of the medicines. The HTML returned from the template is sent by the controller action as a response to the user.

If no matching route for the web request is found, our application will respond with a `404` informing the user's browser that our application cannot find a match for that request URL.

Mapping http://localhost:9393/patients to a Sinatra Action for a response:
![route for patients](https://dl.dropboxusercontent.com/s/t3mgmc0qwr9hfsi/2015-09-15%20at%209.48%20PM.png)

Users interact with our application by requesting specific URLs via HTTP. URLs are the interface to a web application. How does our application know what to show a user based on the web request they sent? Through the routes we program in our application. Being able to draw a route in a web application to respond to an HTTP request is the absolute first step in building anything on the web.

More routes
```ruby
get '/' do
  .. show something ..
end

post '/' do
  .. create something ..
end

put '/' do
  .. replace something ..
end

patch '/' do
  .. modify something ..
end

delete '/' do
  .. annihilate something ..
end

options '/' do
  .. appease something ..
end

link '/' do
  .. affiliate something ..
end

unlink '/' do
  .. separate something ..
end
```
Routes are matched in the order they are defined. The first route that matches the request is invoked.

## Intro to MVC
We could create a web application in one file, with thousands of lines of code in the same document. It would work. But it would also present us with some very big challenges. It would be close to impossible to debug our program, and our code would be virtually unreadable.

Instead, we use frameworks (Sinatra being one of them), to separate an application's code by function and make writing, reading, and debugging code a much more pleasant and simple experience.

This concept is called **separation of concerns and single responsibility**. Each file in our application will have a different responsibility and we'll keep these responsibilities split up into reasonable chunks.

The Model-View-Controller paradigm is a popular way of building frameworks for web applications - it provides a separation of concerns where groups of files have specific jobs and interact with each other in very defined ways. In a nutshell:
- **Models**: The 'logic' of a web application. This is where data is manipulated and/or saved. First, there are the cooks (the models) that make the food. They take orders (from the waiter), and prepare the customer's meal. Once ready, they give it to the waiter to deliver to the customer.
- **Views**: The 'front-end', user-facing part of a web application - this is the only part of the app that the user interacts with directly. Views generally consist of HTML, CSS, and Javascript. The customers (views) place orders and receive food. The orders are placed with the waiter, who takes them back to the kitchen.
- **Controllers**: The go-between for models and views. The controller relays data from the browser to the application, and from the application to the browser. They are the waiters in the restaurant, going between the kitchen and the front of the restaurant. They take requests from the customer to the kitchen, and take prepared meals from the kitchen to the customer.

### File structure
Keeping our code organized is crucial when developing complex applications. This concept is called separation of concerns and single responsibility. Each file in our application will have a different responsibility and we'll keep these responsibilities split up into reasonable chunks.

```ruby
├── Gemfile
├── README.md
├── app
│   ├── controllers
│   │   └── application_controller.rb
│   ├── models
│   │   └── model.rb
│   └── views
│       └── index.erb
├── config
│   └── environment.rb
├── config.ru
├── public
│   └── stylesheets
└── spec
    ├── controllers
    ├── features
    ├── models
    └── spec_helper.rb
```
In some simple applications the Application Controller will simply be called `app.rb` and will live in the root directory of the project.
Sometimes our other controllers will use `ApplicationController` as an inheritance point so that they inherit all the defaults and behaviors defined in our main ApplicationController. Other times our other controllers will simply inherit from `Sinatra::Base`. 
For instance in `application_controller.rb`, we can have an `ApplicationController` class that inherits from `Sinatra::Base`. When we start up a server, the server will spin up an instance of the ApplicationController class to run our app.

You'll also notice there is a `configure` block already in the controller. This configure block tells the controller where to look to find the views (your pages with HTML to display text in the browser.) and the public directory.

In a Sinatra app we use `.erb` files instead of `.html` files because .erb files allow us to include regular, old HTML tags AND special erb tags which contain Ruby code. We can name them anything we like, but by convention, our file names will match up with the action that renders them.

A `config.ru` file is necessary when building Rack-based applications and using rackup/shotgun to start the application server (the `ru` stands for rackup). `config.ru` is first responsible for loading our application environment, code, and libraries. Once all our code is loaded, `config.ru` then specifies which controllers to load as part of our application using `run` or `use`.

The `config` directory holds an `environment.rb` file. We'll be using this file to connect up all the files in our application to the appropriate gems and to each other. This environment.rb file loads Bundler and thus all the gems in our Gemfile, as well as the app directory

The `public` directory holds our front-end assets. In the example above, it holds a `css` directory with a stylesheet. `Javascript` directories and any other front-end assets (like `image` files) should also be stored in public.

The spec directory contains any tests for our applications. These tests set up any expectations for the rest of the project. These are often broken down into unit tests for models, controller tests for routes, and feature tests, which check the actual behavior for users.

### Sinatra Views
Rendering plain text is a great way to test the behaviors of our routes, but it doesn't give us any control over how the content is displayed. We'd like to structure our content using HTML and render that to the browser instead. The most basic way to do this is to include html tags as a part of the string we're rendering.
```ruby
get '/' do
    "<h1>Hello World</h1>"
end
```
You can imagine that writing HTML this way would get very messy, very quickly. Luckily, most web frameworks include a templating engine that allows us to render HTML content from a different file. We call these files "views." Views represent what the user sees and comprise the "V" in "MVC."

By default, Sinatra uses a templating engine called `ERB`, or Embedded RuBy. Sinatra is also configured by default to look for our .erb files in a directory called `/views`. No need to put the file .erb extension.
```ruby
# app.rb (controller)
get '/' do
  erb :index
end

get "/info" do
  erb :info
end
```
It's important to note that the name of the file doesn't have to match the name of the route. For example, if we wanted our "/info" route to render a file called dogs.erb, we could. By convention though, we keep our routes and our erb files named the same. This makes it easier to keep track of as our projects get bigger.

### Using ERB
Most major web frameworks provide some means of view templating, i.e. allowing the view, in our case an HTML document, to be constructed using a combination of HTML and Ruby code.

This allows us to greatly reduce duplication of HTML, as well as generate HTML that can change based on the data that is available. This is how Facebook can support 2 billion users - they create one template for the profile page which gets filled in with data from their servers representing each user.

ERB and other templating engines allow us to modify the content and structure of our HTML code. With ERB, we do this using two different types of tags 
- the substitution, or `<%= ... %>` tag
- the scripting tag, or <% ... %>` tag.

The substitution tag evaluates ruby code and then displays the results into the view. It opens with `<%=` and closes with `%>`. In general, we use the substitution tags when we want to display some evaluation on the page.
```ruby
<%= "I love " + "Ruby!!" %>
```
The strings are concatenated first, then displayed on the page. 

Scripting tags open with `<%` and close with `%>`. They evaluate, but do not actually display, ruby code.
```ruby
<% if 1 == 2 %>
    <p>1 equals 2.</p>
<% else %>
    <p>1 does not equal 2.</p>
<% end %>

<% if logged_in? %>
    <a href="/logout">Click here to Log Out</a>
<% else %>
    <a href="/login">Click here to Log In</a>
<% end %>

<% squares = [1,2,4,8] %>
<ul>
<% squares.each do |square| %>
    <li><%= square %></li>
<% end %>
</ul>

<%# This is just a comment %>
```

## Intro to Capybara tests
In the MVC framework, there are 3 different levels of testing.
![Common Web App Stack and Tests](https://dl.dropboxusercontent.com/s/k2ypcn86btb6ajo/2015-09-29%20at%204.14%20PM.png)

1. Database: Databases persist or save data from our application.
2. Models: Models provide an object-oriented abstraction or metaphor for the data in our application. Our models do the job of interacting with the database for us. Our model can talk to the database by asking for certain data and using that data to create a new instance of our class. We can then interact with that data by using the methods and properties of the instance of that class.
3. Controllers: Controllers provide the main interface and application logic. They deal with things like: "What data should I show a user in response to certain input from that user" or "What HTML should be sent to the user when they visit the /about page?". In large scale MVC applications, controllers are represented by classes, but really, lots of `bin` files could be considered controllers.
4. Views: Views present information to the user. Any code that is responsible for presenting data or output to the user, from methods that use a bunch of puts, to HTML, to ERB templates, could be considered a View. In web applications, Views are generally represented by ERB templates that generate HTML.
5. User/Browser: The top of our application pyramid is finally the user. Whether describing the people using our application, the interface they use such as a Command Line, Voice, or HTML, or the program they use to even access our application, such as a Browser like Chrome or Safari, or a Native app, our application is responsible for delivering the user an experience on some sort of pre-existing platform.

There are 3 levels of testing

1. Unit tests test the models in our application and how they interact with our database.
2. Controller tests test that the code responsible for delivering the appropriate data to a user is working properly. Controller tests should test not HTML or forms but, rather, that the controller is behaving as expected.
3. Integration tests are the highest-level test, and they are closest to describing how a user will actually interact with our application. Integration tests are the highest-level test, and they are closest to describing how a user will actually interact with our application. 

Capybara is a library of code that we can include in our application via the Capybara gem. The Capybara library allows us to write code that simulates how a user interacts with our app. We can write such code in our integration tests and thus test the functionality of our application.

Colloquially we could express the tests for this application as follows:
```
When a user visits '/'
  they should see a greeting
  they should see a form with a name field
 
When a user submits the greeting form
  they should fill in the name with "Avi"
  they should click submit
  they should see "Hi Avi, it's nice to meet you!"
 ``` 

Let's start writing
```ruby
require 'spec_helper'
 
describe "GET '/' - Greeting Form" do
  it 'welcomes the user' do
    visit '/'
    expect(page.body).to include("Welcome!")
  end
end
```
Most of that code is actually vanilla RSpec. Capybara provides two new methods, `visit` and `page`.

The `visit` method navigates the test's browser to a specific URL. It is equivalent to a user typing a URL into their browser's location bar. The argument it accepts is a string for the URL you want to test. Since we want to test our `'/'` root URL, we say visit '/', and Capybara will load that page within our test.

The page method in Capybara exposes the "session" or "browser" that is conceptually (and literally) being used during the test. The page method gives you a `Capybara::Session` object that represents the browser page the user would actually be looking at if they navigated to `'/'` (or whichever route was last passed to visit). As such, page responds to a lot of methods that represent actions a user could take on a page, such as `click_link`, `fill_in`, and `body`.

Capybara page objects respond to methods that relate intimately to HTML and the DOM (Document Object Model) that defines web applications. You can literally ask the page object: "Hey, do you have HTML that matches the following selector?".

```ruby
require 'spec_helper'

describe "get '/' - Greeting Form" do
  it "welcomes the user" do
    visit '/'
    expect(page.body).to include("Welcome!")
  end

  it "has a greeting form with a user_name field" do
    visit '/'
    expect(page).to have_selector("form")
    expect(page).to have_field(:user_name)
  end

end

describe "POST '/greet' - User Greeting" do
  it 'greets the user personally based on their user_name in the form' do
    visit '/'
    fill_in(:user_name, with: "Avi")
    click_button "Submit"

    expect(page).to have_text("Hi Avi, nice to meet you!")
  end
end
```

To wrap up we just used
- `visit` & `page` – Capybara methods for controlling the test user's browser and examining the current state of the page as they 'see' it.
- `have_selector`, `have_field`, & `have_text` – Capybara matchers for ensuring that the page contains certain matching HTML or text.
- `fill_in` & `click_button` – Capybara methods for mimicking user activities, such as filling in form fields or clicking buttons.

## Dynamic routes
How does AirBnB create a separate url for every property it hosts on its site? Would it make sense to hard-code hundreds of thousands of routes (get '/property1', get '/property2',get '/property2356') in the controller to display each rental property?

Instead, AirBnB (and Twitter, and Facebook, etc) use dynamic routes - routes that are created based on attributes within the url of the request. In this code-along we'll learn why dynamic routes are powerful and how to integrate them in to a Sinatra project.

URL params help us get the text from the URL into the views. That :name in the route name is just a symbol that will be filled in with text later. The data is passed from the URL to the controller action through an automatically generated hash called params. Don't worry too much how the hash is created. Just know that inside your controller action, you automatically have access to this hash through the variable params.

The key of the hash is determined by the symbol in the url (:id), and the associated value will be the content in the url provided by the user (1). Once inside the controller action, we can access the value from the params hash, just like we would any other hash: `params[:id]`.

You can receive multiple pieces of data through a dynamic route by separating the content with a forward slash. For example, get '/addnumbers/:number1/:number2' would give you a params hash with two key-value pairs (number1 and number2).

In some of the tests you might notice `%20` between words in a URL. URLs are not allowed to have spaces in them. Something like this www.facebook.com/flatiron school would never work as a website name. The % sign in a URL is called URL encoding. Basically, it replaces unsafe characters for a URL with appropriate ascii characters.
You can take a look at [this list](http://www.degraeve.com/reference/urlencoding.php) for a reference on unsafe url characters, and how to use url encoding to correct them.

Remember that values in params always come in as strings, and your return value for the route should also be a string (use `.to_i` and `.to_s`).

Remark: In Sinatra, the order in which you define your routes in a controller matters. Routes are matched in the order they are defined. So, if we were to define the get `'/posts/:id'` route before the get `'posts/new'` route, Sinatra would feed all requests for `posts/new` to the `posts/:id` route and we should see an error telling us that your app is unable to find a Post instance with an id of "new". The takeaway is that you should define your `/new` route before your `/posts/:id` route.

## HTML forms
Think about how many forms you fill out online every day. Credit card payments, logins, registration forms, and even Google searches are all examples of forms. That's because forms are the most common way for users to pass data to a web application.

Here's a simple example
```ruby
# in views/food_form.erb
<form method="POST" action="/food">
  <p>Your Name: <input type="text" name="name"></p>
  <p>Your Favorite Food: <input type="text" name="favorite_food"></p>
  <input type="submit">
</form>
```
The `method` attribute tells the form what kind of request should be fired to the server when the submit button is clicked. In general, forms use `POST` request, because it is 'posting' data to the server.
The `action` attribute tells the form what **specific route the post request should be sent to**. In this case, we're posting to a route called /food.
Each form field `<input>` also must define a `name` attribute. The name attribute of an <input> defines how our application will identify each <input> data.

If you create a text field input with <input type="text" name="favorite_food">, whenever the user submits that form, you will be able to access the data entered into the Favorite Foods text box via `params[:favorite_food]`.
```ruby
params = { 
  :name => "Sam",
  :favorite_food => "Green Eggs and Ham"
}
```
When you submit your form, you should now see the contents of params displayed as a hash in your browser like this:
```ruby
post '/food' do
    params.to_s
end

{"name"=>"Sam", "favorite_food"=>"Green Eggs and Ham"}
```
Great! This means you've been able to successfully get the data from the form in to the controller, and can now manipulate it any way you want.
```ruby
post '/food' do
  "My name is #{params[:name]}, and I love #{params[:favorite_food]}"
end
```
Make sure to use `''` and not `""` in the routes definition.

You can have several type of inputs 
```html
<label for="POST-name">Name:</label>
<input id="POST-name" type="text" name="name">

<label for="POST-email">Email:</label>
<input id="POST-email" type="email" name="email">

<label for="POST-newsletter">Sign Me Up for the Newsletter!</label>
<input id="POST-newsletter" type="checkbox" name="newsletter">

<label for="POST-item1">Item1:</label>
<select id="POST-item1" name="item[0][name]">
    <option value="bigfoot">Bigfoot the Holiday Yeti Holiday Ornament</option>
    <option value="guineapig">Santa Guinea Pig Outfit</option>
    <option value="beardski">Bearded Ski Masks</option>
    <option value="inflatable">Inflatable Santa Suit</option>
    <option value="uglyhan">Ugly Hanukkah Sweater</option>
    <option value="donut">Donut Behind Glass</option>
    <option value="pizza">Pizza Slice-Shaped Bean Bag Chair</option>
</select>

<label for="POST-item1_quant">Quantity:</label>
<select id="POST-item1_quant" name="item[0][quant]">
    <option value="0">0</option>
    <option value="1">1</option>
    <option value="2">2</option>
</select>

<label for="POST-item">I will bring:</label>
<input type="radio" id="wine" name="item" value="Wine">Wine</input>
<input type="radio" id="cheese" name="item" value="Cheese">Cheese</input>
<input type="radio" id="crackers"  name="item"value="Crackers">Crackers</input>

<button type="submit" value="RSVP">RSVP</button>
```

## Passing Data between views and controllers
Why is passing data back to views from your controller so important? It allows us to make your pages dynamic rather than static - that is, the data can change depending on the inputs provided by the user.

Instance variables allow us to bypass scope between the various methods in a class. Creating an instance variable **in a controller method** (route) lets the contents become 'visible' to the erb file to which it renders. Instance variables are ONLY passed from the controller method where they are created to the view that is rendered, not between controller methods.

```ruby
post '/reverse' do
    original_string = params["string"]
    @reversed_string = original_string.reverse
    erb :reversed
end
```
In order to show the content of a Ruby string, we need to use erb tags.
```html
<!DOCTYPE html>
<html>
 <head>
  <meta charset="UTF-8">
  <title>title</title>
  <link rel="stylesheet" href="stylesheets/style.css" type="text/css">
 </head>
 <body>
  <h1> Your Reversed String!</h1>
    <h2><%= @reversed_string %></h2>
 </body>
</html>
```

## Layouts and Yield
If you look at pretty much every website, you'll notice that there are things that exist across all of the site's pages. Typically, the navigation bar and the footer content stay the same. There may also be menu options that stay consistent across all pages.

You could copy and paste the HTML and ERB for nav bar and make sure that code is in every single erb file, but that isn't at all DRY.

In order to not repeat ourselves, we can create a single file, `layout.erb`, that contains all of the code we want to exist on every single web page.
In layout.erb, we need to add a yield wherever we want the other page content to be loaded:
```html
<!doctype html>
<html>
  <head>
    <title>Cats</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/style.css">
  </head>
  <body>
 
    <div class="container">
      <h1>I love cats</h1>
      <img src="https://s3.amazonaws.com/after-school-assets/cat-typing.gif">
      <%= yield %>
    </div>
    
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js"></script>
  </body>
</html>
```
We have a controller action
```ruby
get '/' do
    erb :index
end
```
When the above controller action is triggered and the erb method is called, it looks to see if there is a view titled `layout.erb`. If that file exists, it loads that content around the desired erb file, in this case `index.erb`.
```html
<h2>This cat...<h2>
<img src="https://s3.amazonaws.com/after-school-assets/cat.gif">
```

## Integrating models for a Full MVC
So far, we've applied logic to data provided by the user directly in our application controller. While this works, it does not follow the principle of **'separation of concerns' - our files should do one thing and one thing only**. In this code-along lesson, we'll learn how to move the logic to a model in a Sinatra application.

We're going to create a web application that analyzes a block of text from the user - showing the number of words, most common letters, and least common letters to us.

We'll use input from a form to create an instance of a model, and then send that instance back to a view to be displayed to the user.

We could analyze all of the data from `params[:user_text]` in our application controller, but our route would get messy very quickly. Instead, let's create a new `class` inside of our models directory that will take care of the analysis of the text. In your models directory, create a new file called `textanalyzer.rb`.
```ruby
class TextAnalyzer
  attr_accessor :text
 
  def initialize(text)
    @text = text
  end
 
  def count_of_words
    @text.split(" ").count
  end
 
  def count_of_vowels
    @text.downcase.scan(/[aeoui]/).count
  end
 
  def count_of_consonants
    @text.downcase.scan(/[bcdfghjklmnpqrstvwxyz]/).count
  end
 
def most_used_letter
    all_letters_in_string = @text.downcase.gsub(/[^a-z]/, '').split('') #gets rid of spaces and turns it into an array
    letters_to_compare = all_letters_in_string.uniq
    most_used_letter = ""
    letter_count = 0
 
    letters_to_compare.map do |letter|
      letter_repetitions = all_letters_in_string.count(letter)
      if letter_repetitions > letter_count
        letter_count = letter_repetitions
        most_used_letter = letter
      end
    end
    biggest = [most_used_letter, letter_count]
  end
 
end
```
The model above has an initializer which takes in a string text and saves it to an instance variable `@text`. This instance variable is then used in the four instance methods, which provide information on the block of text in question.

In general our models are agnostic about the rest of our application - we could drop this class into a Command Line or Ruby on Rails app and it would function in the exact same way.

```ruby
# in app.rb

post '/' do
  @analyzed_text = TextAnalyzer.new(params[:user_text])
  erb :results
end
```
This responds to a form `<form method="post" action="/">`.

In our `results.erb` file, use erb tags to display the data stored in the `@analyzed_text` variable.
```html
<h1> Your Text Analysis </h1>
<h2>Number of Words: <%= @analyzed_text.count_of_words %></h2>
<h2>Vowels: <%= @analyzed_text.count_of_vowels %></h2>
<h2>Consonants: <%= @analyzed_text.count_of_consonants %></h2>
<h2>Most Common Letter:<%= @analyzed_text.most_used_letter[0] %> used <%= @analyzed_text.most_used_letter[1] %> times</h2>
```

## Nested forms for Sinatra
In web apps, **we use forms to create objects**. When you fill out a form for a dinner reservation on Open Table, you're creating a reservation object. When you upload a photo to Instagram, you're creating an image object.

What if you wanted to use a form to create more than one object? This is where nested forms comes in.

Imagine we need to create each student and their class schedule. It would be tedious to go through the steps to first create the student and then go through the same steps again and again to create each of that student's courses. Wouldn't it be nice to create the student and their courses in one go?

To create these two different classes of objects, we need to create two models, `Student` and `Course`. It's important to **initialize the objects using hash arguments keys** (because this is how they will be passed).
Note the `STUDENTS` class constant can also be used as `@@students` class variable.
```ruby
class Student
  attr_reader :name, :grade
  STUDENTS = []
 
  def initialize(params)
    @name = params[:name]
    @grade = params[:grade]
    STUDENTS << self
  end
 
  def self.all
    STUDENTS
  end
end

class Course
  attr_reader :name, :topic
  COURSES = []
 
  def initialize(args)
    @name = args[:name]
    @topic = args[:topic]
    COURSES << self
  end
 
  def self.all
    COURSES
  end
end
```
Let's now create the form. Before we dive into the HTML, let's think about how we want to structure the data our controller action will receive. We need to think about structuring our params hash to have nested hashes.
```ruby
params = {
  "student" => {
        "name" => "Joe",
        "grade" => "9",
        "course" => {
          "name" => "US History",
          "topic" => "History"
        }
  }
}
```
ERB handles that first level of nesting, so instead of having to do `my_hash["student"]={}` we can just go straight into the `student` hash. ERB assumes that the name of your top-level hash is the first key, so the code to call the value associated with the nested "name" key would be `student["name"]`.

For the courses, we can use the ERB syntax to set up our form. We can ignore the first level of nesting, the `my_hash` portion, and just dive straight into student and course, turning `my_hash["student"]["course"]["name"]` (how we would normally build the hash in ruby) into `student[course][name]`.

```html
<form action="/student" method="post">
  Student Name: <input type="text" name="student[name]">
  Student Grade: <input type="text" name="student[grade]">
  
  Course Name: <input type="text" name="student[course][name]">
  Course Topic: <input type="text" name="student[course][topic]">
  
  <input type="submit">
</form>
```

But now, how do we handle two (or more!) courses? . To allow for multiple courses, the courses key should store an array of nested hashes:
```ruby
params = { 
  "student" => {
    "name" => "Vic",
    "grade" => "12",
    "courses" => [
      {
        "name" => "AP US History", 
        "topic" => "History"
      }, 
      {
        "name" => "AP Human Geography", 
        "topic" => "History"
      }
    ]
  }
}
```
Let's now adapt the form
```html
<form action="/student" method="post">
  Student Name: <input type="text" name="student[name]">
  Student Grade: <input type="text" name="student[grade]">
  
  Course Name: <input type="text" name="student[courses][][name]">
  Course Topic: <input type="text" name="student[courses][][topic]">
  
  Course Name: <input type="text" name="student[courses][][name]">
  Course Topic: <input type="text" name="student[courses][][topic]">
  
  <input type="submit">
</form>
```
The courses key will store an array of hashes, each containing course details.
This is where ERB syntax differs from Ruby. To access the name of the first course in Ruby, you would do something like:
```ruby
my_hash["student"]["courses"][0]["name"]
  => "AP US History"
```
ERB makes it even easier on us. Instead of manually indexing each entry, we can use an empty array ([]) in our form view, and ERB will automagically index the array for us. The [] is some ERB magic that we just need to accept and use.
```ruby
# in ruby
my_hash["student"]["courses"][0]["name"]

# in ERB
student[courses][][name]
```
We need a way to display the objects back to the user (in this case the registrar) once the student and their courses have been created. For later use in the controller, we'll call this file student.erb.
```html
<h1>Student</h1>
 
<div class="student">
  <h3>Name: <%= @student.name %></h3><br>
  <h4>Grade: <%= @student.grade %></h4>
</div><br>
 
<h1>Classes</h1>
<% @classes.each do |class| %>
  <div class="class">
    <p>Name: <%= class.name %></p><br>
    <p>Topic: <%= class.topic %></p><br>
  </div><br>
<% end %>
```
Let's write the controller actions – one to serve up the form, and one to process the data from the form.
```ruby
# app.rb

get '/' do
  erb :new
end

post '/student' do
  @student = Student.new(params[:student])
 
  params[:student][:courses].each do |details|
    Course.new(details)
  end
 
  @classes = Course.all
 
  erb :student
end
```
`params[:student]` contains the student's `name`, `grade`, and `courses` as a hash.

### Without a model
We could the following 
```ruby
class App < Sinatra::Base

    get '/' do
      erb :super_hero
    end

    post '/teams' do
      @team = params[:team]
      @members = params[:team][:members]
      erb :team
    end
end
```
The form would be
```html
<h1>Create a Team and Heroes!</h1>
<form action="/teams" method="POST">
  <p>Team Name: <input id="team_name" type="text" name="team[name]"></p>
  <p>Team Motto: <input id="team_motto" type="text" name="team[motto]"></p>
  <h2>Hero 1</h2>
  <p>Hero's Name: <input id="member1_name" type="text" name="team[members][][name]"></p>
  <p>Hero's Power: <input id="member1_power" type="text" name="team[members][][power]"></p>
  <p>Hero's Biography: <input id="member1_bio" type="text" name="team[members][][bio]"></p>
  <h2>Hero 2</h2>
  <p>Hero's Name: <input id="member2_name" type="text" name="team[members][][name]"></p>
  <p>Hero's Power: <input id="member2_power" type="text" name="team[members][][power]"></p>
  <p>Hero's Biography: <input id="member2_bio" type="text" name="team[members][][bio]"></p>
  <h2>Hero 3</h2>
  <p>Hero's Name: <input id="member3_name" type="text" name="team[members][][name]"></p>
  <p>Hero's Power: <input id="member3_power" type="text" name="team[members][][power]"></p>
  <p>Hero's Biography: <input id="member3_bio" type="text" name="team[members][][bio]"></p>
  <input type="submit" value="submit">
</form>
```
When clicking the submit "post" action to 'teams', the params sent would be
```ruby
{"team"=>{"name"=>"Team Ruby", "motto"=>"We love Ruby!", "members"=>[{"name"=>"Amanda", "power"=>"Ruby", "bio"=>"I love Ruby!"}, {"name"=>"Arel", "power"=>"JavaScript", "bio"=>"I love JavaScript!"}, {"name"=>"Katie", "power"=>"Sinatra", "bio"=>"I love Sinatra!"}]}} 
```
And the view could be (with the use of a model instances)
```html
<h1><%= @team[:name] %></h1>
<h2>Team Motto: <%= @team[:motto] %></h2>

<h2>Hero Name: <%= @members[0][:name] %></h2>
<p>Hero Power: <%= @members[0][:power] %></p>
<p>Hero Biography: <%= @members[0][:bio] %></p>

<h2>Hero Name: <%= @members[1][:name] %></h2>
<p>Hero Power: <%= @members[1][:power] %></p>
<p>Hero Biography: <%= @members[1][:bio] %></p>

<h2>Hero Name: <%= @members[2][:name] %></h2>
<p>Hero Power: <%= @members[2][:power] %></p>
<p>Hero Biography: <%= @members[2][:bio] %></p>
```

## Sessions and cookies
Let's talk about `sessions` and `cookies` and how they work together to store information about a particular user's interactions with a website at a given point in time.
- If you don't explicitly log out every time you leave Facebook's site, you'll find that, upon returning, you're still logged in and will be taken directly to your timeline. 
- If you check your Gmail account, you might see advertisements relating to search terms that you typed into Google earlier that day. 
- When you shop on Amazon, your shopping cart tracks all of the items you place into it until you're ready to checkout.

HTTP is a `stateless protocol`, treating each request as an independent transaction that is unable to use information from any previous requests. This means there is no way within the hypertext transfer protocol to remember a user’s identity from page to page...
instead, web applications requiring user login must use a session, which is a semi-permanent connection between two computers (such as a client computer running a web browser and a server running Rails).

Since HTTP is stateless, we must rely on the individual actors involved in an HTTP exchange –– the user's browser and the website's server(s) –– to persist any sort of simple state. On each HTTP request, the server sends information to the browser. Cookies were invented as a means to store that data. A cookie is a `hash` that gets **stored in the browser** and sent back to the server along with every subsequent request.

For example, when you log in to http://www.learn.co, you fill out a form with your Github username and password. Learn receives that information and, at that moment in time, knows who you are by matching up that log in information, submitted via a HTTP POST request, with data in its database. What about after you log in? After you log in and click a link for a particular lesson, you are sending another HTTP request to Learn. At this point in the process of your interaction with the Learn web application, Learn has no idea who you are! But wait, you might be thinking: "Didn't I just log in? How can Learn forget so easily?" That is what it means to be "stateless". Each web request you send is, from the point of view of the application that is receiving that request, totally independent.

There's also an important server-side use case for temporarily persisting data while a user browses the server's website. Enter sessions. Similarly to cookies, a session is an object, like a `hash`, that stores data describing a client's interactions with a website at a given point in time. The session hash **lives on the server**. Your application can access it via any of your controllers at any point in time.

A cookie will usually contain a URL to the generating website, the date on which it was created (and the date on which it is set to expire, if applicable), and other pertinent information that the web application has requested to persist (such as remembered login information, user preferences, etc).

Here's how `cookies` work:
- You visit a webpage, such as facebook.com.
- Facebook's web application receives the request and creates a cookie with some information about you as a user.
- Facebook sends that cookie back to the browser, where it is stored. Cookies will last until they expire or are deleted.
- Every subsequent request you make to facebook.com sends the cookies back to the server, where Facebook can access them, retrieve information, and alter the cookies.

2 types of cookies
- **Session cookies** allow a website to keep track of your movement from page to page for that specific session (from the time you log in to the time you log out/close the browser). They are **temporarily stored in your web browser**. Session cookies simply allow you to navigate through a site without repeatedly having to authenticate yourself on every new page you visit within the web application domain. Example: Without a session cookie, your Amazon shopping cart would remain empty despite your best efforts to fill it with items. There would be no persistent record of your activities for that session.
- **Persistent cookies** allow a website to remember your user information and preferences for future visits. A persistent cookie is **stored on your computer**. Persistent cookies allow for faster page loading, automatic login and user authentication, and access to other potential web application features. This means you can skip the process of sending a web request to a site and waiting to receive data back from the server in order to login, for example. Persistent cookies are usually created on the first visit to a page after you have created an account on the site, and they typically persist unless either the web application has a protocol in place for cookie expiration or the user clears their browser's cache.

You can think of **cookies as the client-side counterpart to sessions**. They store information locally and are visible only to the server that created them. Client browsers send cookies to remote servers along with each web request. The web application on the server reads the cookie, associates it with an existing session (if such a session exists), and decides how to respond. 

For example, if there is no cookie received, the application might show the login page. If a cookie is received that doesn't match the data stored in the server-side session hash, the app might show the login page with the username filled out –– having retrieved that data from the cookie –– but request that the user reauthenticate. If a cookie is received that does match the data in the server-side session hash, the app would respond with that user's data.

### Mechanics of sessions
A session is basically just a hash that stores data on the server and passes that data to the client as a cookie. You can access the data stored in the session in the same way you would any hash in Ruby.

In Sinatra we enable sessions within the controller (e.g., `app.rb`) by adding two lines in the `configure` block:
```ruby
configure do
  enable :sessions
  set :session_secret, "secret"
end
```
The `configure` block above is a part of built-in settings that control whether features are enabled or not. In this case, we're enabling the `sessions` feature.
- The first line of the configure block, `enable :sessions`, turns sessions on. 
- The next line, set `:session_secret, "secret"`, is an encryption key (anything that you want) that will be used to create a `session_id` (a string of letters and numbers that is unique to a given user's session and is stored in the browser cookie). 

`session_secret` is a pretty minimal security feature, but the basic idea is that setting your `:session_secret` to a word that other people don't know makes it harder for someone to create a fake `session_id` and hack into your site without signing up or signing in.

In order to keep track of a current user throughout a session, we need to set up the session hash to store the user_id in the hash during a controller action. Since we haven't covered how to set up logins and logouts, we can simply use the session hash to store the user's name
```ruby
get '/hey' do 
  @session = session
end
```
Because we enabled sessions in our app, every controller action has access to the `session` hash.

We stored the session hash in the instance variable @session so that our views will have access to the session data. In this case, `@session` now looks like this:
```ruby
@session = {
  "session_id"=>  
    "dd32f512ee239ad74aa6f10c8cad37ce28d6c6922eff252ed641b1017130fe22", 
  "csrf"=> "040e9777d4dfae03bb1e6498f2a75482", 
  "tracking"=>{ 
    "HTTP_USER_AGENT"=> "e193e9e937caa9a19ca483f046281aae77d2216b", 
    "HTTP_ACCEPT_LANGUAGE"=> "66eae971492938c2dcc2fb1ddc8d7ec3196037da"
  }
}
```
You can access information from the hash just like you would any hash (e.g, `@session["session_id"]`). You can also modify and add data to the session hash by adding a key-value pair:
```ruby
get '/hey' do 
  session["name"] = "Victoria"
  @session = session
end
```

## ActiveRecord and Sinatra
Sinatra doesn't come with database support out of the box, but it's relatively easy to configure.

First, we'll add three gems to allow us to use ActiveRecord: `activerecord` version 4.2.5, `sinatra-activerecord`, and `rake`. activerecord gives us access to the magical database mapping and association powers. rake, short for "ruby make", is a package that lets us quickly create files, folders, and automate tasks such as database creation, and sinatra-activerecord gives us access to some awesome Rake tasks. Make sure those three gems are in your Gemfile:
```ruby
gem 'sinatra'
gem 'activerecord', '4.2.5'
gem 'sinatra-activerecord'
gem 'rake'
gem 'thin'
gem 'require_all'
```
Into our development group, we'll add two other gems: `sqlite3` and `tux`. sqlite3 is our database adapter gem - it's what allows our Ruby application to communicate with a SQL database. tux will give us an interactive console that pre-loads our database and ActiveRecord relationships for us. Since we won't use either of these in production, we put them in our :development group - this way, they won't get installed on our server when we deploy our application.
```ruby
group :development do
    gem 'shotgun'
    gem 'pry'
    gem 'tux'
    gem 'sqlite3'
end
```
We now have access to all of the gems that we need, but we still need to setup a connection to our database.
```ruby
ENV['SINATRA_ENV'] ||= "development"

require 'bundler/setup'
Bundler.require(:default, ENV['SINATRA_ENV'])

configure :development do
  set :database, 'sqlite3:db/dogs.db'
end

require './app'
```
Notice that this didn't actually create those files or folders yet - that's how Rake will help us.

Let's also make sure we have `rake` set up.
```ruby
# in Rakefile at the root

require './config/environment'
require 'sinatra/activerecord/rake'
```
We can now type `rake -T`
```ruby
rake db:create              # Creates the database from DATABASE_URL or config/database.yml for...
rake db:create_migration    # Create a migration (parameters: NAME, VERSION)
rake db:drop                # Drops the database from DATABASE_URL or config/database.yml for t...
rake db:fixtures:load       # Load fixtures into the current environment's database
rake db:migrate             # Migrate the database (options: VERSION=x, VERBOSE=false, SCOPE=blog)
rake db:migrate:status      # Display status of migrations
rake db:rollback            # Rolls the schema back to the previous version (specify steps w/ S...
rake db:schema:cache:clear  # Clear a db/schema_cache.dump file
rake db:schema:cache:dump   # Create a db/schema_cache.dump file
rake db:schema:dump         # Create a db/schema.rb file that is portable against any DB suppor...
rake db:schema:load         # Load a schema.rb file into the database
rake db:seed                # Load the seed data from db/seeds.rb
rake db:setup               # Create the database, load the schema, and initialize with the see...
rake db:structure:dump      # Dump the database structure to db/structure.sql
rake db:structure:load      # Recreate the databases from the structure.sql file
rake db:version             # Retrieves the current schema version number
```

Let's now create a `dogs` table with two columns `name` and `breed`.
```
rake db:create_migration NAME=create_dogs
=># db/migrate/20150914201353_create_dogs.rb
```
We can update this migration file
```ruby
class CreateDogs < ActiveRecord::Migration
  def up
    create_table :dogs do |t|
      t.string :name
      t.string :breed
    end
  end
 
  def down
    drop_table :dogs
  end
end
```
The change method is actually a shorter way of writing up and down methods. While the rollback (`down`) method is not included, it's implicit in the change method. Rolling back the database would work in exactly the same way as using the `down` method.

We can now run our migration
```
rake db:migrate

== 20150914201353 CreateDogs: migrating =======================================
-- create_table(:dogs)
   -> 0.0019s
== 20150914201353 CreateDogs: migrated (0.0020s) ==============================
```

## CRUD actions in Sinatra
Let's perform the basic CRUD (Create, Read, Update, Delete) actions using Active Record.
For this example, we'll use the class name Model to stand in for whatever model your app is working with (Post, Student, Song, you name it).
- Create: Model.create
- Read: Model.all/Model.find(id_number)
- Update: Model.update
- Delete: Model.destroy

The "**create**" part of CRUD is implemented in Sinatra by building a route, or controller action, to render the form for creating a new instance of your model.
- The get '/model/new' route renders the view page with that form.
- The new.erb is the view page that contains that form.
- That form sends a POST request to another controller action, post '/models'. It is here that you place the code that extracts the form data from the params and uses it to create a new instance of your model class, something along the lines of Model.create(some_attribute: params[:some_attribute]).

There are two ways in which we can **read** data. We may want to "read" or deliver to our user, all of the instances of a class, or a specific instance of a class.
- The get '/models' controller action handles requests for all instances of a class. It should load up all of those instances and set them equal to an instance variable: @models = Model.all. Then, it renders the index.erb view page.
- The index.erb view page will use erb to render all of the instances stored in the @models instance variable.
- The get '/models/:id' controller action handles requests for a given instance of your model. For example, if a user types in www.yourwebsite.com/models/2, this route will catch that request and get the id number, in this case 2, from the params. It will then find the instance of the model with that id number and set it equal to an instance variable: @model = Model.find(params[:id]). Finally, it will render the show.erb view page.
- The show.erb view page will use erb to render the @model object.

To implement the **update** action, we need a controller action that renders an update form, and we need a controller action to catch the post request sent by that form.
- The get 'models/:id/edit' controller action will render the edit.erb view page.
- The edit.erb view page will contain the form for editing a given instance of a model. This form will send a PATCH request to patch '/models/:id'.
- The patch '/models/:id' controller action will find the instance of the model to update, using the id from params, update and save that instance.

We'll need to update config.ru to use the Sinatra Middleware that lets our app send patch requests.
```ruby
# in config.ru
use Rack::MethodOverride
```
An we need to add a line in our `edit.rb` form
```ruby
<form action="/models/<%= @model.id %>" method="post">
    <input id="hidden" type="hidden" name="_method" value="PATCH">
    <input type="text" ...>
</form>
```
The MethodOverride middleware will intercept every request sent and received by our application. If it finds a request with `name="_method"`, it will set the request type based on what is set in the value attribute, which in this case is patch.

The **delete** part of CRUD is a little tricky. It doesn't get its own view page but instead is implemented via a "delete button" on the show page of a given instance. This "delete button", however, isn't really a button; it's a form! The form should send a DELETE request to delete '/models/:id/delete' and should contain only a "submit" button with a value of "delete". That way, it will appear as only a button to the user. Here's an example:
```ruby
<form method="post" action="/models/<%= @model.id %>/delete">
  <input id="hidden" type="hidden" name="_method" value="DELETE">
  <input type="submit" value="delete">
</form>
```
The hidden input field is important to note here. This is how you can submit `PATCH` and `DELETE` requests via Sinatra. The form tag method attribute will be set to post, but the hidden input field sets it to DELETE. In the controller action, we use the `.destroy` method.

Remember, the purpose of this reading is to help you understand which controller actions render which views, and which views have forms that send requests to which controller actions, as we implement CRUD. Check out the diagram below for the big picture:
![CRUD Sinatra Diagram](http://readme-pics.s3.amazonaws.com/Screen%20Shot%202015-12-28%20at%2010.49.31%20AM.png)

### Example with simple blog app
Let's create the database
```
rake db:create_migration NAME=create_posts
```
And then we can write the migration file
```ruby
class CreatePosts < ActiveRecord::Migration
  def up
    create_table :posts do |t|
      t.string :name
      t.text :content
    end
  end

  def down
    drop_table :posts
  end
end
```
Then we can run `rake db:migrate`.

In our application controller
```ruby
require_relative '../../config/environment'

class ApplicationController < Sinatra::Base

  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
  end

  get '/' do
    @posts = Post.all
    erb :index
  end

  get '/posts/new' do
    erb :new
  end

  post '/posts' do
    Post.create(name: params[:name], content: params[:content])
    # could also be Post.create(params)
    redirect to '/posts'
  end

  get '/posts' do
    @posts = Post.all
    erb :index
  end

  get '/posts/:id' do
    @post = Post.find_by(id: params[:id])
    erb :show
  end

  get '/posts/:id/edit' do
    @post = Post.find_by(id: params[:id])
    erb :edit
  end

  patch '/posts/:id' do
    @post = Post.find_by(id: params[:id])
    @post.update(name: params[:name], content: params[:content])
    erb :show
  end

  delete '/posts/:id/delete' do
    @post = Post.find_by(id: params[:id])
    @post.destroy
    erb :deleted
  end

end
```
Corresponding to the model
```ruby
class Post < ActiveRecord::Base
end
```

And we also have the following views
```html
<!-- layout.erb -->
<!DOCTYPE html>
<head>
  <title>Page Title</title>
</head>
<body>
  <%= yield %>
</body>

<!-- new.erb -->
<form action="/posts" method="POST">
  <p>Name: <input type="text" name="name" value="name"></p>
  <p>Content: <input type="text" name="content" value="content"></p>
  <input id="submit" type="submit" name="submit" value="Submit">
</form>

<!-- index.erb -->
<% @posts.each do |post| %>
  <h2><%= post.name %></h2>
  <p><%= post.content %></p>
<% end %>

<!-- show.erb -->
<h2><%= @post.name %></h2>
<p><%= @post.content %></p>
<form action="/posts/<%= @post.id %>/delete" method="POST">
  <input id="hidden" type="hidden" name="_method" value="delete">
  <input type="submit" value="delete">
</form>

<!-- edit.erb 
name="content" is what is taken for the params
value="<%= @post.content %>" is what is displayed in the form
-->
<form action="/posts/<%= @post.id %>" method="POST">
  <input type="text" name="name" value="<%= @post.name %>">
  <input type="text" name="content" value="<%= @post.content %>">
  <input id="hidden" type="hidden" name="_method" value="patch">
  <input id="submit" type="submit" name="submit" value="Submit">
</form>

<!-- deleted -->
<p><%= @post.name %> was deleted</p>
```

## User authentication in Sinatra
We'll build a Sinatra application that will will allow users to sign up for, log into, and then log out of your application.

### LOG IN
The action of **logging in** is the simple action of storing a user's ID in the `session` hash. 

1. User visits the login page and fills out a form with their `email` and `password`. They hit 'submit' to POST that data to a controller route.
2. That controller route accesses the user's email and password from the params hash. That info is used to find the appropriate user from the database with a line such as `User.find_by(email: params[:email], password: params[:password])`. Then, that `user.id` is stored as the value of `session[:id]`.
3. As a result, we can introspect on the session hash in any other controller route and grab the current user by matching up a user ID with the value in `session[:id]`. That means that, for the duration of the session (i.e., the time between when someone logs in to and logs out of your app), the app will know who the current user is on every page.

For the time being, we will simply store a user's password in the database in its raw form. However, that is not safe! In an upcoming lesson, we'll learn about password encryption.

### LOG OUT
The action of **logging out** is really just the action of clearing all of the data, including the user's ID, from the session hash. Luckily for us, there is already a Ruby method for emptying a hash: `#clear`.

### SIGN UP
Before a user can sign in, they need to sign up! A new user submits their information (for example, their name, email, and password) via a form. When that form gets submitted, a POST request is sent to a route defined in the controller.

1. That route will get the new user's name, email, and password from the `params` hash
2. It will create and save a new instance of User. For example: `User.create(name: params[:name], email: params[:email], password: params[:password])`
3. It signs in the user automatically (would be annoying if you had to create a new account on a site and then sign in immediately afterwards). So in the same controller route in which we create a new user, we set the `session[:id]` equal to the new user's ID, effectively logging them in.
4. We redirect the user somewhere else, such as their personal homepage

Let's use the following file structure
```
-app
  |- controllers
      |- application_controller.rb
  |- models
      |- user.rb
  |- views
      |- home.erb
      |- registrations
          |- signup.erb
      |- sessions
          |- login.erb
      |- users
          |- home.erb
-config
-db
-spec
...
```

Let's create the db `rake db:create_migration NAME=create_users`
```ruby
class CreateUsers < ActiveRecord::Migration
  def up
    create_table :users do |t|
      t.string :name
      t.string :email
      t.string :password
    end
  end

  def down
    drop_table :users
  end
end
```

In our controller let's handle the different routes
```ruby
class ApplicationController < Sinatra::Base
  register Sinatra::ActiveRecordExtension
  set :views, Proc.new { File.join(root, "../views/") }

  configure do
    enable :sessions
    set :session_secret, "secret"
  end

  get '/' do
    erb :home
  end

  get '/registrations/signup' do
    erb :'/registrations/signup'
  end

  post '/registrations' do
    @user = User.create(params)
    session[:id] = @user.id
    redirect '/users/home'
  end

  get '/sessions/login' do
    erb :'sessions/login'
  end

  post '/sessions' do
    @user = User.find_by(email: params["email"], password: params["password"])
    session[:id] = @user.id
    redirect '/users/home'
  end

  get '/sessions/logout' do
    session.clear
    redirect '/'
  end

  get '/users/home' do
    @user = User.find_by(id: session[:id])
    erb :'/users/home'
  end

end
```

Our model is quite simple, with a validation for each necessary parameter:
```ruby
class User < ActiveRecord::Base
  validates_presence_of :name, :email, :password
end
```

And four our views, in their respective folders
```ruby
<!-- views/home.erb -->
<DOCTYPE! html>
<head>
</head>

<body>
  <h1>Welcome to Hogwarts</h1>
  <h4>Please sign up or log in to access your @hogwarts.edu email account</h4>
  <a href="/registrations/signup">Sign Up</a>
  <a href="/sessions/login">Log In</a>

</body>
</html>

<!-- views/registrations/signup.erb -->
<h1>Sign Up Below:</h1>
<form action="/registrations" method="post">
  <input type="text" name="name" value="your name">
  <input type="text" name="email" value="your email">
  <input type="password" name="password" value="choose a password">
  <input type="submit" value="Sign Up">
</form>

<!-- views/sessions/login.erb -->
<h1>Log In Below:</h1>
<form action="/sessions" method="post">
  <input type="text" name="email" value="your email">
  <input type="password" name="password" value="">
  <input type="submit" name="submit" value="Log In">
</form>

<!-- views/users/home.erb -->
"Welcome, <%=@user.name%>!"
<a href="/sessions/logout">Log Out</a>
```

Here are a few takeaways:
- Separate out your views into sub-folders according to their different concerns / controller routes. 
- **Signing up** for an app is nothing more than submitting a form, grabbing data from the params hash, and using it to create a new user.
- **Logging in** is nothing more that locating the correct user and setting the :id key in the session hash equal to their user ID.
- **Logging out** is accomplished by clearing all of the data from the session hash.

### Using Helper Methods
MVC architecture relies heavily on the principle of separation of concerns. We make sure that we have a different model for every class we build, that we only have one erb file per view, etc. This even extends to the purposes each of these files has. A model handles our Ruby logic, our controllers handle the HTTP requests and connect to our models, and our views either take in or display data to our users.

This means that we want to minimize the amount of logic our views contain. Our views should never directly pull from the database (ie. `User.all`, etc). All of that should be taken care of in the controller actions, and the data should be passed to the view via a specific controller action.

But if you think about most web applications you use, there is information on most pages that is dependent on being logged in. Instead of writing that type of logic directly into a view, we use helper methods. Helper methods are methods that are written in the controller, are accessible in the views, and provide some logical support. But a helper method is just a regular method, defined using `def` and `end` just like you've always done.

In the `app/helpers` directory, we're going to define a separate class specifically designed to control logic in our views. This `Helpers` class will have two class methods, `current_user` and `is_logged_in?`. These two methods will only ever be called in views, particularly account.erb, in order to add double protection to this view so that only the current user, when they are logged in, can see their own info (e.g. bank account balance). **Helpers are methods that make it cleaner to add logic to our views**.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>title</title>
    <link rel="stylesheet" href="stylesheets/bootstrap.css" type="text/css">
  </head>
  <body id="page-top">

  <% if Helpers.is_logged_in?(session) %>
    <h1>Welcome <%= Helpers.current_user(session).username %></h1>
    <h3>Your Balance: <%= Helpers.current_user(session).balance %>.0</h3>
    <a href="/logout">Log Out</a>
  <% end %>

  </body>
</html>

```
This uses helpers from `app/helpers/helpers.rb` 
```ruby
class Helpers
  def self.current_user(session)
    @user = User.find_by(id: session[:user_id])
  end

  def self.is_logged_in?(session)
    session[:user_id] ? true : false
    # can be re-written as !!session_info["user_id"]
  end
end
```
Note that this could also be written in our application_controller.rb as
```ruby
require "./config/environment"
require "./app/models/user"
class ApplicationController < Sinatra::Base
    ...
    helpers do
    	def logged_in?
    		!!session[:user_id]
    		# when you chain two exclamation marks together, it converts the value to a boolean
    		# remember, in Ruby, the only two things that evaluate to false are false (itself) and nil.
    	end
    
    	def current_user
    		User.find(session[:user_id])
    	end
    end
end
```
The `Helper Methods` at the bottom of the controller are part of Sinatra's configurations for helper methods. These are methods that allow us to add logic to our views. Views automatically have access to all helper methods thanks to Sinatra.


### Securing Passwords in Sinatra
We'll cover `bcryp`, an open-source gem that works to encrypt passwords. Securing users' data is one of the most important jobs of a web developer. Despite frequent warnings against it, many of your users will use the same username and password combination across many different websites. This means that, in general, people will use the same password for our applications that they do for their bank.

Because of this, we never want to store our users' passwords in plain text in our database. Instead, we'll run the passwords through a hashing algorithm. A hashing algorithm manipulates data in such a way that it cannot be un-manipulated. This is to say that if someone got a hold of the hashed version of a password, they would have no way to turn it back into the original. In addition to hashing the password, we'll also add a "salt". A salt is simply a random string of characters that gets added into the hash. That way, if two of our users use the password "fido", they will end up with different hashes in our database.

BCrypt will store a salted, hashed version of our users password in our database in a column called `password_digest` (not password).
```
rake db:create_migration NAME=create_users

# in the migration file make sure to pu password_digest and not password
class CreateUsers < ActiveRecord::Migration
  def up
    create_table :users do |t|
      t.string :username
      t.string :password_digest
    end
  end

  def down
    drop_table
  end
end
```
In case you want to rename a column after a wrong migration, you can create another migration
```
# db/migrate/xxxxxxxxxx_fix_column_name.rb
class FixColumnName < ActiveRecord::Migration
  def self.up
    rename_column :table_name, :old_column, :new_column
  end

  def self.down
    # rename back if you need or do something else or do nothing
  end
end
```

Let's update our model so that it includes `has_secure_password`. This ActiveRecord macro gives us access to a few new methods, and gives us all of those abilities in a secure way that doesn't actually store the plain text password in the database.
```ruby
class User < ActiveRecord::Base
	has_secure_password
end
```
Because our user has `has_secure_password`, we won't be able to save this to the database unless our user filled out the password field.

Now let's update the controller
```ruby
require "./config/environment"
require "./app/models/user"
class ApplicationController < Sinatra::Base

	configure do
		set :views, "app/views"
		enable :sessions
		set :session_secret, "password_security"
	end

	get "/" do
		erb :index
	end

	get "/signup" do
		erb :signup
	end

	post "/signup" do
		user = User.new(username: params[:username], password: params[:password])
		if user.save
			redirect '/login'
		else
			redirect "/failure"
		end
	end

	get "/login" do
		erb :login
	end

	post "/login" do
		user = User.find_by(username: params[:username])
		if user && user.authenticate(params[:password])
			session[:user_id] = user.id
			redirect "/success"
		else
			redirect '/failure'
		end
	end

	get "/success" do
		if logged_in?
			erb :success
		else
			redirect "/login"
		end
	end

	get "/failure" do
		erb :failure
	end

	get "/logout" do
		session.clear
		redirect "/"
	end

	helpers do
		def logged_in?
			!!session[:user_id]
		end

		def current_user
			User.find(session[:user_id])
		end
	end

end
```
Our `authenticate` method`, provided by the `bcrypt` gem, takes a string as an argument. If the string matches up against the password digest, it will return the user object, otherwise it will return false.

Side note: in the view note we use the `placeholder` attribute, to put a default indication inside of the input field.
```html
<h3>Login</h3>
<form action="/login" method="POST">
	<input type="text" name="username" placeholder="Username:">
	<input type="password" name="password" placeholder="Password:">
	<input type="submit" value="Log In">
</form>
```

## RESTful routes
The internet would be a really confusing place without a convention for how to handle URLS - to delete a Facebook post might be `www.facebook.com/delete-this-wallpost`, but Twitter might be `www.twitter.com/remove-this-tweet`. Without a specific convention to follow, it would be hard to create new content, edit content, and delete it. RESTful routes provides a design pattern that allows for easy data manipulation. It's nicer for users and nicer for developers to have everything consistent.

A RESTful route is a route that provides mapping between HTTP verbs (**get, post, put, delete, patch**) to controller CRUD actions (**create, read, update, delete**). Instead of relying solely on the URL to indicate what site to visit, a RESTful route also depends on the HTTP verb and the URL.

It's important to note that much of the CRUD actions are different actions that occur on the same resource. Let's take the example of a blog post with the ID 4. If we wanted to view the post, we would make a GET request to `/posts/4`. But what about when I want to update that post? Am I hitting a different resource? Nope! Just doing a different action to that same resource. So instead of a `GET` against `post/4` we do a `PUT`. That's why separating what you're talking to (the resource/noun) from the action you're doing (the HTTP verb) is important! That's key to REST.

Browsers behave a little strange as it relates to `PUT`, `PATCH` and `DELETE` requests, in that they don't know how to send those requests. They only handle well `GET` and `POST`. **Forms that delete and edit need to be submitted via POST requests.**

In the config.ru file, you'll need the following line to be placed above the run ApplicationController line: `use Rack::MethodOverride`.
This [middleware](https://github.com/rack/rack/blob/master/lib/rack/method_override.rb) will then run for every request sent by our application. It will interpret any requests with `name="_method"` by translating the request to whatever is set by the value attribute. In this example, the post gets translated to a patch request. The middleware handles put and delete in the same way.

Here is an example of application controller routes
```ruby
class ApplicationController < Sinatra::Base
  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
  end

  get '/' do
    @recipes = Recipe.all
    erb :index
  end

  get '/recipes/new' do
    erb :'/recipes/new'
  end

  get '/recipes' do
    @recipes = Recipe.all
    erb :index
  end

  get '/recipes/:id' do
    @recipe = Recipe.find_by_id(params[:id])
    erb :'/recipes/show'
  end

  get '/recipes/:id/edit' do
    @recipe = Recipe.find_by_id(params[:id])
    erb :'/recipes/edit'
  end

  patch '/recipes/:id' do
    @recipe = Recipe.find_by_id(params[:id])
    @recipe.name = params[:name]
    @recipe.ingredients = params[:ingredients]
    @recipe.cook_time = params[:cook_time]
    @recipe.save
    redirect to "/recipes/#{@recipe.id}"
  end

  post '/recipes' do
    @recipe = Recipe.create(params)
    redirect to "/recipes/#{@recipe.id}"
    # need to use "" to be able to practice string interpolation
  end

  delete '/recipes/:id/delete' do
    @recipe = Recipe.find_by_id(params[:id])
    @recipe.delete
    redirect '/recipes'
  end

end
```
Of course don't forget to add `use Rack::MethodOverride` to `config.ru`.
And for the views
```html
<!-- app/views/recipes/show.erb -->
<h1><%= @recipe.name %></h1>
<p><%= @recipe.ingredients %></p>
<p><%= @recipe.cook_time %></p>
<form action="/recipes/<%= @recipe.id %>/delete" method="post">
  <input id="hidden" type="hidden" name="_method" value="delete">
  <input id="delete" type="submit" value="delete">
</form>
<a href="/recipes/<%= @recipe.id %>/edit">Edit the <%= @recipe.name %> recipe</a>

<!-- app/views/recipes/new.erb -->
<h1>Let's create a recipe:</h1>
<form action="/recipes" method="post">
  <input type="text" name="name" placeholder="Name of recipe">
  <input type="text" name="ingredients" placeholder="Ingredients">
  <input type="text" name="cook_time" placeholder="Cooking time">
  <input type="submit" value="Submit">
</form>

<!-- app/views/recipes/edit.erb -->
<h1>Edit the <%= @recipe.name %> recipe</h1>
<form action="/recipes/<%= @recipe.id %>" method="post">
  <input type="text" name="name" placeholder="<%= @recipe.name %>">
  <input type="text" name="ingredients" placeholder="<%= @recipe.ingredients %>">
  <input type="text" name="cook_time" placeholder="<%= @recipe.cook_time %>">
  <input id="hidden" type="hidden" name="_method" value="patch">
  <input type="submit" value="submit">
</form>

<!-- app/views/layout.erb -->
<!DOCTYPE html>
<head>
</head>
<body>
  <%=yield%>
</body>

<!-- app/views/index.erb -->
<p>Hello World! Here's a list of recipes:</p>
<% if @recipes %>
  <% @recipes.each do |recipe| %>
    <h1><a href="/recipes/<%= recipe.id %>"><%= recipe.name %></a></h1>
    <p><%= recipe.ingredients %></p>
    <p><%= recipe.cook_time %></p>
  <% end %>
<% end %>

<a href="/recipes/new">Create a new recipe</a>
```

## Active Record associations
### Using Tux in Sinatra with ActiveRecord
Tux is an incredible Ruby gem that lets you access your database and perform all CRUD operations on it through the terminal. It also loads a full environment in the console that allows you to see all routes and views. 

Primarily, you'll use Tux to make sure your database is set up properly, play around with Ruby objects, and make sure your ActiveRecord associations are working properly.

Trying to test our User model and methods in IRB is a big big mess, it doesn't recognize `ActiveRecord`. We keep running into issues.
![tux](http://readme-pics.s3.amazonaws.com/sinatra-tux-irb-error.png)
![tux2](http://readme-pics.s3.amazonaws.com/sinatra-tux-irb-error2.png)

In Tux you can use all of our ActiveRecord methods to play with the database, for real!

### ActiveRecord Associations
Let's create a relationship in our app that mimics the real life cat-owner relationship: owners can `have many` cats and cats `belong to` an owner. Let's assume we have two tables in our database: `cats` and `owners`, which we created from the command line using rake.

First, we create a cats table from the command line:
`rake db:create_migration NAME="create_cats"`

In the migration file let's put a foreign key `owner_id`:
```ruby
class CreateCats < ActiveRecord::Migration
  def change
    create_table :cats do |t|
      t.string :name
      t.integer :age
      t.string :breed
      t.integer :owner_id
    end
  end
end
```

Our models are simple
```ruby
class Cat
  belongs_to :owner
end

class Owner
  has_many :cats
end
```
The model with the `belongs_to` association has the foreign key.

We can play around in Tux:
```ruby
sophie = Owner.create(name: "Sophie")
maru = Cat.new(name: "Maru", age: 3, breed: "Scottish Fold")

maru.owner = sophie
maru.save
```

### Join tables in ActiveRecord
How would you handle an e-commerce site? If you think about the models, there would be a `User` class and an `Item` class, which would contain all the items a user could buy.

With an online store, many many users could select to buy the same item, so you would need to store more than one `user_id`. Basically, we're dealing with a many-to-many relationship. A user can have many items, and an item can belong to many users.

This is where **Join Tables** come in to play, with the `has_many :through` association, which is used for object associations where more than one object can own many objects of the same class.

A join table is a table that only has two columns. To keep up with the online store example, this table would contain a `user_id` and `item_id`. Each row in this table would contain a user's ID and an item's ID. 
We call this join table `user_items`. The `has_many :through` is always **singular** and in the first part of the join table name, and the `belongs_to` portion of the relationship is **pluralized** in the second part of the table name.

We need to create 3 migrations
```ruby
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name
      t.timestamps null: false
    end
end
 
class CreateItems < ActiveRecord::Migration
  def change
    create_table :items do |t|
      t.string :name
      t.integer :price
      t.timestamps null: false
    end
  end
end
 
class CreateUserItems < ActiveRecord::Migration
  def change 
    create_table :user_items do |t|
      t.integer :user_id
      t.integer :item_id
      t.timestamps null: false
    end
  end
end
```

Next we need to define the appropriate relationships in our three models.
```ruby
class User < ActiveRecord::Base
  has_many :user_items
  has_many :items, through: :user_items
end
 
class Item < ActiveRecord::Base
  has_many :user_items
  has_many :users, through: :user_items
end
 
class UserItem < ActiveRecord::Base 
  belongs_to :user
  belongs_to :item
end
```

This gives us access to the users who have purchased a particular item, as well as all the items purchased by a specific user.

## Sinatra Multiple Controllers
If you think about a typical e-commerce application, you would need at least two models: one for orders and one for products. The products model would require a series of routes all starting with `/products`, and the orders model routes would start with `/orders`. 
`/products/1` would represent requesting information about the very first product, and `/orders/1` would represent requesting information about the first order.

CRUD action | request | route
--- | --- | ---
Create | GET | '/products/new'
Create | POST | '/products'
Read | GET | '/products/:id'
Read | GET | '/products'
Update | GET | '/products/:id/edit'
Update | POST | '/products/:id'
Delete | POST | '/products/:id/delete'

And the same for Orders. With GET and POST requests, you're looking at a really full and complex controller, with 7 controller actions for each model. That's 14 controller actions before you even add in users or shopping carts.

Just like we separate out our different models into different files, we need to separate these domain concepts in our code into separate controllers. Every controller in our application should follow the Single Responsibility Principle, only encapsulating logic relating to a singular entity in our application domain. We need to separate out a `ProductsController` and an `OrdersController`.

```ruby
class ProductsController < Sinatra::Base
 
  get '/products' do
    "Product Index"
  end
 
  get '/products/:id' do
    "Product #{params[:id]} Show"
  end
 
end
```

And we separate the Orders controller
```ruby
class OrdersController < Sinatra::Base
 
  get '/orders' do
    "Order Index"
  end
 
  get '/orders/:id' do
    "Order #{params[:id]} Show"
  end
 
end
```

Now that our application logic spans more than one controller class, our `config.ru` that starts our application becomes a bit more complicated. **We must mount both classes. Only one class can be specified to be run. The other class must be loaded as Middleware**. We won't get into MiddleWare now, suffice to say, you simply `use` it instead of `run`.
```ruby
require 'sinatra'
 
require_relative 'app/controllers/products_controller'
require_relative 'app/controllers/orders_controller'

use ProductsController
run OrdersController
```
Which classes you `use` or `run` matter, but we won't worry about that now, just make sure you only ever run one class and the rest are loaded via use. **DON'T FORGET TO DO IT OR YOUR CONTROLLERS WON'T WORK!**

## Sinatra complex forms associations
As the relationships we build between our models grow and become more complex, we need to build ways for our users to interact with those models in all of their complexity.

If a `song` has many `genres`, then a user should be able to create a new song and select from a list of existing genres and/or create a new genre to be associated to that song, all at the same time.

In order to achieve this, we'll have to build forms that allow for a user to create and edit not just the given object but any and all objects that are associated to it.

Let's work on a domain model where a `Pet` `belongs_to` an `Owner`, and an `Owner` has_many `Pets`.

Here's our `OwnersController` controller 
```ruby
class OwnersController < ApplicationController

  get '/owners' do
    @owners = Owner.all
    erb :'/owners/index'
  end

  get '/owners/new' do
    erb :'/owners/new'
  end

  post '/owners' do
    @owner = Owner.create(params[:owner])
    @owner.pets << Pet.create(params[:pet]) unless params[:pet][:name].empty?
    redirect "/owners/#{@owner.id}"
  end

  get '/owners/:id/edit' do
    @owner = Owner.find(params[:id])
    erb :'/owners/edit'
  end

  get '/owners/:id' do
    @owner = Owner.find(params[:id])
    erb :'/owners/show'
  end

  post '/owners/:id' do
    @owner = Owner.find(params[:id])
    @owner.update(params[:owner])
    # for the case where no new pet is created
    @owner.pets << Pet.create(params[:pet]) unless params[:pet][:name].empty?
    redirect "owners/#{@owner.id}"
  end
end
```

And we have a `views/owners/new.erb` form, that enables us to create an owner, choose from the list of existing pets, or even create a new pet.
```html
<h1>Create a new Owner</h1>
<form action="/owners" method="POST">
  <label>Name:</label>
  <input id="owner_name" type="text" name="owner[name]">
  
  <label>Choose an existing pet:</label>
  <% Pet.all.each do |pet| %>
    <input id="<%= pet.id %>" type="checkbox" name="owner[pet_ids][]" value="<%= pet.id %>"><%= pet.name %></input>
  <% end %>

  <label>And/or, create a new pet:</label>
  <input type="text" name="pet[name]">
  
  <input type="submit" value="Create Owner">
</form>
```
This results in a `params` hash like
```
{"owner"=>{"name"=>"Adele", "pet_ids"=>["1", "2"]}, "pet"=>{"name"=>"Fake Pet"}}
```

And for the edit form we have 
```html
<h1>Update Owner</h1>
<form action="/owners/<%= @owner.id %>" method="post">
  <label>Name:</label>
  <input id="owner_name" type="text" name="owner[name]" value="<%= @owner.name %>">

  <label>Choose an existing pet:</label>
  <% Pet.all.each do |pet| %>
    <input id="<%= pet.id %>" type="checkbox" name="owner[pet_ids][]" value="<%= pet.id %>" <%= 'checked' if @owner.pets.include?(pet) %> >
      <%= pet.name %>
    </input>
  <% end %>

  <label>and/or create a new pet:</label>
  <input id="pet_name" type="text" name="pet[name]" ></input>

  <input type="submit" value="Update Owner">
</form>
```
This form results in a `params` hash like:
```
{"owner"=>{"name"=>"Adele", "pet_ids"=>["3", "4"]},
 "pet"=>{"name"=>"Another New Pet"},
 "splat"=>[],
 "captures"=>["8"],
 "id"=>"8"}
```

We are familiar with using mass assignment to create new instances of a class with Active Record. Our `params` hash contains this additional key of `pet_ids` pointing to an array of pet ID numbers. Active Record is smart enough to take that key of `pet_ids`, pointing to an array of numbers (note the use of `name="owner[pet_ids][]"`), find the pets that have those IDs, and associate them to the given owner, all because we set up our associations such that an owner has many pets. Similarly, ActiveRecord knows the `owner_id` (singular this time).
```ruby
@owner = Owner.create(params["owner"])
# => #<Owner:0x007fdfcc96e430 id: 2, name: "Adele">

@owner.pets
#=> [#<Pet:0x007fb371bc22b8 id: 1, name: "Maddy", owner_id: 5>, #<Pet:0x007fb371bc1f98 id: 2, name: "Nona", owner_id: 5>]
```

And on the other side of the association for pets we have the following views
```html
# views/pets/new.erb
<h1>Create a new Pet</h1>
<form action="/pets" method="post">
  <label>Create a new pet</label>
  <input id="pet_name" type="text" name="pet[name]">
  <br>
  <label>Choose an existing owner</label>
  <% Owner.all.each do |owner| %>
    <input id="<%=owner.id%>" type="checkbox" name="pet[owner_id]" value="<%= owner.id %>"><%= owner.name %></input>
  <% end %>
  <br>
  <label>or, create a new owner</label>
  <input id="owner_name" type="text" name="owner[name]">
  <br>
  <input type="submit" value="Create Pet">
</form>



# views/pets/edit.erb
<h1>Update Pet: <%= @pet.name %></h1>
<form action="/pets/<%= @pet.id %>" method="post">
  <label>Edit Name</label>
  <input id="pet_name" type="text" name="pet[name]" value="<%=@pet.name%>">
  <br>

  <label>Edit Owner</label>
  <% Owner.all.each do |owner| %>
    <input type="checkbox" name="pet[owner_id]" value="<%= owner.id %>" <%= 'checked' if @pet.owner_id == owner.id %> >
      <%= owner.name %>
    </input>
  <% end %>
  <br>

  <label>or, add a new owner:</label>
  <input id="owner_name" type="text" name="owner[name]">
  <br>

  <input type="submit" value="Update Pet">
</form>
```

And `PetsController`
```ruby
class PetsController < ApplicationController

  get '/pets' do
    @pets = Pet.all
    erb :'/pets/index'
  end

  get '/pets/new' do
    erb :'/pets/new'
  end

  post '/pets' do
    @pet = Pet.create(params[:pet])
    @pet.owner = Owner.create(params[:owner]) unless params[:owner][:name].empty?
    @pet.save
    redirect to "pets/#{@pet.id}"
  end

  get '/pets/:id' do
    @pet = Pet.find(params[:id])
    erb :'/pets/show'
  end

  get '/pets/:id/edit' do
    @pet = Pet.find(params[:id])
    erb :'/pets/edit'
  end

  post '/pets/:id' do
    @pet = Pet.find(params[:id])
    @pet.update(params[:pet])
    @pet.owner = Owner.create(params[:owner]) unless params[:owner][:name].empty?
    @pet.save
    redirect to "pets/#{@pet.id}"
  end
end
```

## Sinatra Project Structure
The basic architecture for a Sinatra App is the following (the rest of the files will be generated based on commands and tasks).
```
/app
    /models
    /controllers
    /views
/config
    environment.rb
/db
    /migrate
    seeds.rb
/spec
    /controllers
    /models
    spec_helper.rb
config.ru
Gemfile
Rakefile
README.md
```

### 1. Gemfile
Let's start with the Gemfile
```ruby
source 'http://rubygems.org'

gem 'sinatra'
gem 'activerecord', :require => 'active_record'
gem 'sinatra-activerecord', :require => 'sinatra/activerecord'
gem 'rake'
gem 'require_all'
gem 'bcrypt'

group :development do
    gem 'shotgun'
    gem 'pry'
    gem 'tux'
    gem 'sqlite3'
end

group :test do
  gem 'rspec'
  gem 'capybara'
  gem 'rack-test'
  gem 'database_cleaner', git: 'https://github.com/bmabey/database_cleaner.git'
end
```

### 2. config/environment.rb
We need a `/config/environment.rb` file to set the default environment, require the `gems` , establish a `DB` connection, and `require_all` of the app files (thanks to the require_all gem).
```ruby
require 'bundler/setup'
Bundler.require

ENV['SINATRA_ENV'] ||= "development"

ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/#{ENV['SINATRA_ENV']}.sqlite"
)

require_all 'app'
require_all 'lib'
```
Note we can also require our gems with `Bundler.require(:default, ENV['SINATRA_ENV'])` (default + specific to the ENV). The `Bundler.require` method basically requires all of our gems from `.`gemfile.lock`.

Note it would also be possible to require the files in several other manual ways -  without the `require_all` gem.
```ruby
Dir[File.join(File.dirname(__FILE__), "../app/models", "*.rb")].each {|f| require f}
Dir[File.join(File.dirname(__FILE__), "../app/controllers", "*.rb")].sort.each {|f| require f}
```
or using the require_relative statement (that searches relatively/from the current file's folder)
```ruby
# in config/environment.rb
require_relative "../app/controllers/application_controller.rb"
```

### 3. config.ru
A `config.ru` file is necessary when building Rack-based applications and using rackup/shotgun to start the application server (the ru stands for rackup). config.ru is first responsible for **loading our application environment**, code, and libraries. Once all our code is loaded, config.ru then specifies **which controllers to load as part of our application** using run or use (all controllers must be registered with `use` and the main one called with `run`!!).
```ruby
require_relative './config/environment'

if defined?(ActiveRecord::Migrator) && ActiveRecord::Migrator.needs_migration?
  raise 'Migrations are pending run `rake db:migrate` to resolve the issue.'
end

use Rack::Static, :urls => ['/css'], :root => 'public' 

use Rack::MethodOverride

use LandmarksController
use FiguresController
run ApplicationController
```
Note `use Rack::MethodOverride` is used to enable us to process patch and delete requests.
There's a hack to auto-add the `use` for multiple controllers
```ruby
Dir[File.join(File.dirname(__FILE__), "app/controllers", "*.rb")].collect {|file| File.basename(file).split(".")[0] }.reject {|file| file == "application_controller" }.each do |file|
  string_class_name = file.split('_').collect { |w| w.capitalize }.join
  class_name = Object.const_get(string_class_name)
  use class_name
end
```

### 4. Rakefile
Thanks to this `Rakefile` we can now use the tasks listed in `rake -T` for all of our operations with the database.
```ruby
require_relative './config/environment'
require 'sinatra/activerecord/rake'

ENV["SINATRA_ENV"] ||= "development"

task :console do
  Pry.start
end
```

### 5. application_controller.rb
```ruby
class ApplicationController < Sinatra::Base
  register Sinatra::ActiveRecordExtension
  
  configure do
    enable :sessions
    set :public_folder, 'public'
    set :views, 'app/views'
    set :session_secret, "my_application_secret"
  end


  get '/' do
   ... #regular routes stuff
  end
end
```
Note the need to declare `register Sinatra::ActiveRecordExtension` [sinatra-activerecord gem](https://github.com/janko-m/sinatra-activerecord)
Note it is also possible to configure the :views using `set :views, Proc.new { File.join(root, "../views/") }`.
