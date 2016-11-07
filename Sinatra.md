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
The `action` attribute tells the form what specific route the post request should be sent to. In this case, we're posting to a route called /food.
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

## Passing Data between views and controllers
Why is passing data back to views from your controller so important? It allows us to make your pages dynamic rather than static - that is, the data can change depending on the inputs provided by the user.














