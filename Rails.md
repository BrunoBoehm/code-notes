# Rails

## MVC architecture
In order to ensure that applications have a specific organizational structure, the creators of Rails utilized the model-view-controller architecture.

In a restaurant kitchen we have three key components:

- A chef that makes the food
- A waiter that takes the order and brings out the food
- A table where the food is served to the customer

Using this as an analogy, we can assign the following roles to each of our restaurant components:

- Model - The model is the chef, it should manage the critical aspects of the application. One of my favorite tasks in a Rails application is working with the model files. This is where you can be very expressive with the custom algorithms that you want to utilize and you also have direct access to the specific database record. The logic of your application should mainly reside in the model files.
- Controller - The controller is the waiter. In the same way that the waiter takes the order from the customer to the chef and then brings the food to the table, the controller transmits data requests from the user to the model, and then delivers data that is rendered in the view to the user.
- View - The view is the table. A table shouldn't do anything besides sit there and hold the food when it is delivered. In like manner the views should not contain any programming logic, they should simply render what the controller sends it. One of the top issues I discover when I review a Rails application that has a large number of bugs is that the developer integrated too much logic directly into the view.

Rails was created with the concept of convention over configuration and this holds true for how the MVC structure was setup.

- A `post.rb` model file that will contain: validations, database relationships, callbacks, and any custom logic for posts. At the end of the day, the model file is a Ruby class. If it has a corresponding database table, it will inherit from the ActiveRecord::Base class, which means that it can access a number of methods that assist in working with the database. Some of the domain logic would include items such as complex database queries, data relationships, and custom algorithms. It is important to remember to follow the single responsibility principle for your model class files. If any of the methods that you place in the model file perform tasks outside the scope of that specific model, they should probably be moved to their own class.
- A `posts_controller.rb` file that will have methods to manage data flow for the Post behavior, including the full set of CRUD features. The standard methods are: index, new, create, show, edit, update, and destroy. Each controller inherit from `StaticController < ApplicationController`.
- A `views/` directory that will contain a corresponding view for each of the pages that the end user will access. For a CRUD based model, a few of the standard views would include: an index view to show all records, a show page that shows a specific record, and then new and edit pages that each render a form. In a Rails application, the view files should contain the least amount of logic of any of the files in the model-view-controller architecture. The role of the view is to simply render whatever it is sent from the database.

## Routing
As a framework, Rails has a comprehensive routing system for both dynamic and static pages.

- Static route - A static route will render a view that does not change. Typically, you will not send parameters to it. Examples would be a site's about or contact pages.
```ruby
#   in config/routes.rb
Rails.application.routes.draw do
    get         'about',    to: 'static#about'
#   HTTP verb   URL path    method in StaticController
end

#   in app/controllers/static_controller.rb
class StaticController < ApplicationController
    def about
        render "some_page"
        # You can either provide the full view path (static/some_page) 
        # or you can omit the enclosing directory, e.g., render "some_page"
    end
end

#   in some_page.html.erb
<h1>Hello from some page</h1>
```

- Dynamic route - Dynamic routes are pages that accept parameters and render different content based on those parameters. An example would be a blog's post page that contains a specific article.

```ruby
get 'posts/:id', to: 'posts#show'

resources :posts, only: :show
```
Here you will notice something that's different from the static route. The `/:id` tells the routing system that this route can receive a parameter and that the parameter will be passed to the controller's show action.


Below is the flow that takes place when a user attempts to go to a page on a Rails application:

1. A URL is entered into the browser; this is the HTTP request
2. That request is sent to the server where the application's router interprets the request and sends a message to the controller mapped to that route
3. The controller communicates with the view file mapped to the controller method
4. The server returns that HTTP response, which contains the view page that can be viewed in the browser

Rails gives us two options for how views are mapped between the controller and view files:
- Explicit rendering - for explicit rendering, Rails lets you dictate which view file you want to have the controller action mapped to.
- Implicit rendering - for implicit rendering, Rails follows a standard convention that automatically looks for the view file with the same name as the controller action. Rails core team has built out a number of standardized processes, such as implicit view rendering to help make development life a little easier.

Typically, you will find that you want to utilize the implicit workflow in your day to day coding practice. The rationale is quite practical. Imagine that you are taking over a legacy Rails project. Rails has always had the goal of making the development process as efficient as possible, which is why it is typically best to follow these types of implicit procedures.

Video [review](https://www.youtube.com/watch?time_continue=32&v=KKQ8lpEyw2g)

Be careful: Rails requires the file name to match the class name. Therefore you should name `app/controllers/students_controller.rb` the same as your controller name `StudentsController`.

## ActiveRecord and Rails
Active Record is the built-in ORM that Rails utilizes to manage the model aspects of an application.
By using Active Record, you are also able to perform advanced query tasks, such as method chaining and scoping, which typically require less code and make for a more readable query.

An important thing to remember is that at the end of the day the model file is a Ruby class. It will typically inherit from the `ActiveRecord::Base` class, which means that it has access to a number of methods that assist in working with the database. A typical model file will contain code such as but not limited to the following:
- Custom scopes
- Model instance methods
- Default settings for database columns
- Validations
- Model-to-model relationships
- Callbacks
- Custom algorithms

As a professional Rails developer, you will be expected to build applications by leveraging a [BDD](http://rspec.info/) (Behaviour Driven Development)  process, so we will walk through how to build each feature with a test-first approach so that the tests can lead our development. In order to get started, we will first create an RSpec test. We've provided a basic skeleton of a Rails application using RSpec in this repo.

We generate the initial rspec config by
```
# the -T flag tells the Rails project generator not to
# include TestUnit, the default testing framework:
$ rails new rails-activerecord-models-and-rails-readme -T

# The Rails project generator created this directory for us:
$ cd rails-activerecord-models-and-rails-readme

# We modified the Gemfile to include
# gem 'rspec-rails', '~> 3.0'
# in the :development, :test group, then ran:
$ bundle install

# Finally, we created the initial RSpec config:
$ rails g rspec:install
```
Let's create a `spec/models/post_spec.rb` file
```ruby
require 'rails_helper'
 
describe Post do
    it 'can be created' do
        post = Post.create!(title: "My title", description: "The post description")
        expect(post).to be_valid
    end
end
```
To make this test pass we need a `Post` model in `app/models/post.rb`
```ruby
class Post < ActiveRecord::Base

end
```
We also need to create the database table for our posts. Create a new directory and file `db/migrate/001_create_posts.rb`.
```ruby
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.text :description

      t.timestamps null: false
    end
  end
end
```

Now let's make another test
```ruby
it 'has a summary' do
  post = Post.create!(title: "My title", description: "The post description")
  expect(post.post_summary).to eq("My title - The post description")
end
```
We need to add a `post_summary` method to `Post`:
```ruby
def post_summary
    self.title + " - " + self.description
end
```

Being able to work in different environments is one of the strong points of Rails, and the database.yml file takes advantage of this feature by having different database options for each type of environment. If you take a look at the file, you can see that you can establish different database adapters, pools, timeout values, etc. for each environment specifically. This allows for you to have a setup such as using SQLite locally and Postgres in production, along with having a segmented database environment for your testing suite.

Creating a migration in rails can be done with the dedicated `rails g migration` command
```ruby
rails generale migration CreateProducts name:string part_number:string

rails generate migration AddDetailsToProducts part_number:string price:decimal
```

Inside of the migration file `change` method, you can use the following methods
```ruby
change_table :products do |t|

    add_column
    add_foreign_key
    add_index
    add_reference
    add_timestamps
    change_column_default #must supply a :from and :to option
    change_column_null
    create_join_table
    create_table
    disable_extension
    drop_join_table
    drop_table #must supply a block
    enable_extension
    remove_column #must supply a type
    remove_foreign_key #must supply a second table
    remove_index
    remove_reference
    remove_timestamps
    rename_column
    rename_index
    rename_table

end
```

## REST
If you have been building applications for a while, there is a good chance that you have already worked with RESTful APIs. Integrating a Facebook login, having something in your application post to Twitter, pulling in a feed of images from Instagram, or even calling a list of locations from Google Maps are all examples of using a RESTful API to communicate between applications.

Rails has RESTful principles built into its core, so, whether you are utilizing the built-in view rendering system or using the application purely as an API, you will have the tools necessary to follow standardized routing procedures.

Let's take a look at a practical example of how this works. If we want to build out a newsletter feature, we would need the system to have four key actions – Create, Read, Update, and Destroy – commonly known as 'CRUD' actions. In addition to the CRUD actions, we will also need an index page that lists out all of our newsletters – that's our fifth route. Since our users will need to have a visual interface for creating and updating records (a form for creating and another form for updating), we will need two more routes. Putting all of that together, you will see that we end up needing seven different routes. 

The `GET` routes are all routes that usually render some erb content to a web browser. These are the routes that our users will work with everyday. The `POST` and `PUT` are the url in the form action, and the `DELETE` is a new type of verb.

Method | Action | Description
--- | --- | ---
GET | /newsletters | Show all newsletters
POST | /newsletters | Create a new newsletter
GET | /newsletters/new | Render the form for creating a new newsletter
GET | /newsletters/:id/edit | Render the form for editing a newsletter
GET | /newsletters/:id | Show a single newsletter
PATCH | /newsletters/:id | Update a newsletter
DELETE | /newsletters/:id | Delete a newsletter

Thankfully, Rails maps these specific things to specific methods or "actions" as they are called in Rails. If we had a controller called NewsletterController, we would define these seven methods and Rails will call them automatically based on the correct route.

Method | Description
--- | ---
index | Show all newsletters
create | Create a new newsletter
new | Render the form for creating a new newsletter
edit | Render the form for editing a newsletter
show | Show a single newsletter
update | Update a newsletter
destroy | Delete a newsletter

![REST in Rails](https://curriculum-content.s3.amazonaws.com/web-development/rails-intro-to-rest/rails_routes.png)

In analyzing the diagram, you can see the flow of data as follows:
- The HTTP request contains an HTTP verb and hits a specific URL path.
- The router in the application processes the request and 'routes' the request data to the proper controller action.
- The controller action either performs a task, such as creating, updating, or deleting a record in the database, or it runs a database query and renders a view to the client.

If you've worked with Sinatra, you've seen how it's possible to declare an action's route(s) within the controller. Rails eschews this method of routing in favor of moving routes to a config file and treating them as RESTful by default. That's not to say that Sinatra applications cannot serve resources in a RESTful fashion — of course they can! — but Rails goes the additional step of making it difficult to do anything else.


## ActionView

### Rails URL Helpers
Route helpers are more dynamic since they are methods and not simply strings. This means that if something changes with the route there are many cases where the code itself won't need to be changed at all.

Route helper methods help clean up the view and controller code and assist with readability. On a side note, you cannot use these helper methods in your model files.
It's more natural to be able to pass arguments into a method as opposed to using string interpolation. For example, `post_path(post, opt_in: true)` is more readable than `"posts/<%= post.id %>?opt_in=true"`.

Route helpers translate directly into HTML-friendly paths. In other words, if you have any weird characters in your urls, the route helpers will convert them so they can be read properly by browsers. This includes items such as spaces or characters such as `&`, `%`, etc.

#### `_path` and `_url`
The two most popular method types are `_path` and `_url`. So if we want to render a relative link path to our posts' index page, the method would be `posts_path` or `posts_url`. The difference between `_path` and `_url` is that `_path` gives the relative path and `_url` renders the full URL. **In general, it's best to use the _path version so that nothing breaks if your server domain changes.** 
```ruby
posts   GET  /posts(.:format)       posts#index
post    GET  /posts/:id(.:format)   posts#show
```
Whenever you have `:id` parameters listed in the path like this, you will need to pass the route helper method an ID, so an example of what our show route code would look like is `post_path(@post)`.

One of the other nice things about utilizing route helper methods is that they create predictable names for the methods. Once you get into day-to-day Rails development, you will only need to run `rake routes` to find custom paths. Let's imagine that you take over a legacy Rails application that was built with traditional routing conventions. If you see CRUD controllers for newsletters, students, sales, offers, and coupons, you don't have to look up the routes to know that you could call the index URLs for each resource.

#### `link_to`
Let's improve the links inside of a view.
```ruby
<% @posts.each do |post| %>
  <div><a href='<%= "/posts/#{post.id}" %>'><%= post.title %></a></div>
<% end %>

# replaced with
<% @posts.each do |post| %>
  <div><%= link_to post.title, "/posts/#{post.id}" %></div>
<% end %>

# and even better
<% @posts.each do |post| %>
  <div><%= link_to post.title, post_path(post.id) %></div>
<% end %>

# and even better
<% @posts.each do |post| %>
  <div><%= link_to post.title, post_path(post) %></div>
<% end %>
```

Plenty other cool customizations are possible on the [documentation](http://api.rubyonrails.org/classes/ActionView/Helpers/UrlHelper.html).

#### Using the :as option
If for any reason you don't like the naming structure for the methods or paths, you can customize them quite easily. A common change is updating the path users go to in order to register for a site. Out of the box, the standard path would be `/users/new`. However, we want something a little more readable, like `/register`.
```ruby
get '/register', to: 'users#new', as: 'register'
```
Now the application lets users navigate to `/register` to sign up, and you, the developer, can utilize your own custom `register_path` route helper throughout the app.

### Rails **form-tag**
Rails forms give users the ability to submit data into form fields. This can be used for: creating new database records, building a contact form, integrating a search engine field, and pretty much every other aspect of the application that requires user input. We have the flexibility to utilize:
- Built-in form helper methods
- Plain HTML form elements

It would be very easy to integrate form helpers (and we could have our form working in a few minutes). However, fully understanding what Rails is doing behind the scenes is more important than getting the form working right away.

```erb
<form action="<%= posts_path %>" method="POST">
  <label>Post title:</label><br>
  <input type="text" id="post_title" name="post[title]"><br>
 
  <label>Post description:</label><br>
  <textarea id="post_description" name="post[description]"></textarea><br>
 
  <input type="submit" value="Submit Post">
</form>
 
<%= params.inspect %>
```

Just like Sinatra, Rails takes the user input entered into form fields and stores it in the `params` hash. The name attribute for a given input field is used as the key within `params` at which the entered data is stored.

For instance, the input entered into the "Post title:" field in the above form would be stored as the value of `params[:post][:title]`. Traditionally, Rails apps use that `model[attribute]` syntax for name attributes (e.g., `post[title]`).

You'll also notice that we're printing out params to the page. Until we set up the form action, clicking Submit Post won't actually redirect to a page on which the input values will be visible, but we'd still like to verify that the params hash is being populated correctly.

We need to draw a create route so that the routing system knows what to do when a POST request is sent to the /posts resource:
```ruby
# config/routes.rb
resources :posts, only: [:index, :new, :create]
```

And in our controller we want to define the create function
```ruby
def create
  Post.create(title: params[:post][:title], description: params[:post][:description])
  redirect_to posts_path
  # redirect_to post_path(@post)
end
```

Now back in the browser when clicking submit we get
![CSRF](https://s3.amazonaws.com/flatiron-bucket/readme-lessons/InvalidAuthenticityToken.png)
Which leads us to a very important part of Rails forms: CSRF. "CSRF" stands for: Cross-Site Request Forgery. One site making a request to another site via a form is the general flow of a Cross-Site Request Forgery. Rails blocks this from happening by default by requiring that a unique authenticity token be submitted with each form. This authenticity token is stored in the session and can't be hijacked by hackers: it performs a match check when the form is submitted, and it will throw an error if the token isn't there or doesn't match.

To fix this `ActionController::InvalidAuthenticityToken` error, we can integrate the `form_authenticity_token` helper into the form as a hidden field. 
```ruby
<input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
```

Let's now use form helpers to refactor the form. `ActionView`, a sub-gem of Rails, provides a number of helper methods to assist with streamlining view template code.
```ruby
# let's replace this
<form action="<%= posts_path %>" method="POST">
</form>

#by 
<%= form_tag posts_path do %>
<% end %>
```
The `form_tag` Rails helper is smart enough to know that we want to submit the form via the POST method, and it automatically renders the HTML that we were writing by hand before. The `form_tag` method also automatically generates the necessary authenticity token, so we can remove the now-redundant `hidden_field_tag`.

For this form, we'll be using a `text_field_tag` and a `text_area_tag` and passing each the corresponding name attribute as a symbol. It's important to keep in mind that form helpers aren't magic –– they're simply Ruby methods that accept arguments, such as the name attribute and any additional parameters related to the form's elements. In addition to updating the form fields, we'll also replace the HTML tag for the submit button with a `submit_tag`.
```ruby
<input type="text" id="post_title" name="post[title]"><br>

# becomes
<%= text_field_tag :'post[title]' %>
```
And
```ruby
<%= text_area_tag :'post[description]' %>

# becomes
<textarea name="post[description]" id="post_description"></textarea><br>
```
And 
```ruby
<input type="submit" value="Submit Post">

# becomes
<%= submit_tag "Submit Post" %>
```

As a recap we have now
```ruby
<%= form_tag posts_path do %>
  <label>Post title:</label><br>
  <%= text_field_tag :'post[title]' %><br>
 
  <label>Post description:</label><br>
  <%= text_area_tag :'post[description]' %><br>
 
  <%= submit_tag "Submit Post" %>
<% end %>
```
And we have used 4 tags (more [here](http://api.rubyonrails.org/classes/ActionView/Helpers/FormTagHelper.html))
- `form_tag posts_path`
- `text_field_tag :title`
- `label_tag :description, "Desc"`
- `text_area_tag :description`
- `submit_tag("Submit Post")`

This `form_tag` setup is the most basic form helper that's available in Rails.
```ruby
<%= form_tag("/cats") do %>
  <%= label_tag('cat[name]', "Name") %>
  <%= text_field_tag('cat[name]') %>
 
  <%= label_tag('cat[color]', "Color") %>
  <%= text_field_tag('cat[color]') %>
 
  <%= submit_tag "Create Cat" %>
<% end %>
```
Will build out the following HTML
```ruby
<form accept-charset="UTF-8" action="/cats" method="POST">
  <label for="cat_name">Name</label>
  <input id="cat_name" name="cat[name]" type="text">
  <label for="cat_color">Color</label>
  <input id="cat_color" name="cat[color]" type="text">
  <input name="commit" type="submit" value="Create Cat">
</form>
```

#### Edit/Update actions
As you may have noticed, there is a trend in Rails conventions where the logic for rendering a form is separate from the action that manages the database record alteration. 

For example:
- The `new` action in the controller simply renders the new form
- The `create` action is what actually handles the process of inserting the form data into the database

In like fashion, the edit and update actions have a similar convention:
- The `edit` action will handle rendering the edit form
- The `update` action will be the method that updates the database record itself

Let's draw a get route for our edit form. Since the form will need to know which record is being edited, this will need to be a dynamic route that accepts an :id as a parameter that the controller can access:
```ruby
get 'posts/:id/edit', to: 'posts#edit', as: :edit_post
# :edit_post will set the prefix of the route, so that we can now use edit_post_path instead of nothing
```
We still need to draw one additional route to handle the update action. This second route will also need to be dynamic, accepting the same `:id` as a parameter so that the action will know which record is being altered by the `PUT` HTTP request.
```ruby
put 'posts/:id', to: 'posts#update'
```
On a side note, as a shortcut you could also simply add the edit and update actions to the resources call in the routes file. That would accomplish the same goal that these two lines do.
```ruby
<h1>Edit "<%= @post.title %>"</h1>
<%= form_tag posts_path(@post), method: "put" do %>
  <label>Post Title:</label><br>
  <%= text_field_tag :title, @post.title %>
  <br>
  <label>Post Description:</label><br>
  <%= text_field_tag :description, @post.description %>
  <br>
  <%= submit_tag "Submit post" %>
<% end %>
```
Don't forget to change the target of the `form_tag` method from `posts_path` to `post_path(@post)`. The former is the equivalent of submitting a new post via the /posts action in Sinatra (and will create a new post instead of updating); the latter is the equivalent of updating an existing post via the /posts/:id action in Sinatra.

Unless we specify otherwise, a `form_tag` will default to sending an HTTP `POST` request. We need to add the `method: "put"` argument to the `form_tag` so that it knows not only which route the data should be passed through but also which HTTP verb to use.

Before we try to implement the update action, let's first make sure the data is being routed properly. Enter the following code inside of the update method:
```ruby
def update
  raise params.inspect
end
```
The `raise` method will cause the application to pause and print out the params on an error page. You could also see the params if you called `puts params.inspect`; using `puts` would simply require you to track down the data in the Rails server `log`.

#### "Get" action
We can create a get action triggered from the view, that will send to the index action of the students controller (conventional route) a `get` request.
```erb
<%= form_tag students_path, method: :get do %>
	<%= text_field_tag :query, params[:query] %>
	<%= submit_tag "Search", name: nil %>
<% end %>

<% @students.each do |student|  %>
  <%= render partial: 'student', locals: {student: student} %>
<% end %>
```
The partial is defined as
```erb
<ul>
  <li>
    Name: <%= student.name %>
  </li>
  <li>
    Birthday: <%= student.birthday.strftime("%m/%d/%Y") if student.birthday %>
  </li>
  <li>
    Hometown: <%= student.hometown.capitalize if student.hometown %>
  </li>
</ul>
```
In the controller here's the code that will handle the params
```ruby
  def index
    @students = Student.search(params[:query])
    render 'index'
  end
```
The `search` method is delegated to the model, because it deals with the database.
```ruby
  def self.search(query)
  	if query.present?
  		# self.all.select { |student| student.name.match(/#{query}/i) }
      where('NAME like ?', "%#{query}%")
  	else
  		self.all
  	end
  end
```

## Using form_for to refactor
If you know how to utilize the `form_tag` method for creating forms in Rails you may wonder why you need to learn a new form building process. Let's imagine that you've been tasked with creating the world's first pet hamster social network, and one of the requirements is that the hamster profile page needs to have about 100 different form fields that can be edited. 
If you are using the `form_tag` method, your application will be technically resubmitting all 100 fields each time you edit the data. Your form view templates will also have 100 calls to the `@hamster` instance variable and each of the hamster attributes. Thankfully `form_for` is here and will help clean up the form view template and provide some additional benefits.

To review, the `form_tag` helper method allows us to pass in: 
- the route for where the parameters for the form will be sent
- the HTTP method that the form will utilize
- the attributes for each field

Some limitations of this method are that
- Our form must be manually passed to the route where the form parameters will be submitted
- The form has no knowledge of the form's goal; it doesn't know if the form is meant to create or update a record
- You're forced to have duplicate code throughout the form; it's hard to adhere to DRY principles when utilizing the `form_tag`

The differences between `form_for` and `form_tag` are subtle, but important. Below is a basic breakdown of the differences. We'll start with talking about them at a high level perspective and then get into each one of the aspects on a practical/implementation basis:
- The `form_for` method accepts the instance of the model as an argument. Using this argument, `form_for` is able to make a bunch of assumptions for you.
- `form_for` yields an object of class FormBuilder
- `form_for` automatically knows the standard route (it follows RESTful conventions) for the form data as opposed to having to manually declare it
- `form_for` gives the option to dynamically change the submit button text (this comes in very handy when you're using a form partial and the new and edit pages will share the same form, but more on that in a later lesson)

A good rule of thumb for when to use one approach over the other is below:
- Use `form_for` when your form is directly connected to a model `object`. Extending our example from the introduction, this would be our Hamster's profile edit form that connects to the profile database table. This is the most common case when `form_for` is used, and it's the real magical form helper in Rails, it handles the retrieval of values from your object model.
- Use `form_tag` when you simply need an HTML form generated. Examples of this would be: a search form field or a contact form. `form_tag` is a lower-level form helper that simply generates a form element. `form_tag` makes no assumptions about what you're trying to do, and you're responsible for specifying exactly what the form is supposed to do (send a POST request, PATCH request, etc.)

Here's the comparison between the two, let's start with the `form_tag`
```ruby
<% # app/views/posts/edit.html.erb %>
<h3>Post Form</h3>
 
<%= form_tag post_path(@post), method: "put" do %>
  <label>Post title:</label><br>
  <%= text_field_tag :title, @post.title %><br>
 
  <label>Post description</label><br>
  <%= text_area_tag :description, @post.description %><br>
 
  <%= submit_tag "Submit Post" %>
<% end %>
```

Here's the refactored version
```ruby
<h3>Post Form</h3>
 
<%= form_for(@post) do |f| %>
  <label>Post title:</label><br>
  <%= f.text_field :title %><br>
 
  <label>Post description</label><br>
  <%= f.text_area :description %><br>
 
  <%= f.submit %>
<% end %>
```
`form_for` yields through `|f|` a FormBuilder object that lets you create form elements that correspond to attributes in the model

If you had previously created a `PUT` route like we did in the `form_tag` lesson, we'll need to change that to a `PATCH` method since that is the HTTP verb that `form_for` utilizes.
```ruby
patch 'posts/:id', to: 'posts#update'
```
You can also add `update` as an additional argument in the resources method array, and this will all happen automatically.

What's the difference between `PUT` and `PATCH`? It's pretty subtle. On a high level, `PUT` has the ability to update the entire object, whereas `PATCH` simply updates the elements that were changed. Many developers choose to utilize `PATCH` as much as possible because it requires less overhead; however, it is pretty rare when you will need to distinguish between the two verbs, and they are used interchangeably quite often.

Because `form_for` is bound directly with the Post model, we need to pass the model name into the Active Record update method in the controller. Let's change:
```ruby
@post.update(title: params[:title], description: params[:description])

# becomes
@post.update(params.require(:post))
```

So, why do we need to `require` the post model? If you look at the old form, the params would look something like this:
```
{
  "title": "My Title",
  "description": "My description"
}
```
With the new structure introduced by form_for, the params now look like this:
```
{
  "post": {
            "title": "My Title",
            "description": "My description"
          }
}
```
Notice how the title and description attributes are now nested within the post hash? That's why we needed to add the `require` method.

When you're using the `form_for` method, the object is passed as a form_for parameter, and it creates corresponding inputs with each of the attributes. For example, if you have `form_for(@cat)`, the form field params would look like `cat[name]`, `cat[color]`, etc
```ruby
<%= form_for(@cat) do |f| %>
  <%= f.label :name %>
  <%= f.text_field :name %>
  <%= f.label :color %>
  <%= f.text_field :color %>
  <%= f.submit %>
<% end %>
```
This will generate the following HTML
```html
<form accept-charset="UTF-8" action="/cats" method="post">
  <label for="cat_name">Name</label>
  <input id="cat_name" name="cat[name]" type="text" />
  <label for="cat_color">Color</label>
  <input id="cat_color" name="cat[color]" type="text" />
  <input name="commit" type="submit" value="Create" />
</form>
```

## Strong Params
To understand the goal of strong params, let's pretend that you run a pharmacy. What would happen if you let all prescription orders come through without checking for: valid prescriptions, driver licenses, etc.? (Spoiler alert: you'd probably end up in jail).
In the same way Rails application (starting in Rails 4+) wanted to shore up some security vulnerabilities and now require that you whitelist the parameters that are permitted when you're sending form data to the database. To prevent confusion, in previous lessons we manually turned off the strong parameter requirement.

We want to make sure that when users submit a form we only let the field we want get by. Let's enable Strong Params. To do this, open up `config/application.rb` and delete the line that says: `config.action_controller.permit_all_parameters = true`. Now restarting the server we'll see the error `ForbiddenAttributesError`:
![forbidden attributes](https://s3.amazonaws.com/flatiron-bucket/readme-lessons/ForbiddenAttributesError.png)

What this means is that Rails needs to be told what parameters are allowed to be submitted through the form to the database. The default is to let nothing through. Let's update our code
```ruby
def create
  @post = Post.new(params["post"])
  @post.save
  redirect_to post_path(@post)
end

# replace by
def create
  @post = Post.new(params.require(:post).permit(:title, :description))
  @post.save
  redirect_to post_path(@post)
end
```
And in our `update` method, where we only want to enable the modification of the title (not the description).
```ruby
def update
  @post = Post.find(params[:id])
  @post.update(params["post"])
  redirect_to post_path(@post)
end

# becomes
def update
  @post = Post.find(params[:id])
  @post.update(params.require(:post).permit(:title))
  redirect_to post_path(@post)
end
```

What is the deal with the `#permit` vs `#require`? The `#require` method is the most restrictive. It means that the params that gets passed in must contain a key called "post". If it's not included then it fails and the user gets an error. The `#permit` method is a bit looser. It means that the params hash may have whatever keys are in it. So in the create case, it may have the `:title` and `:description` keys. If it doesn't have one of those keys it's no problem, the hash just won't accept any other keys.

In our example we had different code for create and update, but generally you have the same items. It's a standard Rails practice to remove code repetition, so let's abstract the strong parameter call into its own method in the controller:
```ruby
# app/controllers/posts_controller.rb
 
def create
  @post = Post.new(post_params)
  @post.save
  redirect_to post_path(@post)
end
 
def update
  @post = Post.find(params[:id])
  @post.update(post_params)
  redirect_to post_path(@post)
end
 
private
    def post_params
      params.require(:post).permit(:title, :description)
    end
```
We can even improve this DRY private method by passing the permitted fields in as `*args` splat, since for the edit action the user can only modify the `:title`.
```ruby
# app/controllers/posts_controller.rb
 
def create
  @post = Post.new(post_params(:title, :description))
  @post.save
  redirect_to post_path(@post)
end
 
def update
  @post = Post.find(params[:id])
  @post.update(post_params(:title))
  redirect_to post_path(@post)
end
 
private
    def post_params(*args)
      params.require(:post).permit(*args)
    end
```


## Rails Generators
A primary goal of the Rails team was to make it efficient to build core application functionality. The Rails system has a number of generators that will do some of the manual work for us, with nice extra benefits:
- They can setup some basic specs for an application's test suite. They won't write our complex logic tests for us, but they will provide some basic examples.
- They are setup to work the same way each time. This helps standardize your code and enables your development to be more efficient since you don't have to worry as much about bugs related to spelling, syntax errors, or anything else that can occur when writing code manually.
- They follow Rails best practices, which includes utilizing RESTful naming patterns, removing duplicate code, using partials and a number of other best of breed design patterns (if you don't know what all of these are, don't worry, we will cover them shortly).

Careful though: certain generators create quite a bit of code, and if that code is not going to be used it will clutter the application code and cause confusion for future developers.

Each of the generators are entered into the terminal and will follow the syntax:
```
rails generate <name of generator>
rails g <name of generator>

rails g migration add_published_status_to_posts published_status:string
rails g migration remove_published_status_from_posts published_status:string
rails g migration add_post_status_to_posts post_status:boolean
rails g migration change_post_status_data_type_to_posts post_status:string
```

Model generators are used regularly. It does a great job of creating the core code needed to create a model and associated database table without adding a lot of bloat to the application.
At a high level, this creates:
- A database migration that will add a table and add the columns name, genre, and bio.
- A model file that will inherit from ActiveRecord::Base
- Some mocked RSpec test files
```
rails g model Author name:string genre:string bio:text
invoke  active_record
create    db/migrate/20151127225446_create_authors.rb
create    app/models/author.rb
invoke    rspec
create      spec/models/author_spec.rb
invoke      factory_girl
create        spec/factories/authors.rb
```

Controller generators are great if you are creating static views or non-CRUD related features. It will create a ton of code
Below is a list that is a little more high level:
- A controller file that will inherit from ApplicationController
- A set of routes to each of the generator arguments: dashboard, stats, financials, and settings
- A new directory for all of the view templates along with a view template file for each of the controller actions that we declared in the generator command
- A number of view based tests
- A view helper method file and spec helper file
- A Coffeescript file for specific JavaScripts for that controller
- A scss file for the styles for the controller
```
rails g controller admin dashboard stats financials settings
create  app/controllers/admin_controller.rb
 route  get 'admin/settings'
 route  get 'admin/financials'
 route  get 'admin/stats'
 route  get 'admin/dashboard'
invoke  erb
create    app/views/admin
create    app/views/admin/dashboard.html.erb
create    app/views/admin/stats.html.erb
create    app/views/admin/financials.html.erb
create    app/views/admin/settings.html.erb
invoke  rspec
create    spec/controllers/admin_controller_spec.rb
create    spec/views/admin
create    spec/views/admin/dashboard.html.erb_spec.rb
create    spec/views/admin/stats.html.erb_spec.rb
create    spec/views/admin/financials.html.erb_spec.rb
create    spec/views/admin/settings.html.erb_spec.rb
invoke  helper
create    app/helpers/admin_helper.rb
invoke    rspec
create      spec/helpers/admin_helper_spec.rb
invoke  assets
invoke    coffee
create      app/assets/javascripts/admin.coffee
invoke    scss
create      app/assets/stylesheets/admin.scss
```
This is a generator to be careful with – it can create a number of files that are never used and can cause wasted files in an application.

If you are building an API, using a front end MVC framework, or simply want to manually create your views, the resource generator is a great option for creating the code. The resource generator is a smart generator that creates some of the core functionality needed for a full featured resource without much code bloat.
```
rails g resource Account name:string payment_status:string
invoke  active_record
create    db/migrate/20151127233150_create_accounts.rb
create    app/models/account.rb
invoke    rspec
create      spec/models/account_spec.rb
invoke      factory_girl
create        spec/factories/accounts.rb
invoke  controller
create    app/controllers/accounts_controller.rb
invoke    erb
create      app/views/accounts
invoke    rspec
create      spec/controllers/accounts_controller_spec.rb
invoke    helper
create      app/helpers/accounts_helper.rb
invoke      rspec
create        spec/helpers/accounts_helper_spec.rb
invoke    assets
invoke      coffee
create        app/assets/javascripts/accounts.coffee
invoke      scss
create        app/assets/stylesheets/accounts.scss
invoke  resource_route
route    resources :accounts
```
`resources :accounts` is considered a 'magic' route that entails the full set of RESTful routes needed to perform CRUD in an application. Let's see those routes
```
rake routes | grep account
accounts      GET    /accounts(.:format)          accounts#index
              POST   /accounts(.:format)          accounts#create
new_account   GET    /accounts/new(.:format)      accounts#new
edit_account  GET    /accounts/:id/edit(.:format) accounts#edit
account       GET    /accounts/:id(.:format)      accounts#show
              PATCH  /accounts/:id(.:format)      accounts#update
              PUT    /accounts/:id(.:format)      accounts#update
              DELETE /accounts/:id(.:format)      accounts#destroy
```

## Scaffolding
It's not considered a good practice to use scaffolds in a production application. It's important to study scaffolds since they can be a great reference for how we can build CRUD functionality into our apps.
```
rails g scaffold Article title:string body:text
invoke  active_record
create    db/migrate/20151128001950_create_articles.rb
create    app/models/article.rb
invoke    rspec
create      spec/models/article_spec.rb
invoke      factory_girl
create        spec/factories/articles.rb
invoke  resource_route
 route    resources :articles
invoke  scaffold_controller
create    app/controllers/articles_controller.rb
invoke    erb
create      app/views/articles
create      app/views/articles/index.html.erb
create      app/views/articles/edit.html.erb
create      app/views/articles/show.html.erb
create      app/views/articles/new.html.erb
create      app/views/articles/_form.html.erb
invoke    rspec
create      spec/controllers/articles_controller_spec.rb
create      spec/views/articles/edit.html.erb_spec.rb
create      spec/views/articles/index.html.erb_spec.rb
create      spec/views/articles/new.html.erb_spec.rb
create      spec/views/articles/show.html.erb_spec.rb
create      spec/routing/articles_routing_spec.rb
invoke      rspec
create        spec/requests/articles_spec.rb
invoke    helper
create      app/helpers/articles_helper.rb
invoke      rspec
create        spec/helpers/articles_helper_spec.rb
invoke    jbuilder
create      app/views/articles/index.json.jbuilder
create      app/views/articles/show.json.jbuilder
invoke  assets
invoke    coffee
create      app/assets/javascripts/articles.coffee
invoke    scss
create      app/assets/stylesheets/articles.scss
invoke  scss
create    app/assets/stylesheets/scaffolds.scss
```
Scaffolds actually go beyond the other generators and create both the front- and back-end code needed for CRUD features.
What did the scaffold build for us? If we look through the files that got printed out in the console, we see:
- A migration file
- A Model file
- A controller
- View templates for each of the controller actions that render a view
- The full set of RESTful routes
- And every other component needed for a functional CRUD environment

It's interesting to see how Rails set up the controller. Below are the contents of the articles_controller.rb file:
```ruby
class ArticlesController < ApplicationController
  before_action :set_article, only: [:show, :edit, :update, :destroy]
 
  # GET /articles
  # GET /articles.json
  def index
    @articles = Article.all
  end
 
  # GET /articles/1
  # GET /articles/1.json
  def show
  end
 
  # GET /articles/new
  def new
    @article = Article.new
  end
 
  # GET /articles/1/edit
  def edit
  end
 
  # POST /articles
  # POST /articles.json
  def create
    @article = Article.new(article_params)
 
    respond_to do |format|
      if @article.save
        format.html { redirect_to @article, notice: 'Article was successfully created.' }
        format.json { render :show, status: :created, location: @article }
      else
        format.html { render :new }
        format.json { render json: @article.errors, status: :unprocessable_entity }
      end
    end
  end
 
  # PATCH/PUT /articles/1
  # PATCH/PUT /articles/1.json
  def update
    respond_to do |format|
      if @article.update(article_params)
        format.html { redirect_to @article, notice: 'Article was successfully updated.' }
        format.json { render :show, status: :ok, location: @article }
      else
        format.html { render :edit }
        format.json { render json: @article.errors, status: :unprocessable_entity }
      end
    end
  end
 
  # DELETE /articles/1
  # DELETE /articles/1.json
  def destroy
    @article.destroy
    respond_to do |format|
      format.html { redirect_to articles_url, notice: 'Article was successfully destroyed.' }
      format.json { head :no_content }
    end
  end
 
  private
    # Use callbacks to share common setup or constraints between actions.
    def set_article
      @article = Article.find(params[:id])
    end
 
    # Never trust parameters from the scary internet, only allow the white list through.
    def article_params
      params.require(:article).permit(:title, :body)
    end
end
```

The main reason why scaffolds are discouraged in production is because they create so much code and so many files that they can be hard to manage. The best applications were built by following TDD principles where I created features one element at a time, whereas scaffolds build dozens of processes instantly.

A good alternative is `resource` generators. The scaffold generator tends to be very 'opinionated' with the code that it builds, it builds out the system in a very specific manner, which is rarely the way that you would want to build your application.

With modern development practices, a large number of Rails apps are leveraging client side MVC setups such as `Backbone` or `AngularJS`. These frameworks render the view templates for a Rails application pointless, so if you rely on using scaffolds you're going to have to be removing quite a bit of code after each generate command.

Let's pretend that you're integrating the `ReactJS` framework into your Rails application. If you use a scaffold you will instantly have to go through the code and remove a large percentage of the code along with around 50% of the files themselves. Whereas if you run the resource generator, it simply creates the: migrations, model, routes, controller, and asset pipeline files. This means that you will be able to instantly start implementing the ReactJS components instead of having to be concerned with which elements need to be deleted.

All the files that you currently see in your project directory, with the exception of a few `spec` files, were generated via the `rails new` command. Consequently, it built a fully fledged web app ready out of the box. This is an example of why the Rails framework is called an opinionated framework that **favors convention over configuration**. With Sinatra, we can build an app from scratch by just adding `require 'sinatra'` in a file and calling `rackup`. Rails is much heavier than that. With an app ready out of the box through generators like rails new and rails generate scaffold, we're able to build larger scale applications very quickly.

For instance, say you were making an app to help New Yorkers find apartments. Your apartments should have an address (string), a price (float), a description (text), and an image url (string). You want to make the following things:

1. an apartment model
2. an apartments table with four columns (address, price, description, image_url)
3. an apartments controller
4. routes for apartments (show, new, index, update, edit, delete, create)
5. views (show, new, edit, index)

To make all these in one command, you would run:
```
> rails generate scaffold Apartment address:string price:float description:text image_url:string
```

And to destroy a generator's actions you can
```ruby
rails destroy scaffold Apartment
```

## Validations and forms

### AR Validations
ActiveRecord can [validate](http://guides.rubyonrails.org/active_record_validations.html) our models for us before they even touch the database. This means it's harder to end up with bad data, which can cause problems later even if our code is technically bug-free. We can use ActiveRecord::Base class methods like `#validates` to set things up.

In the context of Rails, validations are special method calls that go at the
top of model class definitions and prevent them from being saved to the
database if their data doesn't look right. In general, "validations" are any code that perform the job of protecting the database from invalid code.

Many relational databases such as SQLite have data validation features that check things like length and data type. In Rails, these validations are not used, because each database works a little differently, and handling everything in ActiveRecord itself guarantees that we'll always get the same features no matter what database we're using under the hood.

```ruby
class Person < ActiveRecord::Base
  validates :name, presence: true
end
```
`#validates` is our Swiss Army knife for validations. It takes two arguments:
- the first is the name of the attribute we want to validate
- the second is a hash of options that will include the details of how to validate it.
```ruby
class Person < ActiveRecord::Base
  validates(:name, { presence: true })
end
```
or you can use the alternate syntax
```ruby
validates_presence_of :name, :content
```

An ActiveRecord model instantiated with `#new` will not be validated, because no attempt to write to the database was made. Validations won't run unless you call a method that actually hits the DB, like `#save`. The only way to trigger validation without touching the database is to call the `#valid?` method.
Here is the [list](http://guides.rubyonrails.org/active_record_callbacks.html#running-callbacks) of all methods that trigger validation

**What can we do when a record fails validation?**
By default, ActiveRecord does not raise an exception when validation fails. DB
operation methods (such as `#save`) will simply return false and decline to
update the database.
To find out what went wrong, you can look at the model's `#errors` object. You can check all errors at once by examining `errors.messages`.
```ruby
p = Person.new
p.errors.messages #=> empty
p.valid? #=> false
p.errors.messages #=> name: can't be blank
```
You can check one attribute at a time by passing the name to errors as a key,
like so:
```ruby
person.errors[:name]
```

We can display validation errors in views, using the `ActiveModel::Errors#full_messages helper`
```ruby
<% if @article.errors.any? %>
  <div id="error_explanation">
    <h2>
      <%= pluralize(@article.errors.count, "error") %>
      prohibited this article from being saved:
    </h2>
 
    <ul>
    <% @article.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

`length` is one of the most versatile:
```ruby
class Person < ActiveRecord::Base
  validates :name, length: { minimum: 2 }
  validates :bio, length: { maximum: 500 }
  validates :password, length: { in: 6..20 }
  validates :registration_number, length: { is: 6 }
end

# which in pure ruby / no-poetry mode
class Person < ActiveRecord::Base
  validates(:name, { :length => { :minimum => 2 } })
  validates(:bio, { :length => { :maximum => 500 } })
  validates(:password, { :length => { :in => 6..20 } })
  validates(:registration_number, { :length => { :is => 6 } })
end
```

Another common built-in validator is `uniqueness`. This will prevent any account from being created with the same email as another already-existing account. You can put a scope.
```ruby
class Account < ActiveRecord::Base
  validates :email, uniqueness: true
  validates :title, uniqueness: { scope: [:artist_name, :release_year]}
end
```
or with an alternative syntax
```ruby
validates_uniqueness_of :name
```

Here's the `format` validator
```ruby
validates :name, format: { without: /[0-9]/, message: "does not allow numbers" }
```

The `numericality` validator, with a conditional application
```ruby
validates :release_year, presence: true, numericality: {less_than_or_equal_to: Date.today.year}, if: :released?
```

There is also the inclusion validator, that validates that the attributes' values are included in a given set. The inclusion helper has an option :in that receives the set of values that will be accepted. The `:in` option has an alias called `:within` that you can use for the same purpose, if you'd like to. The previous example uses the `:message` option to show how you can include the attribute's value.
```ruby
class Coffee < ApplicationRecord
  validates :size, inclusion: { in: %w(small medium large),
    message: "%{value} is not a valid size" }
end
```
The default error message for this helper is "is not included in the list". 
Note: `%w(foo bar)` is a shortcut for `["foo", "bar"]`. Meaning it's a notation to write an array of strings separated by spaces instead of commas and without quotes around them. Also, the parenthesis can be almost any other character such as square brackets `%w[...]`, curly braces `%w{...}` or even something like exclamation marks `%w!...!`.

There are ways to implement [custom validators](http://guides.rubyonrails.org/active_record_validations.html#performing-custom-validations). `#validate` is the simplest, because all you need to do is define an instance method that is invoked by `#validate`. This is probably the best way to start with most custom validations, because everything is in one place, and you can come back later to re-organize if it starts to get more complex.

The main 3 methods are:
`validate` for quick custom validations that you can extract into one of the `ActiveModel` classes and use `#validates` or `#validates_with` instead, or use the same validation logic on a different model.
```ruby
class Invoice < ApplicationRecord
  validate :expiration_date_cannot_be_in_the_past,
    :discount_cannot_be_greater_than_total_value
 
  def expiration_date_cannot_be_in_the_past
    if expiration_date.present? && expiration_date < Date.today
      errors.add(:expiration_date, "can't be in the past")
    end
  end
 
  def discount_cannot_be_greater_than_total_value
    if discount > total_value
      errors.add(:discount, "can't be greater than total value")
    end
  end
end
```
Another example
```ruby
class Post < ActiveRecord::Base
  validates :title, presence: true
  validates :content, length: { minimum: 250 }
  validates :summary, length: { maximum: 250 }
  validates :category, inclusion: { in: %w(Fiction Non-Fiction) }
  validate :is_clickbait?

  CLICKBAIT_PATTERNS = [
    /Won't Believe/i,
    /Secret/i,
    /Top [0-9]*/i,
    /Guess/i
  ]

  def is_clickbait?
    if CLICKBAIT_PATTERNS.none? { |pat| pat.match title }
      errors.add(:title, "must be clickbait")
    end
  end
end
```

Subclassing `ActiveModel::EachValidator` and `validates` for validating one specific attribute. In this case, the custom validator class must implement a `validate_each` method which takes three arguments: record, attribute, and value. These correspond to the instance, the attribute to be validated, and the value of the attribute in the passed instance.
```ruby
class EmailValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    unless value =~ /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i
      record.errors[attribute] << (options[:message] || "is not an email")
    end
  end
end
 
class Person < ApplicationRecord
  validates :email, presence: true, email: true
end
```

Subclassing `ActiveModel::Validator` and invoking `validates_with` for doing many validations in one pass. 
```ruby
class MyValidator < ActiveModel::Validator
  def validate(record)
    unless record.name.starts_with? 'X'
      record.errors[:name] << 'Need a name starting with X please!'
    end
  end
end
 
class Person
  include ActiveModel::Validations
  validates_with MyValidator
end
```

### Validations in controller actions
Now that we know Rails automatically performs validations defined on models, let's use this information to help users fix typos or other problems in their form submissions.
Up until this point, our create action has looked something like this:
```ruby
# app/controllers/posts_controller.rb

  def create
    @post = Post.create(post_params)
 
    redirect_to post_path(@post)
  end
```
However, we have two problems now:
- If the post is invalid, there will be no show path to redirect to. The post was never saved to the database, so that post_path will result in a 404!
- If we redirect, we start a new page load, which will lose all of the feedback from the validations.

When a form is submitted, a full page load occurs, as if you had navigated to a completely new URL. This means that all of the variables set by the controller's new action (like @post) disappear and are unavailable to the create action. Rails throws everything out after each request, except for the `session` hash. You're probably used to validations happening almost instantaneously on websites that you interact with on a daily basis. When you get validation feedback without a full page load, that's JavaScript at work, sneakily performing requests in the background without throwing away the current page. We won't be using that advanced technique just yet!

```ruby
# app/controllers/posts_controller.rb
 
  def create
    # Create a brand new, unsaved, not-yet-validated Post object from the form.
    @post = Post.new(post_params)
 
    # Run the validations WITHOUT attempting to save to the database, returning
    # true if the Post is valid, and false if it's not.
    if @post.valid?
      # If--and only if--the post is valid, do what we usually do.
      @post.save
      # This returns a status_code of 302, which instructs the browser to
      # perform a NEW REQUEST! (AKA: throw @post away and let the show action
      # worry about re-reading it from the database)
      redirect_to post_path(@post)
    else
      # If the post is invalid, hold on to @post, because it is now full of
      # useful error messages, and re-render the :new page, which knows how
      # to display them alongside the user's entries.
      render :new
    end
  end
```
In the above code, since we want the :new template from the same controller, we don't have to specify anything except the template name. Remember: redirects incur a new page load. When we redirect after validation failure, we lose the instance of `@post` that has feedback (messages for the user) in its errors attribute.

or in short
```ruby
# app/controllers/posts_controller.rb
  def create
    @post = Post.new(post_params)
    if @post.valid?
      @post.save
      redirect_to post_path(@post)
    else
      render :new
    end
  end
```
or 
```ruby
  def create
    @author = Author.new(author_params)

    if @author.save
      redirect_to author_path(@author)
    else
      render :new
    end
  end
```

And for the `update` action:
```ruby
  def update
    if @post.update(post_params)
      redirect_to post_path(@post)
    else
      render :edit
    end
  end
```

### Validations with `form_tag`
Now that we've learned to handle the server side of validations, we need to take care of the client side.
No one likes re-doing work. First, let's make sure we know how to pre-fill forms with the user's input so they don't have to type everything all over again.

There are two ways to pre-fill forms in Rails: `form_tag` and `form_for`. `form_for` is very heavy on Rails magic and continues to baffle scientists to this day, so we'll be going over `form_tag` first.

We're working with the `Person` model
```ruby
# app/models/person.rb
 
class Person < ActiveRecord::Base
  validates :name, format: { without: /[0-9]/, message: "does not allow numbers" }
  validates :email, uniqueness: true
end
```

Here's the form
```erb
<!-- app/views/people/new.html.erb //-->
 
<%= form_tag("/people") do %>
  Name: <%= text_field_tag "name" %><br>
  Email: <%= text_field_tag "email" %>
  <%= submit_tag "Create Person" %>
<% end %>
```
This will output the following HTML
```html
<form action="/people" accept-charset="UTF-8" method="post">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <input type="hidden" name="authenticity_token" value="TKTzvQF+atT/XHG/7h48xKVdXvILdiPj83XQhn2mWBNNhvv0Oh5YfAl2LM3DlHsQjbMOFVsYEyOwj+rPaSk3Bw==" />
  Name: <input type="text" name="name" id="name" /><br />
  Email: <input type="text" name="email" id="email" />
  <input type="submit" name="commit" value="Create Person" />
</form>
```

Remember that our `create` action now looks like this:
```ruby
# app/controllers/people_controller.rb
 
  def create
    @person = Person.new(person_params)
 
    if @person.valid?
      @person.save
      redirect_to person_path(@person)
    else
      # re-render the :new template WITHOUT throwing away the invalid @person
      render :new
    end
  end
```

With this in mind, we can use the invalid `@person` object to "re-fill" the usually-empty new form with the user's invalid entries. This way they don't have to re-type anything (You wouldn't always want to do this –– for example, with credit card numbers –– because you want to minimize the amount of times sensitive information travels back and forth over the internet.)

```erb
<!-- app/views/people/new.html.erb //-->
 
<%= form_tag "/people" do %>
  Name: <%= text_field_tag "name", @person.name %><br>
  Email: <%= text_field_tag "email", @person.email %>
  <%= submit_tag "Create Person" %>
<% end %>
```
The second argument to `text_field_tag`, as with most form tag helpers, is the "default" value (it will be `nil` at first, since we've only instantiated the object with no value, and will display the input values if `render :new` is called upon an error). Here's what it changes in the HTML form:
```html
Name: <input type="text" name="name" id="name" /><br>
Email: <input type="text" name="email" id="email" />

<!-- becomes -->
Name: <input type="text" name="name" id="name" value="Jane Developer" /><br />
Email: <input type="text" name="email" id="email" value="jane@developers.fake" />
```

Let's now display full errors with `errors.full_messages`.
When a model fails validation, its errors attribute is filled with information about what went wrong. Rails creates an `ActiveModel::Errors` object to carry this information, and we can check it's existence and then iterate:
```erb
<% if @person.errors.any? %>
  <div id="error_explanation">
    <h2>There were <%= pluralize(@person.errors.count, "error") %>:</h2>
    <ul>
      <% @person.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
    </ul>
  </div>
<% end %>
```

`ActiveModel::Errors` has much more than just a list of `full_message` error strings. It can also be used to access field-specific errors by interacting with it like a hash. If the field has errors, they will be returned in an array of strings:
```ruby
@person.errors[:name] #=> ["does not allow numbers"]
@person.errors[:email] #=> []
```
With this in mind, we can conditionally "error-ify" each field in the form,
targeting the divs containing each field:
```html
<div class="field">
  <%= label_tag "name", "Name" %>
  <%= text_field_tag "name", @person.name %>
</div>
```
And conditionally adding a class if there are errors:
```html
<div class="field<%= ' field_with_errors' if @person.errors[:name].any? %>">
  <%= label_tag "name", "Name" %>
  <%= text_field_tag "name", @person.name %>
</div>
```

Here's the full form code for our `new` action
```html
<%= form_tag("/people") do %>
  <% if @person.errors.any? %>
    <div id="error_explanation">
      <h2>There were some errors:</h2>
      <ul>
        <% @person.errors.full_messages.each do |message| %>
          <li><%= message %></li>
        <% end %>
      </ul>
    </div>
  <% end %>
 
 
  <div class="field<%= ' field_with_errors' if @person.errors[:name].any? %>">
    <%= label_tag "name", "Name" %>
    <%= text_field_tag "name", @person.name %>
  </div>
 
  <div class="field<%= ' field_with_errors' if @person.errors[:email].any? %>">
    <%= label_tag "email", "Email" %>
    <%= text_field_tag "email", @person.email %>
  </div>
 
  <%= submit_tag "Create" %>
<% end %>
```

And in terms of `edit` action we have to put in our opening form tag `post_path(@post), method: "patch"`  but the rest of the form looks the same.
```html
<h2>Editing "<%= @post.title %>"</h2>
<%= form_tag post_path(@post), method: "patch" do %>

  <% if @post.errors.any? %>
  <div id="error_explanation">
    <h2>We found <%= pluralize(@post.errors.count, "error") %></h2>
    <ul>
      <% @post.errors.full_messages.each do |msg| %>
        <li><%= msg %></li>
      <% end %>
    </ul>
  </div>
  <% end %>


  <div class="field<%= ' field_with_errors' if @post.errors[:title].any? %>">
    <%= label_tag "title", "Title" %>
    <%= text_field_tag "title", @post.title %>
  </div>

  <div class="field<%= ' field_with_errors' if @post.errors[:category].any? %>">
    <%= label_tag "category", "Category" %>
    <p>Must be either "Fiction" or "Non-Fiction".</p>
    <%= text_field_tag "category", @post.category %>
    <p>
      Please type carefully as our top scientists are working around the clock to
      enable state-of-the-art dropdown technology for this form field.
    </p>
  </div>

  <div class="field<%= ' field_with_errors' if @post.errors[:content].any? %>">
    <%= label_tag "content", "Content" %>
    <br />
    <%= text_area_tag "content", @post.content %>
  </div>
  <%= submit_tag "Update" %>
<% end %>
```

### Validations with `form_for`
The biggest difference between these two helpers is that `form_for` creates a form specifically for a model object. `form_for` is full of convenient features.
In the example below, `@post` is the model object that needs a form. `form_for` automatically performs a route lookup to find the right URL for post.
form_for takes a block. It passes an instance of `FormBuilder` as a parameter to the block, which is what `|f|` is below.
A basic implementation looks like this:
```erb
<!-- app/views/posts/edit.html.erb //-->
 
<%= form_for @post do |f| %>
  <%= f.text_field :title %>
  <%= f.text_area :content %>
  <%= f.submit %>
<% end %>
```
That creates the HTML:
```html
<form class="edit_post" id="edit_post" action="/posts/1" accept-charset="UTF-8" method="post">
  <input name="utf8" type="hidden" value="&#x2713;" />
  <input type="hidden" name="_method" value="patch" />
  <input type="hidden" name="authenticity_token" value="nRPP2OqVKB00/Cr+8EvHfYrb5sAkZRtr8f6dzBaJAI+cMceR0fUatcLWd4zdwYCpojW2J3QLK6uyBKeFAgZvmw==" />
  <input type="text" name="post[title]" id="post_title" value="Existing Post Title"/>
  <textarea name="post[content]" id="post_content">Existing Post Content</textarea>
  <input type="submit" name="commit" value="Update Post" />
</form>
```

Here would be the equivalent with `form_tag`
```erb
<!-- app/views/posts/edit.html.erb //-->
 
<%= form_tag post_path(@post), method: "patch", name: "edit_post", id: "edit_post" do %>
  <%= text_field_tag "post[title]", @post.title %>
  <%= text_area "post[content]", @post.content %>
  <%= submit_tag "Update Post" %>
<% end %>
```
Remember from a few lessons ago how CRUD methods return false when validation fails? We can use that to our advantage here and branch our actions based on the result:
```ruby
# app/controllers/posts_controller.rb
 
  def create
    @post = Post.new(post_params)
 
    if @post.save
      redirect post_path(@post)
    else
      render :new
    end
  end
```

Because of form_for, Rails will automatically prepopulate the new form with the values the user entered on the previous page.
To get some extra verbosity, we can add the snippet from the previous lesson to the top of the form:
```erb
<!-- app/views/posts/new.html.erb //-->
 
<% if @post.errors.any? %>
  <div id="error_explanation">
    <h2>
      <%= pluralize(@post.errors.count, "error") %>
      prohibited this post from being saved:
    </h2>
 
    <ul>
    <% @post.errors.full_messages.each do |msg| %>
      <li><%= msg %></li>
    <% end %>
    </ul>
  </div>
<% end %>
```

Not only will `FormBuilder` pre-fill an existing Post object's data, it will also wrap the tag in a div with an error class if the field has failed validation(s):
```html
<div class="field_with_errors">
  <input type="text" name="post[title]" id="post_title" value="Existing Post Title"/>
</div>
```
This can also result in some unexpected styling changes because `<div>` is a block tag (which takes up the entire width of its container) while `<input>` is an inline tag. If your layout suddenly gets messed up when a field has errors, this is probably why.

Our challenge as developers is to keep track of the different layers of magic that make this tool so convenient. The old adage is true: we're responsible for understanding not only how to use form_for but also why it works. Otherwise, we'll be completely lost as soon as a sufficiently unusual edge case appears.
When in doubt, read the HTML. Get used to hitting the "View Source" and "Open Inspector" hotkeys in your browser (`Ctrl-u` and `Ctrl-Shift-i` on Chrome Windows; `Option-Command-u` and `Option-Command-i` on Chrome Mac), and remember that most browsers let you examine POST data in their [developer network tools](http://superuser.com/questions/395919/where-is-the-post-tab-in-chrome-developer-tools-network).

### Delete Forms
So far, we've worked with three pieces of the CRUD puzzle:
- CREATING records, using HTTP POST requests.
- READING records, using HTTP GET requests.
- UPDATING records, using HTTP PATCH requests.
One piece remains:
- DELETING records, using HTTP DELETE requests.

Before we dive into the problem with `DELETE` (and `PATCH`) requests, let's proceed as if we were none the wiser, setting up our route and form as usual:
```ruby
# config/routes.rb
 
delete 'people/:id', to: 'people#destroy'
```
and in the view of the `show` page
```erb
# app/views/people/show.html.erb
 
<h2><%= @person.name %></h2>
<%= @person.email %>
<%= form_tag people_path(@person.id), method: "delete" %>
  <%= submit_tag "Delete #{@person.name}" %>
<% end %>
```
But, wait a minute... there's something weird about the output we get:
```html
<h2>Caligula</h2>
caligula@rome-circa-40-AD.com
<form accept-charset="UTF-8" action="/people/1" method="post">
  <input name="_method" type="hidden" value="delete" />
  <input name="utf8" type="hidden" value="&#x2713;" />
  <input name="authenticity_token" type="hidden" value="f755bb0ed134b76c432144748a6d4b7a7ddf2b71" />
  <input name="commit" type="submit" value="Delete Caligula" />
</form>
```

Browser and server developers are the yin to the web developers' yang. These are the people who own and maintain the tools themselves: Internet Explorer, Firefox, Chrome, Apache, and so on.
When a web developer makes a mistake, it might affect the users of their site. When a browser or server developer makes a mistake, it might affect over a billion people!
Because of this, browser/server developers have to go slow. Really slow. And they have to resist the urge to release "duct tape" solutions because duct tape doesn't scale to a billion users! This means that, sometimes, incomplete solutions can remain in place for years, or even decades, while the maintainers go back and forth trying to find a better approach.

As of HTML5, forms officially do not support `DELETE` and `PATCH` for their methods. What you're seeing in the above `#form_tag()` behavior is a workaround implemented for us by Rails itself.

As shown, you have to go to a user's show page to delete them. What if we want an admin control panel where users can be deleted from a list?
```erb
<!-- app/views/people/index.html.erb //-->
 
<% @people.each do |person| %>
    <div class="person">
      <span><%= person.name %></span>
      <%= link_to "Delete", person, method: :delete, data: { confirm: "Really?" } %>
    </div>
<% end %>
```
The HTML generated by that call to link_to looks like this:
```html
<a data-confirm="Really?" rel="nofollow" data-method="delete" href="/people/1">Delete</a>
```
The `data-confirm` attribute and the `data-method` attribute rely on some JavaScript built into Rails.
- `data-method` will "submit" a DELETE request as if a form had been submitted. It will use GET (the default method used by all browsers for HTML links) if the user has JavaScript disabled.
- `data-confirm` pops up a confirmation window before the link is followed, allowing the user to make sure they're ready to delete someone forever (what a decision!).

The `#destroy` action in the controller would be 
```ruby
  def destroy
    @song = Song.find(params[:id])
    @song.destroy
    redirect_to songs_path
  end
```

Bonus:
Make the delete action work with jquery
Add to your `Gemfile` and run `bundle install`
```ruby
gem 'jquery-rails'
```

Add in the `app/assets/javascripts/application.js` the following 2 lines
```
//= require jquery
//= require jquery_ujs
```

Add  in your `views/layouts` file, before the closing `</body>` tag.
```erb
<%=  javascript_include_tag "application" %>
```

And now the following link will work
```
<%= link_to 'Delete', song, method: :delete, data: {confirm: "Are you sure?"} %>
```

Put a nice nav bar on each page based on the the controller action
```erb
<% unless current_page?(controller: "songs", action: "index") %>
  <hr />
  <%= link_to "View All Songs", songs_path %>
<% end %>
```


### Rails testing
One of the most fundamental aspects of programmer productivity is the feedback loop. "Scripting" languages like Ruby and Python are great for this because you can run your code immediately after writing it. Conversely, lower-level languages like C must be compiled before being run.

Rails ships with many features to save precious seconds in developer feedback
loops, but there's no two ways about it: in anything but the most trivial app, it can be pretty complex to make sure your code is actually working correctly. In this lesson, we'll learn to shorten our feedback loop with different flavors of Rails tests, combining some standard approaches suggested in the Guides themselves with some more advanced practices that require additional dependencies (namely Capybara).

We'll be covering three types of tests:
- Models (RSpec)
- Controllers (RSpec)
- Features (RSpec/Capybara)
Features are the fanciest, so we'll leave them for last. They are preferred over regular Rails "View" tests.

By default, Rails uses Test::Unit for testing, which keeps its tests in the
mysteriously-named test/ folder. If you're planning from the start to use RSpec instead, you can tell Rails to `skip Test::Unit` by passing the `-T` flag to rails new, like so:
```
rails new cool_app -T
```
Then, you will add the gem to your Gemfile:
```
gem 'rspec-rails'
```
And use the built-in generator to add a spec folder with the right boilerplate:
```
bundle install
bundle exec rails g rspec:install
```
This is the Rails equivalent of the usual `rspec --init`.

#### Model tests
These go in `spec/models`, one file per model.
Model tests use the least amount of special features, since all you really need is the model class itself. The most common usage for model tests is to make sure you have set up your validations correctly.
Suppose we're working with this model:
```ruby
# app/models/monster.rb
 
class Monster < ActiveRecord::Base
  validates :name, presence: true
  validates :size, inclusion: { in: ["tiny", "average", "like, REALLY big"] }
  validates :taxonomy, format: { with: /\A[A-Z](\.|[a-z]+) [a-z]{2,}\z/,
    message: "must include genus and species, like 'Homo sapiens'" }
end
```

First, let's test for **validity**: we'll make sure that it understands a valid Monster:
```ruby
# spec/models/monster_spec.rb
 
describe Monster do
  let(:attributes) do
    {
      name: "Dustwing",
      size: "tiny",
      taxonomy: "Abradacus nonexistus"
    }
  end
 
  it "is considered valid" do
    expect(Monster.new(attributes)).to be_valid
  end
end
```
`let` is a standard helper method that takes a symbol and a block. It runs the block once per example in which it is called and saves the return value in a local variable named according to the symbol. This means you get a fresh copy in every test case. `let` ss more fine-grained than `before`, which means you have better control over your data. It can be used in combination with before statements to set up your test data just right before the examples are run.

RSpec provides plenty of built-in matchers, which you can peruse in their [API docs](http://rspec.info/documentation/3.4/rspec-expectations/frames.html#!RSpec/Matchers.html), but be_valid is conspicuously absent from the list. This code uses a neat trick that RSpec refers to as "[predicate matchers](https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers/predicate-matchers)", and you'll see it a lot in Rails testing.
In Ruby, it's conventional for methods that return true or false to be named with a question mark at the end. These methods are called predicate methods, because "predicate" is an English grammar term for the part of a sentence that makes a statement about the subject.
As you learned earlier in this unit, Rails provides a valid? method that returns `true` or `false` depending on whether the model object in question passed its validations.
In RSpec, when you call a nonexistent matcher (such as be_valid), it strips off the `be_` (valid), adds a question mark (`valid?`), and checks to see if the object responds to a method by that name (`monster.valid?`).

Now, let's add some tests to make sure our validations are working in the
opposite direction:
```ruby
# spec/models/monster.rb
 
  let(:missing_name) { attributes.except(:name) }
  let(:invalid_size) { attributes.merge(size: "not that big") }
  let(:missing_species) { attributes.merge(taxonomy: "Abradacus") }
 
  it "is invalid without a name" do
    expect(Monster.new(missing_name)).not_to be_valid
  end
 
  it "is invalid with an unusual size" do
    expect(Monster.new(invalid_size)).not_to be_valid
  end
 
  it "is invalid with a missing species" do
    expect(Monster.new(missing_species)).not_to be_valid
  end
```
Note that each of these `let` blocks rely on the first one, `attributes`, which contains all of our valid attributes. `missing_name` uses the Rails hash helper except to exclude the name key while the other two use the standard Ruby `merge` method to overwrite valid attributes with invalid ones.

Note `should` is an older RSpec syntax that has been deprecated in favor of `expect`.

#### Controller tests
The biggest risk in writing controller tests is redundancy: controllers exist to connect views and models, so it's difficult to test them in isolation.
First, we'll go over how to write controller tests. Then, our discussion of the why will bring us into the final subject, "feature tests".
```ruby
# spec/controllers/monsters_controller_spec.rb
 
describe MonstersController, type: :controller do
  let(:attributes) do
    {
      name: "Dustwing",
      size: "tiny",
      taxonomy: "Abradacus nonexistus"
    }
  end
 
  it "renders the show template" do
    monster = Monster.create!(attributes)
    get :show, id: monster.id
    expect(response).to render_template(:show)
  end
 
  describe "creation" do
    before { post :create, monster: attributes }
    let(:monster) { Monster.find_by(name: "Dustwing") }
 
    it "creates a new monster" do
      expect(monster).to_not be_nil
    end
 
    it "redirects to the monster's show page" do
      expect(response).to redirect_to(monster_path(monster))
    end
  end
end
```
You can use the `get` and `post` methods (along with patch and delete) to initiate test requests on the controller. A response object is available to set expectations on, such as `render_template` or `redirect_to`.
The tests above are great, especially while we're still getting used to how controllers are wired. However, almost these exact tests could be copied for any controller set up according to Rails' RESTish conventions. There's nothing inherently wrong with that, but the **redundance, along with the need to test views, inspired the creation of a new type of test supported by Capybara known as a "Feature Test"**.

#### Feature tests
If you were going to write tests for a car's steering wheel, what would you
start with?

```
When the steering wheel is rotated to the left, the tires rotate to the left.
```
The acceptance test above covers too much ground, making it brittle and difficult to maintain. This is called an acceptance test because it is phrased in terms of features that provide value to the user. (It could also be called an integration test because it tests more than one piece of the system at once.)

```
When the steering column's flange rotates, the steering shaft transmits the
rotation to the steering box.
```
The unit test above tests a single unit of functionality (the steering column - the controller). It is so specific that it almost feels like we just rewrote the controller code with different phrasing.

```
When the steering wheel is rotated to the left, the steering column transmits
the rotation to the steering box.
```
This is a feature test, and on the other hand, it is Just Right. It covers the steering wheel (view) and steering column (controller). It lets us think like a user (in terms of the steering wheel, or view) while still making intelligent assertions about how the underlying system should respond to input (in terms of the steering column, or controller).

#### Capybara
When you see key words like visit, fill_in, and page, you know you're looking at a Capybara test.
To set up Capybara, one must first add the `gem` to the Gemfile: 
```
gem 'capybara'
```
Then set up Capybara-Rails integration in `spec/rails_helper.rb`:
```
require 'capybara/rails'
```
Then set up Capybara-RSpec integration in `spec/spec_helper.rb`:
```
require 'capybara/rspec'
```
Feature tests are traditionally located in `spec/features`, but you can put them anywhere if you pass the `:type => :feature` option to your `describe` call.

To test our monster manager with Capybara, we'll start by setting up a GET request and then use Capybara's convenient helper functions to interact with the page just like a user would:
```ruby
# spec/features/monster_creation.rb
 
describe "monster creation", type: :feature do
  before do
    visit new_monster_path
    fill_in "Name", with: "Dustwing"
    select "tiny", from: "monster_size"
    fill_in "Taxonomy", with: "Abradacus nonexistus"
    click_button "Create Monster"
  end
```

When `click_button` is called, this will trigger the `POST` request to the controller's create action, just as if a user had clicked it in their browser. Now, we can write our original controller tests like usual:
```ruby
  let(:monster) { Monster.find_by(name: "Dustwing") }
 
  it "creates a monster" do
    expect(monster).to_not be_nil
  end
 
  it "redirects to the new monster's page" do
    expect(current_path).to eq(monster_path(monster))
  end
```
And because we're in Capybara land, we also have a very convenient way of making assertions about the final `GET` request:
```ruby
  it "displays the monster's name" do
    within "h1" do
      expect(page).to have_content(monster.name)
    end
  end
```
`within` sets the context for our next expectation, restricting it to the first `<h1>` tag encountered on the page. This way, our expect call will only pass if the specified content ("Dustwing") appears inside that first heading.

One interesting thing about this approach is that we're being much less explicit about certain expectations. For example, we're testing the redirect not with the initial 302 response but instead by examining the current path in Capybara's virtual "browser session". This is much more powerful and intuitive, and it doesn't sacrifice much in the way of expressivity.

The hardest part about testing usually ends up being the "why" and not the "how". Why write the test this way and not that way? For standard
CRUD functionality, Capybara is designed to save you a lot of time and mental
effort.

These can serve as fairly reliable guidelines: 
- Models should always be thoroughly unit tested.
- Controllers should be as thin as possible to keep your feature tests simple.
- If you can't avoid making a controller complex, it deserves its own isolated test.
- Capybara's syntax is much more powerful than Rails's built-in functionality for view tests, so stick with it whenever possible.
- Don't get carried away with the details when testing views: you just need to make sure the information is in the right place. If your tests are too strict, it will be impossible to make even simple tweaks to your templates without breaking the `build`.

## Active Record Associations
It all starts in the database. Foreign keys are columns that refer to the primary key of another table. Conventionally, foreign keys in Active Record are comprised of the name of the `model` you're referencing, and `_id`. So for example if the foreign key was for a `posts` table it would be `post_id`.

Like any other column, foreign keys are accessible through instance methods of the same name. For example, a migration that looks like this:
```ruby
class AddAuthorIdToPosts < ActiveRecord::Migration
  def change
    change_table :posts do |t|
      t.integer :author_id
    end
  end
end
```

Would mean you could find a post's author with the following Active Record query:
```ruby
Author.find(@post.author_id)

# or
Post.where("author_id = ?", @author.id)
```

### `belongs_to` and `has_many`
#### On the parent side
The most common relationship is many-to-one, and it is declared in Active Record with `belongs_to` and `has_many`.
Each Post is associated with **one** Author.
```ruby
class Post < ActiveRecord::Base
  belongs_to :author
end

# we can now lookup
@post.author_id = 5
@post.author #=> #<Author id=5>
```

In the opposite direction, each `Author` might be associated with **zero, one, or many** `Post` objects.
```ruby
class Author < ActiveRecord::Base
  has_many :posts
end

# we can now lookup
@author.posts #=> [#<Post id=3>, #<Post id=8>]
```

Remember, Active Record uses its Inflector to switch between the singular and plural forms of your models.

Name | Data
--- | ---
Model | Author
Table | authors
Foreign Key | author_id
belongs_to | :author
has_many | :authors

Like many other Active Record class methods, the symbol you pass determines the name of the instance method that will be defined
- `belongs_to :author` will give you `@post.author`
- `has_many :posts` will give you `@author.posts`

To `build` a new item in a collection.
If you want to add a new post for an author, you might start this way:
```ruby
new_post = Post.new(author_id: @author.id, title: "Web Development for Cats")
new_post.save
```
But the association macros save the day again, allowing this instead, starting from the `author`:
```ruby
new_post = @author.posts.build(title: "Web Development for Cats")
new_post.save
```
This will return a new Post object with the `author_id` already set for you! We use this one as much as possible because it's just easier. build works just like new. So the instance that is returned isn't quite saved to the database just yet. You'll need to `#save` the instance when you want it to be persisted to the database.

#### On the child side
The other way round, starting from the `post`
```ruby
@post.author = Author.new(name: "Leeroy Jenkins") 

# becomes
new_author = @post.build_author(name: "Leeroy Jenkins")
new_author.save
```
Remember, if you used the `build_` option, you'll need to persist your new author with `#save`.

#### On the parent's collection side
If you add an existing object to a collection association, Active Record will conveniently take care of setting the foreign key for you:
```ruby
@author = Author.find_by(name: "Leeroy Jenkins")
@author.posts
#=> []

@post = Post.new(title: "Web Development for Cats")
@post.author
#=> nil

@author.posts << @post
@post.author
#=> #<Author @name="Leeroy Jenkins">
```

### One to one `has_one` relationships
Profiles can get pretty complex, so in large applications it can be a good idea to give them their own model. In this case:
- Every author would have one, and only one, profile.
- Every profile would have one, and only one, author.

`belongs_to` makes another appearance in this relationship, but instead of `has_many` the other model is declared with `has_one`.
If you're not sure which model should be declared with which macro, it's usually a safe bet to put belongs_to on whichever model has the foreign key column in its database table.

### Many to Many relationship with join table
Each post `has_many` tags, each tag `has_many` posts.
The universe is in balance. We're programmers, so this really disturbs us. Let's shake things up and think about tags.
- One-to-One doesn't work because a post can have multiple tags.
- Many-to-One doesn't work because a tag can appear on multiple posts.

Active Record has a migration method for doing exactly this.
```
create_join_table :posts, :tags
```
This will create a table called posts_tags.

Ideally, we'd like to be able to call a `@my_post.tags` method to get all the join entries, right? That's where `has_many :through` comes in.
```ruby
class Post
  has_many :posts_tags
end
 
class PostsTag
  belongs_to :post
  belongs_to :tag
end
 
class Tag
  has_many :posts_tags
end

# becomes
class Post
  has_many :posts_tags
  has_many :tags, through: :posts_tags
end
 
class PostsTag
  belongs_to :post
  belongs_to :tag
end
 
class Tag
  has_many :posts_tags
  has_many :posts, through: :posts_tags
end
```

The `has_many :through` association is also useful for setting up "shortcuts" through nested `has_many` associations. For example, if a document has many sections, and a section has many paragraphs, you may sometimes want to get a simple collection of all paragraphs in the document. You could set that up this way:
```ruby
class Document < ApplicationRecord
  has_many :sections
  has_many :paragraphs, through: :sections
end
 
class Section < ApplicationRecord
  belongs_to :document
  has_many :paragraphs
end
 
class Paragraph < ApplicationRecord
  belongs_to :section
end
```
With through: :sections specified, Rails will now understand:
```ruby
@document.paragraphs
```

For every relationship, there is a foreign key somewhere. Foreign keys correspond to the `belongs_to` macro on the model.
One-to-one and many-to-one relationships only require a single foreign key, which is stored in the 'subordinate' or 'owned' model. The other model declares its relationship via a `has_one` or `has_many` statement, respectively.
Many-to-many relationships require a join table containing a foreign key for both models. The models are joined using `has_many :through` statements.

Here's the list of all [class methods](http://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html) given.

### Example with Flatiron BNB
Let's set up a [domain model](https://www.draw.io/?chrome=0&nav=1#G0BwkVQI_Ozq8mTk9KUlBIUGUzTWc) with the following models
```ruby
class User < ActiveRecord::Base
	has_many :listings, foreign_key: "host_id"  							# User as host
	has_many :reservations, through: :listings								# User as host
	has_many :trips, foreign_key: "guest_id", class_name: "Reservation"		# User as guest
	has_many :reviews, foreign_key: "guest_id"								# User as guest
end

class Review < ActiveRecord::Base
	belongs_to :reservation
	belongs_to :guest, class_name: "User"
end

class Reservation < ActiveRecord::Base
	belongs_to :listing
	has_many :reviews
	belongs_to :guest, class_name: "User"
end

class Neighborhood < ActiveRecord::Base
	belongs_to :city
	has_many :listings
end

class Listing < ActiveRecord::Base
	belongs_to :neighborhood
	belongs_to :host, class_name: "User"
	has_many :reservations
	has_many :reviews, through: :reservations
	has_many :guests, class_name: "User", through: :reviews
end

class City < ActiveRecord::Base
	has_many :neighborhoods
	has_many :listings, through: :neighborhoods
end
```

## AR Lifecycle events
Everything we cover here are called "Active Record Lifecycle Callbacks". Many people just call them callbacks. It's a bit shorter.
Here is a rule of thumb: Whenever you are modifying an attribute of the model, use `before_validation`. If you are doing some other action, then use `before_save`.
```ruby
class Post < ActiveRecord::Base
 
  belongs_to :author
  validate :is_title_case 
 
# New Code!!
  before_validation :make_title_case 
 
  private
 
  def is_title_case
    if title.split.any?{|w|w[0].upcase != w[0]}
      errors.add(:title, "Title must be in title case")
    end
  end
 
  def make_title_case
    self.title = self.title.titlecase
  end
end
```
If we change this to a `before_save` callback, we wouldn't be able to `create(title: "testing")`. Indeed it turns out that the `before_save` is called after validation occurs. So Rails goes is `.valid?` "Nope! Stop!", and never makes it to `before_save`.

Now let's do something that belongs in the `before_save`. We use `before_save` for actions that need to occur that aren't modifying the model itself. For example, whenever you save to the database, let's send an email to the Author alerting them that the post was just saved!

```ruby
class Post < ActiveRecord::Base
 
  belongs_to :author
  validate :is_title_case 
 
  before_validation :make_title_case 
 
# New Code!!
  before_save :email_author_about_post
 
  private
 
  def is_title_case
    if title.split.any?{|w|w[0].upcase != w[0]}
      errors.add(:title, "Title must be in title case")
    end
  end
 
  def make_title_case
    self.title = self.title.titlecase
  end
end
```

Let's cover one last callback that is super useful. This one is called `before_create`. `before_create` is very close to `before_save` with one major difference: it only gets called when a model is created for the first time. This means not every time the object is persisted, just when it is new.

### Cool AR queries
#### using `.merge`
To combine two queries (that both return `ActiveRecord::Relation` objects), you can use `#merge` to combine with `AND` (intersection):
```ruby
User.where(:first_name => 'Tobias').merge(User.where(:last_name  => 'Fünke'))

# could also be written
first_name_relation = User.where(:first_name => 'Tobias') # ActiveRecord::Relation
last_name_relation  = User.where(:last_name  => 'Fünke') # ActiveRecord::Relation
first_name_relation.merge(last_name_relation)
```

You can also combine/add with `&` or `+` but this will return an array and not and `ActiveRecord::Relation` anymore (like running `.to_a`).

Here are a few examples of `merge` used in queries.
```ruby
Post.where(published: true).merge( Post.order('created_at DESC').first(5) )
```
Returns the intersection of all published posts with the 5 most recently created posts.

```ruby
Post.where(published: true).joins(:comments).merge( Comment.where(spam: false) )
```
Performs a single join query with both where conditions.

#### using `.joins`
If you want to select all categories that have articles, you can call a single association `join`. Note that inside of the `()` you need to put the `other` (singular is `belongs_to`, plural if `has_many`): below we write `:articles` because the `Category has_many :articles`.
```ruby
Category.joins(:articles)

# will produce the following SQL query
SELECT categories.* FROM categories
    INNER JOIN articles ON articles.category_id = categories.id
```
Or, in English: "return a Category object for all categories with articles". 
Note that you will see duplicate categories if more than one article has the same category. If you want unique categories, you can use `Category.joins(:articles).distinct`.

To get multiple associations, like getting all articles with at least one category and comment.
```ruby
Article.joins(:category, :comments)

# will issue the following SQL queries
SELECT articles.* FROM articles
  INNER JOIN categories ON articles.category_id = categories.id
  INNER JOIN comments ON comments.article_id = articles.id
```
Note again that articles with multiple comments will show up multiple times.

Conditions can be given such as
```ruby
time_range = (Time.now.midnight - 1.day)..Time.now.midnight

Client.joins(:orders).where(orders: { created_at: time_range })
```
This will find all clients who have orders that were created yesterday, again using a BETWEEN SQL expression.

To combine using `OR` (only availabel in ActiveRecord 5+)
```ruby
first_name_relation.or(last_name_relation)
```

### Displaying Association Data
With the following model structure `Post belongs_to :category` and `Category has_many :posts` we can create some seed data `rake db:seed`
```ruby
# db/seeds.rb
 
clickbait = Category.create!(name: "Motivation")
clickbait.posts.create!(title: "10 Ways You Are Already Awesome")
clickbait.posts.create!(title: "This Yoga Stretch Cures Procrastination, Maybe")
clickbait.posts.create!(title: "The Power of Positive Thinking and 100 Gallons of Coffee")
 
movies = Category.create!(name: "Movies")
movies.posts.create!(title: "Top 20 Summer Blockbusters Featuring a Cute Dog")
```

And now in our views access the associated (owner and target / parent and child)
```erb
<!-- app/views/posts/show.html.erb -->
 
<h1><%= @post.title %></h1>
 
<h3>Category: <%= link_to @post.category.name, category_path(@post.category) if @post.category %></h3>
 
<p><%= @post.description %></p>
```

### Law of Demeter "One Dot"
The association-related complexity is hidden away within the associated model. This protects user-related code from future changes to friend functionality.
```ruby
user.best_friend

# is better than
user.friends.find_by(best: true)
```
For example, if the above architecture changed such that best friendship was determined by the highest "friendship" value instead of a boolean best flag, the "two-dots" code would need to be changed everywhere, but the first snippet, which obeys the Law of Demeter, hides that complexity in the `User#best_friend` method, whose definition can be changed without having to track down and update every single usage.

## Forms and basic associations
Let's say we have a simple blogging system. Our models are Post and Category. A Post `belongs_to` a Category.

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
  belongs_to :category
end

# app/models/category.rb
class Category < ActiveRecord::Base
  has_many :posts
end
```

Now we need to build the functionality for a user to create a Post. We're going to need a form for the Post's content, and some way to represent what Category the Post belongs to.

### Using category_id
As a first pass, we might build a form like this:

```erb
<%= form_for @post do |f| %>
  <%= f.label :category_id, :category %>
  <%= f.text_field :category_id %>
  <%= f.text_field :content %>
<% end %>
```

This will work if we wire up our `PostsController` with the right parameters:

```ruby
class PostsController < ApplicationController
  def create
    Post.create(post_params)
  end

  private

  def post_params
    params.require(:post).permit(:category_id, :content)
  end
end
```

But as a user experience, this is miserable. I have to know the id of the category I want to use. As a user, it is very unlikely that I know this or want to.

### Using a category_name (first attempt)
We could rewrite our controller to accept a `category_name` instead of an id:
```ruby
class PostsController < ApplicationController
  def create
    category = Category.find_or_create_by(name: params[:post][:category_name])
    Post.create(content: params[:post][:content], category: category)
  end
end
```
But we'll have to do this anywhere we want to set the category for a Post. When we're setting a Post's categories, the one thing we know we have is a Post object. What if we could move this logic to the model?

### Defining a custom setter (second attempt)
What if we gave the Post model a `category_name` attribute? Since our ActiveRecord models are still just Ruby classes, we can define our own setter methods:

```ruby
# app/models/post.rb
class Post < ActiveRecord::Base
   def category_name=(name)
     self.category = Category.find_or_create_by(name: name)
   end
end
```

The setter method `#category_name=` is called whenever a `Post` is initialized with a `category_name` field. It will associate the `category` object instance to the `post` object.

We can expand `Post.create(post_params)` to the code below, so that you can see that `#category_name=` will indeed be called.
```ruby
Post.create({
 {
    category_name: params[:post][:category_name],
    content: params[:post][:content]
  }
})
```
Since we have defined this setter ourselves, `Post.create` does not try to fall back to setting `category_name` through ActiveRecord. 
You can think of `#category_name=` as intercepting the call to the database and instead shadowing the attribute `category_name` by:
- one, making sure the `Category` exists
- providing it in-memory for the `Post` model. 
We sometimes call these in-memory attributes "virtuals".

Now we can set `category_name` on a post. We can do it when creating a post too, so our controller becomes quite simple again:
```ruby
class PostsController < ApplicationController
  def create
    Post.create(post_params)
  end

  private

  def post_params
    params.require(:post).permit(:category_name, :content)
  end
end
```

Notice the difference—we're now accepting a category name, rather than a category id. Even though you don't have an ActiveRecord field for `category_name`, because there is a key in the `post_params` hash for `category_name` it still calls the `category_name=` method.

We can change the view as well now:
```erb
<%= form_for @post do |f| %>
  <%= f.label :category_name %>
  <%= f.text_field :category_name %>
  <%= f.text_field :content %>
<% end %>
```
Now the user can enter a category by name (instead of needing to look up its id), and we handle finding or creating the `Category` in the black box of the server. This results in a much friendlier experience for the user.

### Selecting from existing categories
If we want to let the user pick from existing categories, we can use a [Collection Select](http://apidock.com/rails/ActionView/Helpers/FormOptionsHelper/collection_select) helper to render a `<select>` tag:
```erb
<%= form_for @post do |f| %>
  <%= f.collection_select :category_id, Category.all, :id, :name %>
  <%= f.text_field :content %>
<% end %>
```

The format is the following
```ruby
collection_select(object, method, collection, value_method, text_method, options = {}, html_options = {})
```
Returns `<select>` and `<option>` tags for the collection of existing return values of method for object's class. The value returned from calling `method` on the instance `object` will be selected. The `:value_method` and `:text_method` parameters are methods to be called on each member of collection. 

The return values are used as the value attribute and contents of each `<option>` tag, respectively. This will create a **drop down selection input** where the user can pick a category.

However, we've lost the ability for users to create their own categories. That might be what you want. For example, the content management system for a magazine would probably want to enforce that the category of an article is one of the sections actually printed in the magazine.

In our case, however, we want to give users the flexibility to create a new category *or* pick an existing one. What we want is autocompletion, which we can get with a [datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist). `datalist` is a new element in the HTML5 spec that allows for easy autocomplete:

```erb
<%= form_for @post do |f| %>
  <%= f.text_field :category, list: "categories_autocomplete" %>
  <datalist id="categories_autocomplete">
    <%= Category.all.each do |category| %>
      <option value="<%= category.name %>">
    <% end %>
  </datalist>
  <textarea name="post[content]"></textarea>
<% end %>
```

###  Using array parameters / Category has_many posts selection
Rails uses a [naming convention](http://guides.rubyonrails.org/v3.2.13/form_helpers.html#understanding-parameter-naming-conventions) to let you submit an array/hash of values to a controller.

The two basic structures are arrays and hashes. Hashes mirror the syntax used for accessing the value in params. For example if a form contains
```html
<input id="person_name" name="person[name]" type="text" value="Henry"/>
```
the params hash will contain
```
{'person' => {'name' => 'Henry'}}
```
and params[:person][:name] will retrieve the submitted value in the controller.

Hashes can be nested as many levels as required, for example
```html
<input id="person_address_city" name="person[address][city]" type="text" value="New York"/>
```
will result in the params hash being
```
{'person' => {'address' => {'city' => 'New York'}}}
```
Normally Rails ignores duplicate parameter names. If the parameter name contains an empty set of square brackets `[]` then they will be accumulated in an array. If you wanted people to be able to input multiple phone numbers, you could place this in the form:
```html
<input name="person[phone_number][]" type="text"/>
<input name="person[phone_number][]" type="text"/>
<input name="person[phone_number][]" type="text"/>
```
This would result in `params[:person][:phone_number]` being an array.

We can mix and match these two concepts. For example, one element of a hash might be an array as in the previous example, or you can have an array of hashes. For example a form might let you create any number of addresses by repeating the following form fragment
```html
<input name="addresses[][line1]" type="text"/>
<input name="addresses[][line2]" type="text"/>
<input name="addresses[][city]" type="text"/>
```
This would result in `params[:addresses]` being an array of hashes with keys `line1:`, `line2:` and `city:`. Rails decides to start accumulating values in a new hash whenever it encounters an input name that already exists in the current hash. There’s a restriction, however, while hashes can be nested arbitrarily, only one level of “arrayness” is allowed.

If you put this in a view so that the user can select 3 `posts` to associate with a `category`, it looks like this
```erb
<%= form_for @category do |f| %>
  <input name="category[post_ids][]">
  <input name="category[post_ids][]">
  <input name="category[post_ids][]">
<% end %>
```
When the form is submitted, your controller will have access to a `post_ids` param, which will be an array of strings.

We can write a setter method for this, just like we did for `category_name`:

```ruby
# app/models/category.rb
class Category < ActiveRecord::Base
   def post_ids=(ids)
     ids.each do |id|
       post = Post.find(id)
       self.posts << post
     end
   end
end
```
Now we can use the same wiring in the controller to set `post_ids` from `params`:
```ruby
# app/controllers/categories_controller.rb
class CategoriesController < ApplicationController
  def create
    Category.create(category_params)
  end

  private

  def category_params
    params.require(:category).permit(:name, :post_ids)
  end
end
```

#### Example with Song, Genres, Artists, Notes
The song form enables us to work on a song by assigning it an artist, genre and notes.
Here's the form
```erb
<%= form_for @song do |f| %>
  <%= f.label :title %>
  <%= f.text_field :title %>

  <%= f.label :artist_name, "Artist Name" %>
  <%= f.text_field :artist_name, list: "artists_autocomp" %>
  <datalist id="artists_autocomp">
	<% @artists.each do |artist| %>
		<option value="<%= artist.name %>">
	<% end %>
  </datalist>

  <%= f.label :genre_id, "Song's genre name" %>
  <%= f.collection_select :genre_id, @genres.order(:name), :id, :name %>
  
  <label>Add a few notes</label>
  <input id="song_notes_1" name="song[notes][][content]" placeholder="first note">
  <input id="song_notes_2" name="song[notes][][content]" placeholder="second note">

  <%= f.submit %>
<% end %>
```
This is handled by the Song controller
```ruby
class SongsController < ApplicationController
    ...
  def new
    @song = Song.new
    @artists = Artist.all
    @genres = Genre.all
  end

  def create
    # raise params.inspect
    # "song"=>{"title"=>"Show Must Go On", "artist_name"=>"Queen", "genre_id"=>"139", "notes"=>["Awesome", "Epic"]}
    @song = Song.new(song_params)

    if @song.save
      redirect_to @song
      # similar to redirect_to song_path(@song)
    else
      render :new
    end
  end
  
  private

  def song_params
    params.require(:song).permit(:title, :artist_name, :genre_id, notes: [:content])
  end  
    ...
end
```

And the Song Model
```ruby
class Song < ActiveRecord::Base
  belongs_to :artist
  belongs_to :genre
  has_many :notes

  validates_presence_of :title

  def artist_name=(name)
  	self.artist = Artist.find_or_create_by(name: name)
  end

  def artist_name
  	self.artist.name if self.artist
  	# same as self.try(:artist).try(:name)
  end

  def genre_name=(name)
  	self.genre = Genre.find_or_create_by(name: name)
  end

  def genre_name
  	self.genre.name
  end


  def notes=(notes)
  	notes.each do |note|
  		self.notes << Note.create(content: note[:content])
  	end
  	# takes from the array's hash with the :content key
  	# params.require(:song).permit(:title, :artist_name, :genre_id, notes: [:content])
  end

  def note_contents=(notes_array)
  	notes_array.each do |note|
  		self.notes << Note.create(content: note) unless note.empty?
  	end
  end

  def note_contents
  	self.notes.map { |note| note.content }
  end

  # def genre_name=(name)
  # 	self.genre = Genre.find_or_create_by(name: name)
  # end

  # def genre_name
  # 	self.genre.name if self.genre
  # end

end
```
It would also have been possible to have a simple non-nested array (no hash inside) to have the following combination for `notes`. 
```ruby
# songs/new view
<%= form_for @song do |f| %>
<%= f.text_field :title %>
<%= f.text_field :artist_name %>
<%= f.collection_select :genre_id, Genre.all, :id, :name %>
<input name='song[note_contents][]' id='song_notes_1'>
<input name='song[note_contents][]' id='song_notes_2'>
<input name='song[note_contents][]' id='song_notes_3'>
<%= f.submit %>
<% end %>

# controllers/songs_controller.rb
  def song_params
    params.require(:song).permit(:title, :artist_name, :genre_id, note_contents: [])
  end

# models/song.rb
  def note_contents=(notes)
    notes.each do |content|
      if content.strip != ''
        self.notes.build(content: content)
      end
    end
  end

  def note_contents
    self.notes.map(&:content)
    # same as self.notes.map { |note| note.content }
  end
```

Note on the `&:symbol` operator in ruby: the `&` operator takes its operand, converts it to a `Proc` object if it isn't already (by calling `to_proc` on it) and **passes it to the method as if a block had been used**.
```ruby
my_proc = Proc.new { puts "foo" }

my_method_call(&my_proc) # is identical to:
my_method_call { puts "foo" }

# another example
people.collect(&:name)
# is the same as
people.collect { |p| p.name }

# and lastly
people.select(&:manager?).collect(&:salary)
# is the same as
people.select { |p| p.manager? }.collect { |p| p.salary }
```

Note: Nested parameters work this way:
```ruby
params.require(:person).permit(:name, { emails: [] },
              friends: [ :name,
                         { family: [ :name ], hobbies: [] }
                        ]
            )
```
This declaration whitelists 

- `:name`
- `:emails`, as an array of permitted values
- `:friends`, as an array of resources with specific hash attributes
-   `:name` attribute as a *hash* key
-   `:family` attribute as an *array* containing *hashes* with the key `:name`
-   `:hobbies` as an *array* of permited values

Each attribute can have any of permitted scalar value. The permitted scalar types are String, Symbol, NilClass, Numeric, TrueClass, FalseClass, Date, Time, DateTime, StringIO, IO, ActionDispatch::Http::UploadedFile, and Rack::Test::UploadedFile.

For example
```ruby
# in the views form
<%= f.text_field :title %>
<%= f.text_field :artist_name, list: "artists_autocomp" %>
<%= f.collection_select :genre_id, @genres.order(:name), :id, :name %>
<input id="song_notes_1" name="song[notes][][content]" placeholder="first note">
<input id="song_notes_2" name="song[notes][][content]" placeholder="second note">

# in the controllers params
params.require(:song).permit(:title, :artist_name, :genre_id, notes: [:content])

# as the params sent by the view
"song"=>{
    "title"=>"Take Me Out", 
    "artist_name"=>"Franz Ferdinand", 
    "genre_id"=>"133", 
    "notes"=>[
        {"content"=>"Electric"}, {"content"=>"Frantic"}
    ]}
```

### Basic Nested Forms
Our data model looks like this:

  * `Person`
    * has many `addresses`
    * has a `name` (string)
  * `Address`
    * has one `person`
    * has the first line of the street address stored as `street_address_1` (string)
    * has the second line of the street address stored as `street_address_2` (string)
    * has a `city` (string)
    * has a `zipcode` (string)
    * has an `address_type` (string)

How do we write our `Person` form? We don't want to require our user to first create an `Address`, then create that `Person`. That's annoying. We want a single form for a `Person` containing several slots for their `addresses`.

Previously, we wrote setters like `Song#artist_name=` to find or create an `Artist` and connect them to the song.

That won't work here, because an address contains more than one field. In the `Artist` case we were just doing the `name`. With `Address`, it's "structured data". All that really means is it has multiple fields attached to it. When we build a form for it, the form will send a different key for each field in each address. This can get a bit unwieldy so we generally try to group a hash within the `params` hash, which makes things much neater.

The complete `params` object for creating a `Person` will look like the following. Using "0" and "1" as keys can seem a bit odd, but it makes everything else work moving forward. This hash is now more versatile. You can access nested values the standard way, with `params[:person][:addresses_attributes]["0"]` returning all of the information about the first address at 33 West 26th St.

```ruby
{
  :person => {
    :name => "Avi",
    :email => "avi@flombaum.com",
    :addresses_attributes => {
      "0" => {
        :street_address_1 => "33 West 26th St",
        :street_address_2 => "Apt 2B",
        :city => "New York",
        :state => "NY",
        :zipcode => "10010",
        :address_type => "Work"
      },
      "1" => {
        :street_address_1 => "11 Broadway",
        :street_address_2 => "2nd Floor",
        :city => "New York",
        :state => "NY",
        :zipcode => "10004",
        :address_type => "Home"
      }
    }
  }
}
```

Notice the `addresses_attributes` key. That key is similar to the `artist_name` key we used previously. Last time, we handled this by writing a `artist_name=` method. In this case, we're going to do something *super* similar. Instead of writing our own `addresses_attributes=` method (by the way we call it that way not to override `.addresses=(objects)` that AR gives us), we'll let Rails take care of it for us. We're going to use `accepts_nested_attributes_for` and the `fields_for` FormHelper.

Last time, we first wrote our setter method in the model. That would look like (but we'll not have to do it this time using a Rails macro)
```ruby
#inside app/models/person.rb

def addresses_attributes=(addresses_attributes)
    addresses_attributes.each do |address_attributes|
        self.addresses.build(address_attributes)
        #self is the user instance
    end
end
```
To give params's `:addresses_attributes` part that look like (note a hash, an array)
```
:addresses_attributes => [
    {:street_1 => "Address Street 1", :street_2 => "Address Street 2", :address_type => "Home"},
    {:street_1 => "Address Street 1", :street_2 => "Address Street 2", :address_type => "Biz"}
]
```
The form would be 
```html
<%= form_for(@person) do |f| %>
    <%= f.label :email %>
    <%= f.text_field :email %>
    
    <fieldset>
        <legend>Work Address</legend>
        <label>Street 1</label>
        <input type="text" name="person[addresses_attributes][][street_1]">
        <label>Street 2</label>
        <input type="text" name="person[addresses_attributes][][street_2]">
        <input type="hidden" name="person[addresses_attributes][][address_type]" value="Home">
    </fieldset>
<% end %>
```
It's always good to write HTML first and then integrate rails magic into it.


This time let's modify our `Person` model to include an `accepts_nested_attributes_for :addresses` line.

```ruby
class Person < ActiveRecord::Base
  has_many :addresses
  accepts_nested_attributes_for :addresses
end
```

Now open up `rails c` and run our `addresses_attributes` method that was created for us by `accepts_nested_attributes_for`.

```shell
2.2.3 :018 > new_person = Person.new
 => #<Person id: nil, name: nil, created_at: nil, updated_at: nil>

2.2.3 :019 > new_person.addresses_attributes={"0"=>{"street_address_1"=>"33 West 26", "street_address_2"=>"Floor 2", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work1"}, "1"=>{"street_address_1"=>"11 Broadway", "street_address_2"=>"Suite 260", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work2"}}
 => {"0"=>{"street_address_1"=>"33 West 26", "street_address_2"=>"Floor 2", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work1"}, "1"=>{"street_address_1"=>"11 Broadway", "street_address_2"=>"Suite 260", "city"=>"NYC", "state"=>"NY", "zipcode"=>"10004", "address_type"=>"work2"}}

2.2.3 :020 > new_person.save
   (0.2ms)  begin transaction
  SQL (0.8ms)  INSERT INTO "people" ("created_at", "updated_at") VALUES (?, ?)  [["created_at", "2016-01-14 11:57:00.393038"], ["updated_at", "2016-01-14 11:57:00.393038"]]
  SQL (0.3ms)  INSERT INTO "addresses" ("street_address_1", "street_address_2", "city", "state", "zipcode", "address_type", "person_id", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)  [["street_address_1", "33 West 26"], ["street_address_2", "Floor 2"], ["city", "NYC"], ["state", "NY"], ["zipcode", "10004"], ["address_type", "work1"], ["person_id", 3], ["created_at", "2016-01-14 11:57:00.403152"], ["updated_at", "2016-01-14 11:57:00.403152"]]
  SQL (0.1ms)  INSERT INTO "addresses" ("street_address_1", "street_address_2", "city", "state", "zipcode", "address_type", "person_id", "created_at", "updated_at") VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?)  [["street_address_1", "11 Broadway"], ["street_address_2", "Suite 260"], ["city", "NYC"], ["state", "NY"], ["zipcode", "10004"], ["address_type", "work2"], ["person_id", 3], ["created_at", "2016-01-14 11:57:00.405973"], ["updated_at", "2016-01-14 11:57:00.405973"]]
   (0.6ms)  commit transaction
 => true
```

This is a bit hard to read, but you'll notice that we have a method called `addresses_attributes=`. You didn't write that; `accepts_nested_attributes_for` wrote that. Then when we called `new_person.save` it created both the `Person` object and the two `Address` objects. Boom!

Now, we just need to get our form to create a `params` hash like that. Easy Peasy. We are going to use `fields_for` to make this happen.
```erb
# app/views/people/new.html.erb

<%= form_for @person do |f| %>
  <%= f.label :name %>
  <%= f.text_field :name %><br>

  <%= f.fields_for :addresses do |addr| %>
    <%= addr.label :street_address_1 %>
    <%= addr.text_field :street_address_1 %><br>

    <%= addr.label :street_address_2 %>
    <%= addr.text_field :street_address_2 %><br>

    <%= addr.label :city %>
    <%= addr.text_field :city %><br>

    <%= addr.label :state %>
    <%= addr.text_field :state %><br>

    <%= addr.label :zipcode %>
    <%= addr.text_field :zipcode %><br>

    <%= addr.label :address_type %>
    <%= addr.text_field :address_type %><br>
    <!--  or if we instantiate in the controller the address_type we won't even need to let the user the choice, or use a hidden field like below
        <%= addr.hidden_field :address_type, value: "..." %>
    -->
  <% end %>

  <%= f.submit %>
<% end %>
```

The `fields_for` line gives something nice and English-y. In that block are the fields for the addresses.

We're asking Rails to generate `fields_for` each of the `Person`'s addresses. However, when we first create a `Person`, they have no addresses. Just like `f.text_field :name` will have nothing in the text field if there is no name, `f.fields_for :addresses` will have no address fields if there are no addresses.

We'll take the most straightforward way out: when we create a `Person` in the `PeopleController`, we'll add two empty addresses to fill out. We also need to modify our `person_params` method to accept the new parameters. The final controller looks like this:

```ruby
class PeopleController < ApplicationController
  def new
    @person = Person.new
    @person.addresses.build(address_type: 'work')
    @person.addresses.build(address_type: 'home')
  end

  def create
    person = Person.create(person_params)
    redirect_to people_path
  end
  
  def index
    @people = Person.all
  end

  private

  def person_params
      params.require(:person).permit(
        :name,
        addresses_attributes: [
          :street_address_1,
          :street_address_2,
          :city,
          :state,
          :zipcode,
          :address_type
        ]
      )
  end
end
```

But it's best to know how to build the `addresses_attributes=` and not to use the `accepts_nested_attributes`. Though you can use `field_for` (that will create for your the person[addresses_attributes][1]...` structure in the form. It leaves more liberty and won't break when it gets a bit more complex.
`fields_for` will instantiate a form for each of the objects you instantiate in the controller `new` action (`@person.addresses.build(address_type: "Spacedress")`). Be careful because when we use `fields_for` it will give you a hash of attributes (`{"0" => ..., "1" => ...}`) and not an array anymore.
You can transform your array methods doing a hack
```ruby
addresses_attributes.each do |i, address_attribute|
    self.addresses.build(address_attributes)
end

# or
addresses_attributes.values.each do |address_attribute|
    self.addresses.build(address_attributes)
end
```

Don't forget to use `raise something.inspect` to debug your controllers and models (and `<%= binding.pry %>` for your views).

**Remember: every key in a hash needs to correspond to a writer (mass assignment), but don't override AR methods given by your associations.**

Note: we could also use the `FormBuilder` Object to give us the type and put it in a `<legend>` (use a binding pry to access it when debugging)
```erb
addr.object.address_type
=> "Home"
```

Let's say our `Person belongs_to :team` and we want to enable the person to choose an existing team or create it. Team has a `:name` and `:hometown`.
```ruby
def team_attributes=(team_attributes)
    self.team = Team.where (:name => team_attributes[:name]).first_or_create do |t|
        t.hometown = team_attributes[:hometown]
    end
end
```
Remember the forms (or views) never interact directly with our models (cooks), the controllers (waiters)  are in between. When we pass the giant hash to the controller, it will call the `new` action, that will call the custom `addresses_attributes=` method from the model. The controller pushes the relevant data to the model.

### Avoiding duplicates
One situation we can't use `accepts_nested_attributes_for` is when we want to avoid duplicates of the row we're creating.

In our address book app, perhaps it's reasonable to have duplicate address rows. For instance, both Jerry and Tim live on 22 Elm Street, so there are two address rows for 22 Elm Street. That's fine for those purposes.

But say we have a database of songs and artists. We would want `Artist` rows to be unique, so that `Artist.find_by(name: 'Tori Amos').songs` returns what we'd expect. If we want to be able to create artists _while_ creating songs, we'll need to use `find_or_create_by` in our `artist_attributes=` method:

```ruby
# app/models/song.rb

class Song < ActiveRecord::Base
  def artist_attributes=(artist)
    self.artist = Artist.find_or_create_by(name: artist.name)
    self.artist.update(artist)
  end
end
```

This looks up existing artists by name. If no matching artist is found, one is created. Then we update the artist's attributes with the ones we were given. We could also choose to do something else if we didn't want to allow bulk assigning of an artist's information through a song.

Note that `accepts_nested_attributes_for` and setter methods (e.g., `artist_attributes=`) aren't necessarily mutually exclusive. It's important to evaluate the needs of your specific use case and choose the approach that makes the most sense. Keep in mind, too, that setter methods are useful for more than just avoiding duplicates –– that's just one domain where they come in handy.

[Video review](https://www.youtube.com/watch?v=zZn0xWry6TE)

## Displaying `has_many through:`
Let's say you're making a blog and want to give users the ability to sign up and comment on your posts. What's the relationship between a post and a comment?  If you said, "A comment belongs to a post, and a post has many comments," give yourself a pat on the back!

What about the relationship between a user and a comment? Again, a user has many comments, and a comment belongs to the user. So far, this is pretty straightforward.

Things get slightly more complicated when we talk about the relationship between a user and the posts that the user has commented on. How would you describe that relationship? Well, a user obviously can comment on many posts, and a post has comments from many users. Yep, this is a many to many relationship. We can set up a many-to-many relationship using a join table. In this case, `comments` will act as our join table. Any table that contains two foreign keys can be thought of as a join table.  A row in our `comments` table will look something like this:

<table>
  <tr>
    <td> id </td>
    <td> content </td>
    <td> post_id </td>
    <td> user_id </td>
  </tr>
  
  <tr>
    <td> 1 </td>
    <td> "I loved this post!" </td>
    <td> 5 </td>
    <td> 3 </td>
  </tr>
</table>

For this instance, we know that the `Comment` with an ID of `1` was created by the `User` with an ID of `3` for the `Post` with an ID of `5`. We have all of the information we need to determine all of the posts that a particular user has commented on as well as all of the users who commented on any post. When we're done, we'll be able to simply call `@user.posts` to get a collection of all of those posts.

Let's set this up. First, we'll need migrations for `comments`, `posts`, and `users` tables. We've included migrations and models in this repo, so you can follow along.

```ruby
# db/migrate/xxx_create_posts

class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.string :title
      t.string :content
      t.timestamps null: false
    end
  end
end
```

```ruby
# db/migrate/xxx_create_users

class CreateUsers < ActiveRecord::Migration
   def change
    create_table :users do |t|
      t.string :username
      t.string :email
      t.timestamps null: false
    end
  end
end
```

```ruby
# db/migrate/xxx_create_comments

class CreateComments < ActiveRecord::Migration
  def change
    create_table :comments do |t|
      t.string :content
      t.belongs_to :user
      t.belongs_to :post
      t.timestamps null: false
    end
  end
end
end
```

In our models, we have the following:

```ruby
# app/models/post.rb

class Post < ActiveRecord::Base
  has_many :comments
  has_many :users, through: :comments
end
```

```ruby
# app/models/user.rb

class User < ActiveRecord::Base
  has_many :comments
  has_many :posts, through: :comments
end
```

```ruby
# app/models/comment.rb

class Comment < ActiveRecord::Base
  belongs_to :user
  belongs_to :post
end
```

Notice that we can't just declare that our `User` `has_many :posts` because our `posts` table doesn't have a foreign key called `user_id`. Instead, we tell Active Record to look through the `comments` table to figure out this association by declaring that our `User` `has_many :posts, through: :comments`. Now, instances of our `User` model respond to a method called `posts`. This will return a collection of posts that share a comment with the user.

### Displaying Comments on Our Posts

Now that our association is set up, let's display some data. First, let's set up our `Post#show` page to display all of the comments on a particular post. We'll include the username of the user who created the comment as well as a link to their show page.

In `app/controllers/posts_controller.rb`, define a `show` action that finds a particular post to make it available for display.

```ruby
# app/controllers/posts_controller.rb

class PostsController < ApplicationController

  def show
    @post = Post.find(params[:id])
  end
end
```

In our `Post#show` page, we'll display the title and content information for the post as well as the information for each comment associated with the post.

```erb
# app/views/posts/show.html.erb

<h2><%= @post.title %></h2>
<p>
  Content: <%= @post.content %>
</p>
Comments:
  <% @post.comments.each do |comment| %>
    <%= link_to comment.user.username, user_path(comment.user) %> said
    <%= comment.content %>
  <% end %>
```

This is the same as we've done before –– we're simply looking at data associated with posts and comments. Calling `comment.user` returns for use the `User` object associated with that comment. We can then call any method that our user responds to, such as `username`.

### Adding Posts to Our Users
Let's say that on our `User#show` page we want our users to see a list of all of the posts that they've commented on. What would that look like?

Because we've set up a join model, the interface will look almost identical. We can simply call the `posts` method on our user and iterate through.

```erb
# app/views/users/show.html.erb

<h2><%= @user.username %> </h2> has commented on the following posts:

<% @user.posts.each do |post| %>
  <%= link_to post.title, post_path(post) %>
<% end %>
```

### Conclusion
Displaying data via a `has_many, through` relationship looks identical to displaying data through a normal relationship. That's the beauty of abstraction –– all of the details about how our models are associated with each other get abstracted away, and we can focus simply on the presentation.

## `has_many through:` forms
Let's look at some different ways we can create data from our complex associations to make for a great user experience.

### 1st case: Join-model form
Sometimes, it may be appropriate for a user to create an instance of our join model directly. Think back to the hospital domain from our previous lab. It makes perfect sense that a user would go to `appointments/new` and fill out a form to create a new appointment.

```erb
<%= form_for @appointment do |f| %>
  <%= f.datetime_select :appointment_datetime %>
  <%= f.collection_select :doctor, Doctor.all, :id, :name %>
  <%= f.collection_select :patient, Patient.all, :id, :name %>
  <%= f.submit %>
<% end %>
```

In this example, a user is filling out a form, entering the date and time they'd like to come, and choosing their doctor and their name from a dropdown. We're assigning these properties directly to the appointment as it's created.

### 2nd case: nested has_many through
Some times, we need to be more abstract. Let's return to our blog example, but this time we'll say that a post can have many categories and categories can have many posts. For this, we'll need a join table –– let's call it `post_categories`. If our user wants to associate a post with a category, it doesn't make sense for them to go to `/post_categories/new` and fill out a "new post category form." That's confusing! Let's look at a more abstract way that we can do this thanks to the magic of Active Record.

```ruby
# app/models/post.rb

class Post < ActiveRecord::Base
  has_many :post_categories
  has_many :categories, through: :post_categories
end
```

```ruby
# app/models/category.rb

class Category < ActiveRecord::Base
  has_many :post_categories
  has_many :posts, through: :post_categories
end
```

```ruby
# app/models/post_category.rb

class PostCategory < ActiveRecord::Base
  belongs_to :post
  belongs_to :category
end
```
Now, let's make it so that our user can assign categories to a post when the post is created. We did this in a previous example without a join table. Our post was directly related to its category, and the `posts` table had the foreign key for its category. Because of this, instances of our `Post` class responded to a method called `category_ids=`. We called upon this method from our form helpers to build out a nested form.

Luckily, `has_many, through` functions exactly the same as a `has_many` relationship. **Instances of our `Post` class still respond to a method called `category_ids=`.** We'll use a helper method very similar to the `collection_select` we used previously.

```erb
# app/views/posts/_form.html.erb

<%= form_for post do |f| %>
  <%= f.label "Title" %>
  <%= f.text_field :title %>
  
  <%= f.label "Content" %>
  <%= f.text_area :content %>
  
  <%= f.collection_check_boxes :category_ids, Category.all, :id, :name %>
  
  <%= f.submit %>
<% end %>
```

This will create a checkbox field for each `Category` in our database. The HTML generated looks something like this:

```html
<input type="checkbox" value="1" name="post[category_ids][]" id="post_category_ids_1">
```

In our controller, we've setup our `post_params` to expect a key of `:category_ids` with a value of an array.

```ruby
# app/controllers/post_controller.rb

class PostsController < ApplicationController

  ...

  private
  
  def post_params
    params.require(:post).permit(:title, :content, category_ids:[])
  end
end
```

 After submitting the form, we end up with `post_params` that look something like:

```ruby
{"title"=>"New Post", "content"=>"Some great content!!", "category_ids"=>["2", "3", ""]}
```

Let's check out the SQL that fires from creating our new post.

```ruby
def create
  post = Post.create(post_params)
  redirect_to post
end
```

```SQL
INSERT INTO "posts" ("title", "content", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "New Post"], ["content", "Some great content!!"], ["created_at", "2016-01-15 21:25:59.963430"], ["updated_at", "2016-01-15 21:25:59.963430"]]

INSERT INTO "post_categories" ("category_id", "post_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["category_id", 2], ["post_id", 6], ["created_at", "2016-01-15 21:25:59.966654"], ["updated_at", "2016-01-15 21:25:59.966654"]]

INSERT INTO "post_categories" ("category_id", "post_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["category_id", 3], ["post_id", 6], ["created_at", "2016-01-15 21:25:59.968301"], ["updated_at", "2016-01-15 21:25:59.968301"]]
```

First, we're creating a new row in our `posts` table with `title` and `content`. Next, we create a row in our `post_categories` table for each ID number that was stored in our `category_ids` array. This functions just like it did with a `has_many` relationship, but, instead of creating a new record in our `categories` table, Active Record is creating two new rows in our `post_categories` table. This means that we can interact with our higher-level models directly without having to think too much at all about our join table - ActiveRecord will manage that relationship for us behind the scenes.

### Creating new categories
We can now associate categories with our posts, but what about creating new categories? If I'm posting about baby elephants and no one has created the category "Super Cute!" yet, I want to be able to create it at the same time as my post.  Again, this will be very similar to the way we've done things before.

First, we want a text field to enter the name of our new category. The value of the name should be nested under our `post_params`, so we don't have to add too much code to our controller. We can use the `fields_for` helper to do this very easily.

```erb
# app/view/post/_form.html.erb

<%= form_for post do |f| %>
  <%= f.label "Title" %>
  <%= f.text_field :title %>
  
  <%= f.label "Content" %>
  <%= f.text_area :content %>
  
  <%= f.collection_check_boxes :category_ids, Category.all, :id, :name %>
  <%= f.fields_for :categories, post.categories.build do |categories_fields| %>
    <%= categories_fields.text_field :name %>
  <% end %>
  <%= f.submit %>
<% end %>
```

The `fields_for` helper takes two arguments: the associated model that we're creating and an object to wrap around. In this case, we've passed in the `:categories` association and built an empty category associated with the post.
Let's look at the html that this generated for us.
```html
<input type="text" name="post[categories_attributes][0][name]" id="post_categories_attributes_0_name">
```

Our params hash will now have a key of `:categories_attributes` nested under the key of `post`. Let's add that to our strong params and tell it to expect a key of `name` inside for the category's name.
```ruby
# app/controllers/post_controller.rb

class PostsController < ApplicationController

  ...

  private
  
  def post_params
    params.require(:post).permit(:title, :content, category_ids:[], categories_attributes: [:name])
  end
end
```
Note that `fields_for :categories` will automatically name the field `post[:categories_attributes]`, this is why we whitelist `categories_attributes` in the params.

Now, when we do mass assignment, our `Post` model will call a method called `categories_attributes=`, but only if you put the symbol as a plural when it is a `has_many ...` (`:categories`, not `:category`), and singular if it is a `belongs_to ...`. Let's add that method to our model using the `accepts_nested_attributes_for` macro.
```ruby
class Post < ActiveRecord::Base
  has_many :post_categories
  has_many :categories, through: :post_categories
  accepts_nested_attributes_for :categories

end
```

Voila! Just like when our models were directly related, we can now create categories that are automatically associated with our new post. Looking at the SQL that gets generated, we can see that it's creating new instances of `PostCategory` without us ever having to interact with them.

```shell
(0.1ms)  begin transaction
  SQL (0.4ms)  INSERT INTO "posts" ("title", "content", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["title", "A New Post!"], ["content", "It was the best of times, it was the worst of times"], ["created_at", "2016-01-15 22:08:37.271367"], ["updated_at", "2016-01-15 22:08:37.271367"]]
  SQL (0.1ms)  INSERT INTO "categories" ("name", "created_at", "updated_at") VALUES (?, ?, ?)  [["name", "Really Neat!"], ["created_at", "2016-01-15 22:08:37.277421"], ["updated_at", "2016-01-15 22:08:37.277421"]]
  SQL (0.3ms)  INSERT INTO "post_categories" ("post_id", "category_id", "created_at", "updated_at") VALUES (?, ?, ?, ?)  [["post_id", 9], ["category_id", 5], ["created_at", "2016-01-15 22:08:37.279564"], ["updated_at", "2016-01-15 22:08:37.279564"]]
   (1.0ms)  commit transaction
```

Still, there's a problem. We're creating a new category each time, regardless of whether or not it exists. It will look pretty weird if three people type in "Super Cute!" and we get three different categories. In this case, we need to customize the way our category is created. Luckily, we can easily do this by creating our own `categories_attributes=` method.
```ruby
class Post < ActiveRecord::Base
  has_many :post_categories
  has_many :categories, through: :post_categories
  accepts_nested_attributes_for :categories

  def categories_attributes=(category_attributes)
    category_attributes.values.each do |category_attribute|
      category = Category.find_or_create_by(category_attribute)
      self.categories << category
    end
  end
end
```

Now, we're only creating a new category if it doesn't already exist with the current name. We're also using a cool method called `categories<<`.  What's great about this is you can mentally think of it as two steps. First, we call `self.categories`, which returns an array of `Category` objects, and then we call the shovel (`<<`) method to add our newly found or created `Category` object to the array. 

We could imagine later calling `save` on the `Post` object and this then creating the `post_categories` join record for us. In reality, this is syntactic sugar for the `categories<<` method. That's the actual method name, and behind the scenes it will create the join record for us. It's one of the methods dynamically created for us whenever we use a `has_many` association. The end result is this method doing exactly what Active Record was doing for us before; we're just customizing the behavior a little bit.

As you can see, it doesn't really matter how complex our associations are –– Active Record is really good at managing that complexity for us. We can always drop down a level of abstraction if needed to customize the way our application behaves.

#### Cool example of what can be done
Working on a blog domain model, following this db:schema
```ruby
create_table "categories", force: :cascade do |t|
    t.string   "name"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "comments", force: :cascade do |t|
    t.string   "content"
    t.integer  "user_id"
    t.integer  "post_id"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  add_index "comments", ["post_id"], name: "index_comments_on_post_id"
  add_index "comments", ["user_id"], name: "index_comments_on_user_id"

  create_table "post_categories", force: :cascade do |t|
    t.integer  "post_id"
    t.integer  "category_id"
    t.datetime "created_at",  null: false
    t.datetime "updated_at",  null: false
  end

  add_index "post_categories", ["category_id"], name: "index_post_categories_on_category_id"
  add_index "post_categories", ["post_id"], name: "index_post_categories_on_post_id"

  create_table "posts", force: :cascade do |t|
    t.string   "title"
    t.string   "content"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end

  create_table "users", force: :cascade do |t|
    t.string   "username"
    t.string   "email"
    t.datetime "created_at", null: false
    t.datetime "updated_at", null: false
  end
```
Most of the actions are in the `PostsController`
```ruby
class PostsController < ApplicationController
  def show
    @post = Post.find(params[:id])
    @users = @post.comments.map(&:user)
    @comment = @post.comments.build
  end

  def index
    @posts = Post.all
  end

  def new
    @post = Post.new
    @post.categories.build
  end

  def create
    # raise params.inspect
    # "post"=>{"title"=>"The new post", "content"=>"Llorem ipsum isn't better", "category_ids"=>[""], "categories_attributes"=>{"name"=>"New cat as category"}}
    post = Post.create(post_params)
    redirect_to post
  end

  private

  def post_params
    params.require(:post).permit(:title, :content, category_ids:[], categories_attributes: [:name])
  end
end
```
In the show page you can see a post but also the guys who have commented, and add your comment directly (calls the `Comment` controller).
```erb
<h1><%= @post.title %></h1>
<p><%= @post.content %></p>

<h3>Categories</h3>
<% @post.categories.each do |cat| %>
    <p><%=cat.name%></p>
<% end %>


<h3>They have commented:</h3>
<ul>
	<% @users.uniq.each do |user| %>
		<li><%= link_to user.username, user_path(user) %></li>
	<% end %>
</ul>

<h3>The <%= "comment".pluralize(@post.comments.count) %></h3>
<ul>
	<% @post.try(:comments).each do |comment| %>
		<li><%= link_to comment.user.try(:username), user_path(comment.user) if comment.user %> says: <%= comment.content %></li>
	<% end %>
</ul>

<h3>Add a new comment</h3>
<%= form_for @comment do |f| %>
	<%= f.label :content %>
	<%= f.text_field :content %>
	<br><br>

	<%= f.label :user_id, "Which user are you?" %>
	<%= f.collection_select(:user_id, User.all, :id, :username, include_blank: true) %>
	<br><br>

	Or create a new user right away
	<%= f.fields_for :user, @comment.build_user do |j|  %>
		<%= j.label :username %>
		<%= j.text_field :username %>
	<% end %>

	<%= f.hidden_field :post_id, value: @post.id %>

	<%= f.submit %>
<% end %>
```
And a regular form to create a post
```erb
<h1>Create a new post</h1>

<%= form_for @post do |f| %>
	
	<%= f.label :title %>
	<%= f.text_field :title %>
	<br><br>
	<%= f.label :content %>
	<%= f.text_area :content %>
	<br><br>
	<%= f.label :category_ids, "Select Categories" %>
	<%= f.collection_check_boxes :category_ids, Category.all, :id, :name %>
	<br><br>
	<%= f.fields_for :categories do |cat| %>
		<%= cat.label :name, "New category name" %>
		<%= cat.text_field :name %>
	<% end %>

	<br><br>
	<%= f.submit %>	

<% end %>
```

## Layouts and Templates in Rails
Imagine that you're tasked to build an online store app with Rails. You would probably have a few different views in this app, for example:
- A list of products
- A detail view that shows more info for a selected product
- A shopping cart
Across all these views you would want a consistent look. This consistent look perhaps contains something like a logo, navigation links, a search bar, and a footer at the bottom that contains some info about the shop.

You start building the list of products, and you end up with an action template that looks something like this. It has a nav component, a list of products, and a footer.


```erb
<!-- app/views/products/index.html.erb -->

<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>  
</head>
<body>
    <div class="navigation">
      <ul>
        <a href="/products">Products</a>
        <a href="/cart">Cart</a>
      </ul>
    </div>

    <h1>The Product List</h1>

    <ul>
      <% @products.each do |job| %>
        <li><%= link_to 'Show', job %></li>
      <% end %>
    </ul>  

    <div class="footer">
      <p>This shop promises the lowest prices in widgets!</p>
    </div>    
</body>
</html>
```

Great, so you have a list of products now! Any shop should have that, shouldn't it? Next, you decide to build the view that will show the product details when customers click on a link for that product.

You finish sorting out the view's structure with a nav and footer. You open up `app/views/products/index.html.erb`, select all of the code, and copy it onto your clipboard. Now you are ready to paste it into your new template and customize it to show product info.

Luckily, you don't need to copy content from one template file to the next because layouts in Rails are enabled by default. When you generate a new Rails app, it generates a layout for you.

When you render a template for an action without specifying a different layout to use, **by default Rails will use the LAYOUT found at this location: `app/views/layouts/application.html.erb`.**

```erb
<!-- app/views/layouts/application.html.erb -->

<!DOCTYPE html>
<html>
<head>
  <title>Flatiron Store</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
</head>
<body>

<%= yield %>

</body>
</html>
```

There are 2 things to consider: templates and content (yield or partials).

**`yield` is what Rails uses to decide where in the layout to render the CONTENT for the action, inside of the TEMPLATE**. If you don't put a `yield` in your layout, the layout itself will render just fine, but any additional content coded into the action templates will not be correctly placed within the layout.

At its simplest level, this is what happens when a request is made to your Rails application:

1. Rails finds the **template** for the corresponding action based either on convention or any other options passed to the `render` method in your controller action.
2. Similarly, it then finds the correct layout to use, either through naming/directory conventions or from specific options that you provided.
3. Rails uses the action template to generate the **content specific to the action**. (Note that the template might be composed of partial views, which you'll learn about a bit later.)
4. It then looks for the layout's `yield` statement and inserts the action's template there.

So this means that, for every request handled by Rails, at most one layout and action template will be used. The action template can call out to other templates, called partials, to render itself.

Rails uses a simple convention to find the correct layout for your request. If you have a controller called `ProductsController`, Rails will see whether there is a layout for that controller at `layouts/products.html.erb`. Similarly, if you have a controller called `AdminController`, it'll look for a layout at `layouts/admin.html.erb`. **If it can't find a layout specific to your controller, it'll use the default layout at `app/views/layouts/application.html.erb`.**

With the exception of the admin section of a site, most applications use the default layout for everything, so there's no need to have a layout for each controller. You want to have a consistent look and feel throughout your site, using a different layout only if the situation really warrants it.

### Overriding Conventions

If you need to override the conventions explained above, you can easily do so. For example, if you have a controller called `ShoppingCartController` and want to use the layout at `layouts/products.html.erb`, you have two options:

1. If you want to use the products layout for every action, simply specify that you want to use the products layout by invoking the `layout` method in your controller, passing it a string that it can use to find the desired layout:

  ```ruby
  class ShoppingCartController < ApplicationController
    layout "products"
  end
  ```

2. If you want to use the products layout only for a particular action, simply use the `render` method in the controller action, specifying the layout you want it to use like this:

  ```ruby
  class ShoppingCartController < ApplicationController
    def list
      render :layout => "static"
    end
  
    # the rest of the actions will use standard conventions
  end
  ```

If you want to render your action template without a layout, you can do the following:

```ruby
class ShoppingCartController < ApplicationController
  def list
    render :layout => false
  end

  # the rest of the actions will use standard conventions
end
```

**Note:** It's pretty unusual to not render the layout in a standard action.  However, once you start using AJAX (JavaScript), it's quite common. Keep this in the back of your mind when you get to JavaScript.

## Partials
As you know, while coding we are generally trying to not repeat our code. If we see a repeated chunk of code in different methods, we sometimes extract that chunk of code into its own method, which we can then reference in multiple places. We can apply a similar tool to reduce repetition in HTML. Partials are view-level files that only form one part of an HTML page. By using a partial, we can remove repeated pieces of HTML and add better organization to the code in our views.

This is the code in the `posts#new` form:
```erb
<!-- app/views/posts/new.html.erb -->

<%= form_tag posts_path do %>
  <label>Post title:</label><br>
  <%= text_field_tag :title %><br>

  <label>Post Description</label><br>
  <%= text_area_tag :description %><br>

  <%= submit_tag "Submit Post" %>
<% end %>
```
And this is the code in the `posts#edit` form:
```erb
<!-- app/views/posts/edit.html.erb -->

<h3>Post Form</h3>

<%= form_tag post_path(@post), method: "put" do %>
  <label>Post title:</label><br>
  <%= text_field_tag :title %><br>

  <label>Post Description</label><br>
  <%= text_area_tag :description %><br>

  <%= submit_tag "Submit Post" %>
<% end %>
```

Except for the first line of the form, the code is pretty much the same! The labels and field tags are the same. All of that duplication is not good in code. Duplication means twice the amount of code to maintain, twice the opportunity for bugs, and two slightly different forms when our interface should be consistent.


Instead of duplicating all of that code, we just want to write it once in our partial and call it from both our edit and show views. 

First, let's create a new file in `app/views/posts/` called `_form.html.erb`. To indicate that this file is a partial (and only part of a larger view), an underscore is prefixed to the filename.

Second, let's remove the repeated code in `app/views/posts/edit.html.erb`. The file should look like this:

```erb
<h3>Post Form</h3>

<%= form_tag post_path(@post), method: "put" do %>
<% end %>
```
Note that we left in the non-duplicated code. Now, let's also remove the duplicated code in the `app/views/posts/new.html.erb` file. The file should look like this:

```erb
<%= form_tag posts_path do %>
<% end %>
```
We left the code that is unique to each view and removed the duplicated code inside the `form_tag` blocks.

Third, we'll place the duplicated code in a new file called `app/views/posts/_form.html.erb`. The file should look as follows:
```erb
<label>Post title:</label><br>
<%= text_field_tag :title %><br>

<label>Post Description</label><br>
<%= text_area_tag :description %><br>

<%= submit_tag "Submit Post" %>
```

Fourth, we need to render the code into the `posts/edit` and `posts/new` pages by placing `<%= render "form" %>` where we want the code in the partial to be rendered. Notice that, while the file name of our partial starts with an underscore, when we reference it there is no underscore. 

Notice that we don't specify the folder that the partial lives in, such as `<%= render 'posts/form' %>`. The reason we didn't need this (even though it would have worked if we had included it) is that both the `posts/new` and `posts/edit` files are referencing a partial housed in the same folder in which they reside, `app/views/posts`. When referencing a partial from a different folder, we must include the folder name as well (e.g., `<%= render 'posts/form' %>` as opposed to `<%= render 'form' %>`).

Our `posts/new` file should now look like this:
```erb
<!-- app/views/posts/new.html.erb -->

<%= form_tag posts_path do %>
 <%= render 'form' %>
<% end %>
```

And our `posts/edit` file like this:
```erb
<!-- app/views/posts/edit.html.erb -->

<h3>Post Form</h3>

<%= form_tag post_path(@post), method: "put" do %>
  <%= render 'form' %>
<% end %>
```

And that's it –– we're all done!

### Rendering a partial from a different folder

Let's take a look at our `authors/show.html.erb` file:

```erb
<%= @author.name %>
<%= @author.hometown %>
```

And now look at the code in `posts/show.html.erb`:

```erb
<%= @post.author.name %>
<%= @post.author.hometown %>

<h1><%= @post.title %></h1>
<p><%= @post.description %></p>
```

See the repetition? In both places, we are using the `Author` object to call the `.name` and `.hometown` methods. The first thing we have to fix is the slight difference between the templates. Let's make the beginning portion of the `posts/show` template match the `authors/show` template.

```erb
<!-- app/views/posts/show.html.erb -->

<%= @author.name %>
<%= @author.hometown %>

<h1><%= @post.title %></h1>
<p><%= @post.description %></p>
```

Then, let's make a new partial called `app/views/authors/_author.html.erb` and place the repeated code in the file. It should look like the following:

```erb
<!-- app/views/authors/_author.html.erb -->

<%= @author.name %>
<%= @author.hometown %>
```

Now we can just render this partial in our `authors/show` page by doing the following:

```erb
<!-- app/views/authors/show.html.erb -->

<%= render 'author' %>
```

Let's try making the same change to our `posts/show` page:

```erb
<!-- app/views/posts/show.html.erb -->

<%= render 'author' %>

<h1><%= @post.title %></h1>
<p><%= @post.description %></p>
```

Uh oh, something went wrong. This won't work because, if we don't specify the partial's parent folder, Rails assumes that the partial lives in the same folder as the view that's calling it. In this case, it looks for a file in the `posts` directory called `_author.html.erb` and doesn't find it. We need to tell Rails to go outside the folder by being explicit about the folder and file name that it should render. We can do that by changing the above code to the following:

```erb
<!-- app/views/posts/show.html.erb -->

<%= render 'authors/author' %>

<h1><%= @post.title %></h1>
<p><%= @post.description %></p>
```

We're almost there! One more problem is that our partial assumes it has access to an instance variable called `@author`. The partial won't function without it! We'll need to modify the `PostsController` to have it set that instance variable.

Change the `posts#show` action in the controller to look like the following:

```ruby
# app/controllers/posts_controller.rb

def show
  @post = Post.find(params[:id])
  @author = @post.author
end
```

And now we are done! Great job!

## Partial with Locals
Partials help us break our code up into reusable chunks. They also often have implicit dependencies that can lead to bugs. To make the implicit explicit, use locals whenever your partials depend on data to work.

Partial code from other directories may look OK at first, but it poses some problems.  When we render the partial authors/author, we are not being explicit about that partial's dependencies.
A dependency is data that the code requires in order to work (like passing a variable into a method).  In this case, the dependency of the author partial is the instance variable @author.  Without that instance variable, the code won't work.  Unfortunately, that dependency is defined far away in the controller.

Let's see how local variables make our code more explicit. This is what the entire show view looks like:
```erb
<!-- app/views/posts/show.html.erb -->

<h1>Information About the Post</h1>
<%= render partial: "authors/author", locals: {post_author: @author} %>
<%= @post.title %>
<%= @post.content %>
```

Notice a few things.  First, we are no longer passing the render method a string; we're passing a hash with two key-value pairs. 
- The first key-value pair tells Rails the name of the partial to render. 
- The second key-value pair contains a key of `locals` pointing to a hash of variables to pass into the partial.  Within that nested hash, the key is the name of the variable and its value is the value you'd like it to have in the partial.

When we use locals, we need to make sure that the variables we refer to in our partial have the same names as the keys in our locals hash.

In our example partial, `app/views/author/_author.html.erb`, we need to change our code from:
```erb
<ul>
  <li> <%= @author.name %></li>
  <li> <%= @author.hometown %></li>
</ul>
```

to
`app/views/author/_author.html.erb`
```erb
<ul>
  <li> <%= post_author.name %></li>
  <li> <%= post_author.hometown %></li>
</ul>
```

In other words, the way we use locals with a partial is similar to how we pass arguments into a method.  In the locals hash, the `post_author:` key is the argument name, and the value of that argument, `@author`, is the corresponding value stored as `post_author` and passed into the method.
With locals, we can completely eliminate the `@author = @post.author` line in the `posts#show` controller action, instead only accessing that data where we need it, in the partial.

Let's remove that line of code in our controller and in the view pass through the author information by changing our code to the following:

`app/controllers/posts_controller`
```ruby
  ...
  def show
    @post = Post.find(params[:id])
  end

```

`app/views/posts/show.html.erb`
```erb
Information About the Post
<%= render partial: "authors/author", locals: {post_author: @post.author} %>
<%= @post.title %>
<%= @post.content %>
```

This code is much better.  We are being more explicit about our dependencies, reducing lines of code in our codebase, and reducing the scope of the author variable.

## Rendering collections
Up until now our only way to render collections was somewhat manually. We could iterate over an array and render the partial for each object in the array. Let's see how Rails can abstract this into a nicer syntax.

Currently, our posts#index view is manually rendering the partial in a loop.
```erb
<% @posts.each do |post| %>
  <%= render :partial => "post", {:locals => {:post => post}} %>
<% end %>
```
Rails offers a great way to render a collection using a partial by passing the collection option to the render method.
```erb
<%= render :partial => "post", :collection => @posts %>
```
Another even more abstract method Rails gives us to do this is passing an array directly to the render method.
```erb
<%= render @posts %>
```
This approach is a bit more abstract. Under the hood Rails uses the convention that you will have a partial with the name of the models in the collection. Rails will even render a collection of heterogeneous models ([customer, order, customer]) calling the correct partial for each one.

What happens if the collection you pass to your render call is empty? If you don't handle this exception the render method will return nil and nothing will appear on the screen. A useful trick is to use the || operator to print something to the screen to alert the user to this.
```erb
<%= render(@posts) || "There are no blog posts!" %>
```
Note: When dealing with an empty collection, you'll need to use () to wrap that collection.

[Video Review](https://www.youtube.com/watch?v=XpthyOc767U)

## Separation of concerns
Separation of Concerns is a design principle that helps us keep code logically organized by ensuring that each part of an application concerns itself with a specific job.

Remember the restaurant analogy? In a restaurant, there is a separation of concerns among the staff. A server takes your order, a cook prepares your order, a food runner brings it to your table, and a busser cleans up after you — and all of them work together to provide the cohesive experience of eating out.

We could put all of our code in one file, but then the file is in the same spot as that overworked cook — doing so much that nobody knows what it's supposed to be doing. In a tiny application with only one developer, this might work for a while, but as the application and the team grow everyone needs to know where to go to find certain kinds of code.
With that in mind, let's look again at the components of the MVC pattern:
- Models: Provide the business logic of the application and access and store data. Models do most of the 'heavy lifting' of manipulating data and enforcing the 'rules' of the application. This is also sometimes called domain logic because it's all the stuff that's specific to the domain of the application. Sticking with the restaurant analogy, the business logic governs things like recipes and prices and how old you have to be to order a margarita.
- Views: Provide the presentation logic of the application and allow for user interaction. Views only care about showing a user formatted data and giving them ways to interact with it. If business logic tells us what food the restaurant has and how much it costs, presentation logic puts all that together into a menu that looks great and helps a customer choose what to eat (and also entices them to order that margarita).
- Controllers: Provide communications between the models and the views and help control the flow of data.

To put that into context with Rails MVC, picture a blog application. The view is only concerned with the presentation of blog posts. That's what it knows how to do, and that's all it knows how to do. The model is concerned with the content of all of the posts. The controller is concerned with which blog post you want to see. By keeping them separate, we get to do things like create a single view that can be used to display any post by having the controller ask for a specific post from the model.
By knowing which component is responsible for which concern, we can know where to go to find or add code based on what it does. If we need to write code to hit the database and search for specific blog posts, that seems like business logic and belongs in the model. If we need to write code to display those search results in a nice way and make sure the post date is formatted properly, that's presentation logic and belongs in the view.

We know the view is responsible for presentation logic, but what happens when our application grows and some presentation logic needs to be shared across more than one view? That's where helpers come in, and we'll be tackling those in the next lesson.

### Refactoring Views with helpers
Let's imagine we're working on a blog application.
We've already added the presentation logic to show a blog post:

```erb
<!-- app/views/posts/show.html.erb -->

<h1><%= @post.title %></h1>
<p><%= @post.description %></p>
```

However, our readers really want to know *when* the post was posted or edited. Fair enough.

We know our `Post` model has a timestamp field called `updated_at`, so we decide to display that.

```erb
<!-- app/views/posts/show.html.erb -->

<h1><%= @post.title %></h1>
<p>Last updated: <%= @post.updated_at %></p>
<p><%= @post.description %></p>
```

**Note:** reading about `strftime` in the [docs](http://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) is great, but playing with it live is even more fun, so check out [For a Good Strftime](http://www.foragoodstrftime.com/).

Okay, armed with `strftime` and having read the ancient runes that help us understand the different format directives (e.g. `%A`, `%p`, etc.), we end up with this:

```erb
<!-- app/views/posts/show.html.erb -->

<h1><%= @post.title %></h1>
<p><%= @post.updated_at.strftime("Last updated %A, %b %e, at %l:%M %p") %></p>
<p><%= @post.description %></p>
```

**Top-tip:** Notice how we included the text and punctuation in the argument to `strftime`. It will very nicely ignore anything that isn't one of the format directives and pass it through as part of the final string. This is helpful because it saves us from the awkward business of having to try to parse out the time string and concatenate parts of it together with other strings in order to create more readable output.

If we try copying this code to display the time everywhere we need to use the time to show it to our users, we could copy paste, but it would become not-DRY. We have to remember (or, worse, search for) every place we wrote that code to display the last updated date and time, and then we have to make the *same* change in all of those places. 

DRY is a software design principle that, like Separation of Concerns, helps us make decisions about how to organize our code. The formal, very dry statement of DRY is: "Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

**//Flat-fact"** The opposite of DRY is WET, which may or may not stand for "We Enjoy Typing" or "Write Everything Twice."

What about the model? We only ever get `updated_at` from the model, so it's tempting to do something like:

```ruby
# app/models/post.rb

def last_updated
  updated_at.strftime("Last updated %A, %b %e, at %l:%M %p")
end
```

and then use it like `@post.last_updated` on the view. That would certainly solve our DRY problem, but if we think back on MVC and Separation of Concerns, we've created a new problem. Model code isn't concerned with *presentation logic*.

Rails provides us with a great way to extract common presentation logic from multiple views: helpers! Helpers are methods that are available to your views and encapsulate a common bit of code. If you've used a `link_to` or a `text_field` then you've already come across helpers. Rails has a ton of them built-in to help keep you from having to repeat all the code necessary to build a link to a given route, display an image, or create a form.

We can also create our own helpers to solve our own problems.

Helpers are generally organized by controller. If you use `rails g scaffold` or `rails g controller`, Rails will create a helper for your controller and put it in the `app\helpers` directory. If we look in our helpers directory, we'll see `application_helper.rb`, `authors_helper.rb`, and `posts_helper.rb`. This matches what we have in our `controllers` directory — `application_controller.rb`, `authors_controller.rb`, and `posts_controller.rb`.

Let's use Separation of Concerns to decide where to make our helper. Displaying the date of the last time the post was updated isn't an application-wide concern; it's only a post concern. So let's put it in the `PostsHelper`.

```ruby
# app/helpers/posts_helper.rb

def last_updated(post)
  post.updated_at.strftime("Last updated %A, %b %e, at %l:%M %p")
end
```

Why are we passing `post` as an argument to the helper? If you look back at our three views, you'll see that sometimes we're dealing with an instance variable (`@post`), and sometimes we're dealing with a local variable (`post`). Our helper doesn't know anything that we don't directly tell it, so it can't assume a specific variable name. We will need to tell it which post we want it to act on.

Now that we have our helper, let's DRY up those views.

```erb
<!-- app/views/posts/show.html.erb -->

<h1><%= @post.title %></h1>
<p><%= last_updated @post %></p>
<p><%= @post.description %></p>
```

```erb
<!-- app/views/posts/index.html.erb -->

<% @posts.each do |post| %>
  <div>
    <%= post.title %> - <%= last_updated post %>
  </div>
<% end %>
```

```erb
<!-- app/views/posts/edit.html.erb -->

<%= form_for(@post) do |f| %>
  <label><%= last_updated @post %></label><br>
  <label>Post title:</label><br>
  <%= f.text_field :title %><br>

  <label>Post Description</label><br>
  <%= f.text_area :description %><br>

  <%= f.submit %>
<% end %>
```

Our blog also has an author page that lists the posts by that author. What do we do if we want to also show the last updated time on that page? The answer is: the same thing we just did!

```erb
<!-- app/views/authors/show.html.erb -->

<h1><%= @author.name %></h1>
<p>Posts:</p>
<% @author.posts.each do |post| %>
  <div><%= post.title %> - <%= last_updated post %></div>
<% end %>
```

Helpers are *organized* by controller, but they aren't *restricted* to a single controller. Just like you can use a Rails `link_to` helper in any view, you can also use any of your own helpers in any view. Our `last_updated` helper is still a `Post` concern (so we know to look for it in the `PostsHelper`), but if an author page or any other page needs to use it they can. Just like our models, views, and controllers, helpers are separated by concern to help us keep our codebase organized.

What about things that arent the concern of a single controller and are applicable to the entire application? For these cases, we have the `application_helper`. This helper is created automatically with your Rails project and is where you keep helpers that are related to the application itself rather than any given model or controller.

In applications where users can log in, the application helper will often have a method to expose a `current_user`, something which gets used on almost every view in the system.

For our blog application, we decide we want to dynamically change the `<title>` of each page based on what the user is looking at. On an author's show page, it might be the author name. On a blog post page, it might be the post's title. Since we already know we're going to be repeating the same basic snippet of code across many views, we're going to jump right to creating a helper.

The page `title` isn't strictly the concern of a `Post` or an `Author`, even though it may use data from either of those things. Since it's a broader concern than just one controller/model, it's a prime candidate for the `ApplicationHelper`.

```ruby
# app/helpers/application_helper.rb

def title(text)
  content_for :title, text
end
```

**Note:** We use the Rails `content_for` helper within our custom helper here. Helpers on helpers on helpers! What this will do is send our `text` to the place in our application layout that is expecting some content for the `:title`.

```erb
<!-- app/views/layouts/application.html.erb -->

<head>
  <title><%= yield :title %></title>
</head>
```

You can read more about `content_for` in the [Layouts and Rendering](http://guides.rubyonrails.org/layouts_and_rendering.html#using-the-content-for-method) RailsGuide.

Now that we have our `title` helper, we can use it everywhere to change the page title based on the content.

```erb
<!-- app/views/authors/show.html.erb -->

<% title @author.name %>

<h1><%= @author.name %></h1>
<p>Posts:</p>
<% @author.posts.each do |post| %>
  <div><%= post.title %> - <%= last_updated post %></div>
<% end %>
```

```erb
<!-- app/views/posts/show.html.erb -->

<% title @post.title %>

<h1><%= @post.title %></h1>
<p><%= last_updated @post %></p>
<p><%= @post.description %></p>
```

And just like that, we are using an application helper to dynamically set the page title on our post and author pages!

[Refactoring Views into Partials and Helpers](https://www.youtube.com/watch?v=UYhkBd2Mnl0)

### Model Class Methods 
We have our list of blog posts, which is great, but our readers would like to be able to filter the list by author. Let's do what every great blog does and pander to the masses!

We want to start by adding some controls to our view to do the filtering:
```erb
<!-- app/views/posts/index.html.erb -->

<!-- add this new code above the @posts.each loop -->
<% if !params[:author].blank? %>
  <% @posts = Post.where(author: params[:author]) %>
<% end %>

<h1>Believe It Or Not I'm Blogging On Air</h1>

<div>
  <h3>Filter posts:</h3>
  <%= form_tag("/posts", method: "get") do %>
    <%= select_tag "author", options_from_collection_for_select(Author.all, "id", "name"), include_blank: true %>
    <%= submit_tag "Filter" %>
  <% end %>
</div>
<!-- end new code -->

<% @posts.each do |post| %>

  ...
  
<% end %>
```

The `options_from_collection_for_select(Author.all, "id", "name")` returns a string of option tags that have been compiled by iterating over the `collection` and assigning the result of a call to the `value_method` as the option value and the `text_method` as the option text.
```ruby
options_from_collection_for_select(collection, value_method, text_method, selected = nil)
```

And to ensure that our view has access to the controller's `params` hash, let's go into `posts_controller` and add this line near the top:
```ruby
# app/controllers/posts_controller.rb

class PostsController < ApplicationController
  helper_method :params

  def index

  ...
```

**Note:** You can use `helper_method` in a controller to expose, or make available, a controller method in your view, but, as we'll talk aboutsoon, this isn't always a great idea.

Let's reload our `/posts` page and try that filter again. It works! Now our readers can filter posts by author.

Let's now try to filter by date.
Let's get back into our view and add the new filter to our form:

```erb
<!-- app/views/posts/index.html.erb -->

<%= form_tag("/posts", method: "get") do %>
  <%= select_tag "author", options_from_collection_for_select(Author.all, "id", "name"), include_blank: true %>
  
  <!-- new code -->
  <%= select_tag "date", options_for_select(["Today", "Old News"]), include_blank: true %>
  <%= submit_tag "Filter" %>
<% end %>
```

`:include_blank` - If set to true, an empty option will be created. If set to a string, the string will be used as the option’s content and the value will be empty.

And then let's activate that filter so the new code at the top of our view looks like this:

```erb
<!-- app/views/posts/index.html.erb -->

<% if !params[:author].blank? %>
  <% @posts = Post.where(author: params[:author]) %>
<% elsif !params[:date].blank? %>
  <% if params[:date] == "Today" %>
    <% @posts = Post.where("created_at >=?", Time.zone.today.beginning_of_day) %>
  <% else %>
    <% @posts = Post.where("created_at <?", Time.zone.today.beginning_of_day) %>
  <% end %>
<% end %>

<h1>Believe It Or Not I'm Blogging On Air</h1>

  ...
```

We did it! We added our filters. But we understand that in MVC we separate concerns and put code in the right place. Looking at our current `posts#index` view cluttered with too much *business logic*.

So it's time to do some refactoring.

We have some pretty big red flags here:
* View directly querying the database for posts *and* authors!
* View reading `params`, which we had to go out of our way to allow from the controller!
* View overriding `@posts`, which the controller is already creating, fully *doubling* our database requests!

First, let's dive back into the `posts#index` view and kill that filter logic.

```erb
<!-- app/views/posts/index.html.erb -->

<h1>Believe It Or Not I'm Blogging On Air</h1>
<div>
  <h3>Filter posts:</h3>
  <%= form_tag("/posts", method: "get") do %>
    <%= select_tag "author", options_from_collection_for_select(@authors, "id", "name"), include_blank: true %>
    <%= select_tag "date", options_for_select(["Today", "Old News"]), include_blank: true %>
    <%= submit_tag "Filter" %>
  <% end %>
</div>
<% @posts.each do |post| %>
  <div>
    <h2><%= post.title %></h2>
    <h3>by: <%= link_to post.author.name, author_path(post.author) %></h3>
    <p><%= post.description %></p>
  </div>
<% end %>
```

Don't forget to also change the `select_tag` options from `Author.all` to `@authors` because our view shouldn't be directly querying the database for that, either. A view's concern is *presentation*. The controller should give it all the data it needs.

Now let's get into our controller and repurpose our filter code so it will run there.

First, let's get rid of that `helper_method :params` line. We don't need it anymore. Reading `params` is a controller concern, so we don't need to expose it to the views.

Now let's dig into our `index` method and make some changes. First, we need to add this line to satisfy our author select control: `@authors = Author.all`. We're using the same code, just moving it to where it belongs.

```ruby
# app/controllers/posts_controller.rb

def index
  # provide a list of authors to the view for the filter control
  @authors = Author.all

  # filter the @posts list based on user input
  if !params[:author].blank?
    @posts = Post.where(author: params[:author])
  elsif !params[:date].blank?
    if params[:date] == "Today"
      @posts = Post.where("created_at >=?", Time.zone.today.beginning_of_day)
    else
      @posts = Post.where("created_at <?", Time.zone.today.beginning_of_day)
    end
  else
    # if no filters are applied, show all posts
    @posts = Post.all
  end
end
```

This is looking much better. Our view is back to only dealing with presentation logic, and our controller is providing the right data. But we're still not there yet.

If we think about separation of concerns, is the controller concerned with having deep knowledge of the database so that it can make queries directly? No. All a controller wants to do is ask a model for what it needs in the simplest way possible.

So having something that looks like this...
```ruby
@posts = Post.where("created_at >=?", Time.zone.today.beginning_of_day)
```
...isn't the best application of MVC and separation of concerns. We want the model to know things like `"created_at >=?", Time.zone.today.beginning_of_day` and the controller to just ask for something more like `from_today`.

So we need to move this into the model. Now, the question becomes: is this a *class method* on the `Post` model itself, or is it an *instance method* on a specific `post`?

Well, since we are going to be asking for multiple `post` instances from the database, we won't have an instance to begin with, so we'll need to use a class method.

Class methods are commonly used on ActiveRecord models to encapsulate this type of custom query functionality, so let's do that now.

First, we want to add one for the author filter. Let's dive into `post.rb` and get to work.

```ruby
# app/models/post.rb

def self.by_author(author_id)
  where(author: author_id)
end
```

You'll notice that it's essentially the same code that we had in the controller, but it's now properly encapsulated in the model. This way, a controller doesn't have to query the database — it just has to ask for posts `by_author`.

So let's do that. Get back in the `posts_controller`, and change the code to use our new class method:

```ruby
# app/controllers/posts_controller.rb

if !params[:author].blank?
  @posts = Post.by_author(params[:author])
elsif !params[:date].blank?

  ...
```

**Top-tip:** There are always more ways to accomplish the same thing. You may be wondering why we didn't just `find` the author and return `author.posts`. That also would work. But when we think about constructing an application in a logical way, using principles like Separation of Concerns to guide us, we can conclude that the `posts_controller` is concerned with the `Post` model, so almost everything that controller does will probably flow through that model.

Now that we have that done, we can reload our `/posts` page and make sure it's all still working.

Next, let's give the same treatment to our date filter. We want to move the database code from the controller to the model, so let's add a couple more class methods to the `Post` model:

```ruby
# app/models/posts.rb

...

def self.from_today
  where("created_at >=?", Time.zone.today.beginning_of_day)
end

def self.old_news
  where("created_at <?", Time.zone.today.beginning_of_day)
end
```

And then let's update our controller to use them. Our final `index` method should now look like this:

```ruby
# app/controllers/posts_controller.rb

def index
  # provide a list of authors to the view for the filter control
  @authors = Author.all

  # filter the @posts list based on user input
  if !params[:author].blank?
    @posts = Post.by_author(params[:author])
  elsif !params[:date].blank?
    if params[:date] == "Today"
      @posts = Post.from_today
    else
      @posts = Post.old_news
    end
  else
    # if no filters are applied, show all posts
    @posts = Post.all
  end
end
```

We've taken a look at a few different ways to create some features, but the big takeaway is this: by considering the separation of concerns, sticking to MVC, and using class methods on our models, we were ultimately able to implement those features in a clean, well-organized way.

## Routing and Nested resources
We're going to keep working on our blog application, augmenting it to filter posts by author in a more user-friendly and RESTful way.

You've encountered `REST` already, but, just to review, it stands for REpresentational State Transfer and encapsulates a way of structuring a URL so that access to specific resources is predictable and standardized.

In practice, that means that, if we type `rails s` and run our blog app, browsing to `/posts` will show us the index of all `Post` objects. And if we want to view a specific `Author`, we can guess the URL for that (as long as we know the author's `id`) by going to `/authors/:id`.

**Top-tip:** the `/:id` notation above represents a *dynamic* route segment, which we've touched on before and will be seeing more of in this reading.

Why do we care? When we added the filter button to our blog, we could filter by a certain author to see that author's posts, but the URL looked something like this:
`http://localhost:3000/posts?utf8=%E2%9C%93&author=1&date=&commit=Filter`
That's the opposite of REST.

What we'd love to end up with here is something like `/authors/1/posts` for all of an author's posts and `/authors/1/posts/5` to see an individual post by that author.

### First (bad) method
We could go this way
```ruby
# config/routes.rb

  get 'authors/:id/posts', to: 'authors#posts_index'
  get 'authors/:id/posts/:post_id', to: 'authors#post'
```

And to handle our new filtering routes, we'll need to make some changes in our `authors_controller` to actually do the work.
```ruby
  def posts_index
    @author = Author.find(params[:id])
    @posts = @author.posts
    render template: 'posts/index'
  end

  def post
    @author = Author.find(params[:id])

    # Note that because ids are unique by table we can go directly to
    # Post.find — no need for @author.posts.find...
    @post = Post.find(params[:post_id])
    render template: 'posts/show'
  end
```

**Advanced:** While a controller action would normally implicitly render a template with the same name as the method, in this case we want to leverage the templates we're already using for posts, so we call `render` explicitly with a template path. Because we're telling `render` that we're using a `template`, we don't need to include the `.html.erb` extensions. Rails figures that out for us.

If we go back to our blog and try to browse to `/authors/1/posts`, we should see the posts for that author. And then if we try `/authors/1/posts/1`, we should see that post.

**Note:** If your IDs are different and you are having trouble with the URLs, try running `rake db:reset` to reset your IDs to the defaults in the seed file.

We did it! We have much nicer URLs now. Are we done? Of course not.


If we look at our `routes.rb`, we can already see it getting messy. Instead of something nice like `resources :authors`, now we're specifying controller actions and HTTP verbs just to do a simple filter of an author's posts.

Beyond that, our DRY (Don't Repeat Yourself) and Separation of Concerns klaxons should be wailing because the code to find all posts and to find individual posts by their ID is essentially repeated in the `authors_controller`. These aren't really the concerns of the `authors_controller`, and we can tell that because we're directly rendering `Post`-related templates.

### Second (and best) method
Turns out, Rails *does* give us a way to make this a lot nicer. If we look again at our models, we see that an author `has_many :posts` and a post `belongs_to :author`. Since a post can logically be considered a *child* object to an author, it can also be considered a *nested resource* of an author for routing purposes.

Nested resources give us a way to document that parent/child relationship in our routes and, ultimately, our URLs. Let's get back into `routes.rb`, delete the two routes we just added, and recreate them as nested resources. We should end up with something like this:
```ruby
# config/routes.rb

Rails.application.routes.draw do

  resources :authors, only: [:show] do
    # nested resource for posts
    resources :posts, only: [:show, :index]
  end

  resources :posts, only: [:index, :show, :new, :create, :edit, :update]

  root 'posts#index'
end
```

Now we have the resourced `:authors` route, but by adding the `do...end` we can pass it a block of its nested routes. We can still do things to the nested resources that we do to a non-nested resource, like limit them to only certain actions. In this case, we only want to nest `:show` and `:index` under `:authors`.

Under that, we still have our regular resourced `:posts` routes because we still want to let people see all posts, create and edit posts, and so on outside of the context of an author.

Now we need to update our `posts_controller` to handle the nested resource we just set up. Notice how now we are dealing with the `posts_controller` rather than the `authors_controller`. Ultimately, the resource we're requesting is related to posts, so Separation of Concerns tells us to put that code in the `posts_controller`. And, since we already have actions to handle `:show` and `:index`, we won't be repeating ourselves like we did in the `authors_controller`.

Let's update `index` and `show` to account for the new routes:

```ruby
# app/controllers/posts_controller.rb

  def index
    if params[:author_id]
      @posts = Author.find(params[:author_id]).posts
    else
      @posts = Post.all
    end
  end

  def show
    @post = Post.find(params[:id])
  end
```

We added a conditional to the `posts#index` action to account for whether the user is trying to access the index of *all* posts (`Post.all`) or just the index of all posts *by a certain author* (`Author.find(params[:author_id]).posts`). The conditional hinges on whether there's an `:author_id` key in the `params` hash — in other words, whether the user navigated to `/authors/:id/posts` or simply `/posts`. We didn't have to create any new methods or make explicit calls to render new templates. We just added a simple check for `params[:author_id]`, and we're good to go.

Where is `params[:author_id]` coming from? Rails provides it for us through the nested route, so we don't have to worry about a collision with the `:id` parameter that `posts#show` is looking for. Rails takes the parent resource's name and appends `_id` to it for a nice, predictable way to find the parent resource's ID.

But, wait– we didn't make a single change to the `posts#show` action. What about the new `/authors/:id/posts/:id` route that we added? Remember, the point of nesting our resources is to DRY up our code. We had to create a conditional for the `posts#index` action because it renders *different* sets of posts depending on the path, `/authors/:id/posts` or `/posts`. Conversely, the `posts#show` route is going to render the *same* information — data concerning a single post — regardless of whether it is accessed via `/authors/:id/posts/:id` or `/posts/:id`.

For good measure, let's go into our `authors_controller.rb` and delete the two actions (`post` and `posts_index`) that we added above so that it looks like this:
```ruby
# app/controllers/authors_controller.rb

class AuthorsController < ApplicationController

  def show
    @author = Author.find(params[:id])
  end

end
```

**Top-tip:** Keep your application clean and easy to maintain by always removing unused code.

We've got our routes working and the `posts_controller` is handling its business, but how can we present this on the page so that someone knows how to find a link to an author's posts? Just like any other resourced route, Rails provides named helpers for our nested routes as well. And, just like most other things Rails provides, there's a predictable way to figure out what they are.

If we want to get to the `/authors` page, we know the URL helpers are `authors_path` and `authors_url`. And if we want to get to a single author (`/authors/:id`), we can use `author_path(id)`. Similarly, we have `posts_path` for `/posts` and `post_path(id)` for `/posts/:id`.

So what if we want to get to all posts nested under an author? We know the URL is `/authors/:author_id/posts`, so we can combine the two conventions and use `author_posts(author_id)`. Remember it's the singular `author` because we are getting one by `id`.

It stands to reason that a single post for an author would combine the conventions for the single author path and single post path, leaving us with `author_post(author_id, post_id)`.

Once you become accustomed to breaking it down in that way, it's pretty straightforward to know what our URL helpers will be for a nested route. However, if you're not sure, or if you just want to double-check, you can use `rake routes` on the command line to get a printout of all your named routes, like this:

```bash
      Prefix Verb  URI Pattern                             Controller#Action
  test_index GET   /test/index(.:format)                   test#index
author_posts GET   /authors/:author_id/posts(.:format)     posts#index
 author_post GET   /authors/:author_id/posts/:id(.:format) posts#show
      author GET   /authors/:id(.:format)                  authors#show
       posts GET   /posts(.:format)                        posts#index
             POST  /posts(.:format)                        posts#create
    new_post GET   /posts/new(.:format)                    posts#new
   edit_post GET   /posts/:id/edit(.:format)               posts#edit
        post GET   /posts/:id(.:format)                    posts#show
             PATCH /posts/:id(.:format)                    posts#update
             PUT   /posts/:id(.:format)                    posts#update
        root GET   /                                       posts#index
```

You can also view this in your browser anytime you type in an incorrect route or if you visit `rails/info/routes.`. If you add `_path` or `_url` to any of the names under "Prefix", you'll have the helper for that route.

**Advanced:** Using `rake routes` can be a lot easier than browsing the `routes.rb` file once a project gets to a certain size, but the output might be overwhelming. Remember that you can `grep` the output of any command to search for what you want. So in the example above, if you just wanted to search for routes related to authors, you could type `rake routes | grep authors` to get a filtered list.

Let's make it easy for our readers to look at the posts for each of our authors using these helpers. In `posts/index.html.erb`, we already show the author's name, so let's add a link to the list of the author's posts:
```erb
<!-- app/views/posts/index.html.erb -->

  ...

  <h2><%= post.title %></h2>
  
  <!-- change the name to a link -->
  <h3>by: <%= link_to post.author.name, author_posts_path(post.author) %></h3>
  <p><%= post.description %></p>

  ...
```
Let's reload `/posts` and click on an author name. We should be taken to `/author/id/posts`. Great! Now our URLs properly reflect the relationship of our resources and read almost like an English sentence: `author/1/posts` = "author number one's posts."

Sometimes people get confused by path helpers and what they take as arguments. For example, why does `posts_path` not take an argument, but `post_path(@post)` does? `posts_path` refers to all of the posts, so we're not talking about a specific post (no specific ID). When we're looking at the `post_path`, we're referring to a specific post, and the Rails helper needs to know *which* post so that it can generate the proper url, e.g., `posts/1` as opposed to `posts/2`.

### Don't nest on 3 levels
You can nest resources more than one level deep, but that is generally a bad idea.

Imagine if we also had comments in this blog. This would be a perfectly fine use of nesting:

```ruby
resources :posts do
  resources :comments
end
```

We could then link to a post's comments with `post_comments_path` or `/posts/1/comments`. That makes a lot of sense.

But if we then tried to add to our already nested `posts` resource...

```ruby
resources :authors do
  resources :posts do
    resources :comments
  end
end
```

Now we're getting into messy territory. Our `comments_path` helper is now `author_post_comments_path`, our URL is `/authors/1/posts/1/comments`, and we have to handle for that filtering in our controller.

But if we lean on our old friend Separation of Concerns, we can conclude that a post's comments are not the concern of an author and therefore don't belong nested two levels deep under the `:authors` resource.

In addition, the reason to put the ID of the resource in the URL is so that we have access to it in the controller. If we know we have the post with an ID of `1`, we can use our Active Record relationships to call:

```ruby
  @post = Post.find(params[:id])
  @post.author # This will tell us who the author of the post was! We don't need this information in the URL
```

Nesting resources is a powerful tool that helps you keep your routes neat and tidy and is better than dynamic route segments for representing parent/child relationships in your system. However, as a general rule, you should only nest resources one level deep and ensure that you are considering Separation of Concerns in your routing.

### The Flash
The flash is a special part of the session which is cleared with each request. This means that values stored there will only be available in the next request, which is useful for passing error messages etc.

It is accessed in much the same way as the session, as a hash (it's a FlashHash instance).

Let's use the act of logging out as an example. The controller can send a message which will be displayed to the user on the next request:
```ruby
class LoginsController < ApplicationController
  def destroy
    session[:current_user_id] = nil
    flash[:notice] = "You have successfully logged out."
    redirect_to root_url
  end
end
```
Note that it is also possible to assign a flash message as part of the redirection. You can assign :notice, :alert or the general purpose :flash:
```ruby
redirect_to root_url, notice: "You have successfully logged out."
redirect_to root_url, alert: "You're stuck here!"
redirect_to root_url, flash: { referral_code: 1234 }
```
The destroy action redirects to the application's `root_url`, where the message will be displayed. Note that it's entirely up to the next action to decide what, if anything, it will do with what the previous action put in the flash. It's conventional to display any error alerts or notices from the flash in the application's layout:
```erb
<html>
  <!-- <head/> -->
  <body>
    <% flash.each do |name, msg| -%>
      <%= content_tag :div, msg, class: name %>
    <% end -%>
 
    <!-- more content -->
  </body>
</html>
```
This way, if an action sets a notice or an alert message, the layout will display it automatically.

You can pass anything that the session can store; you're not limited to notices and alerts:
```erb
<% if flash[:just_signed_up] %>
  <p class="welcome">Welcome to our site!</p>
<% end %>
```
If you want a flash value to be carried over to another request, use the keep method:
```ruby
class MainController < ApplicationController
  # Let's say this action corresponds to root_url, but you want
  # all requests here to be redirected to UsersController#index.
  # If an action sets the flash and redirects here, the values
  # would normally be lost when another redirect happens, but you
  # can use 'keep' to make it persist for another request.
  def index
    # Will persist all flash values.
    flash.keep
 
    # You can also use a key to keep only some kind of value.
    # flash.keep(:notice)
    redirect_to users_url
  end
end
```
By default, adding values to the flash will make them available to the next request, but sometimes you may want to access those values in the same request. For example, if the create action fails to save a resource and you render the new template directly, that's not going to result in a new request, but you may still want to display a message using the flash. To do this, you can use flash.now in the same way you use the normal flash:
```ruby
class ClientsController < ApplicationController
  def create
    @client = Client.new(params[:client])
    if @client.save
      # ...
    else
      flash.now[:error] = "Could not save client"
      render action: "new"
    end
  end
end
```

### Create and Nested Resources
Continuing with our blog application, we're going to extend our nested resources to allow for creating and modifying blog posts by author.

The first thing we want to do is to create a new post that is automatically linked to an `Author`. We could set up a select box on the post page and make the author choose. However, if we're already on the author's page, we know who the author is, so why not do it without forcing the user to choose?

We already used nested resources to view posts by author, so now let's look at nested resources to create posts by author. As usual, we want to start with the route. We want to add `:new` to our nested `:posts` resource:

```ruby
# config/routes.rb

resources :authors, only: [:show, :index] do
  resources :posts, only: [:show, :index, :new]
end
```

This gives us access to `/authors/:id/posts/new`, and a `new_author_post_path` helper.

**Top-tip:** Remember to run `rake routes` if you're unsure of the URL helper name.

We have the route, so now we need to update our `posts_controller#new` action to handle the `:author_id` parameter.

```ruby
# controllers/posts_controller.rb

def new
  @post = Post.new(author_id: params[:author_id])
end
```

Notice that we're passing the `params[:author_id]` into `Post.new()`. We want to make sure that, if we capture an `author_id` through a nested route, we keep track of it and assign the post to that author. We'll actually be carrying this `id` with us for the next few steps, babysitting it through the server request/response cycle.

Now let's get into our author `show` template and add a link to the nested new post page for that author.

```erb
<!-- authors/show.html.erb -->

<h1><%= @author.name %></h1>

<%= link_to "New Post", new_author_post_path(@author) %>

<p>Posts:</p>
<% @author.posts.each do |post| %>
  <div><%= post.title %></div>
<% end %>
```

Something seems off. Where's our author? Looks like we didn't do a great job babysitting that `author_id`. We set it up in the `new` action, but it never made it to the view so that it could get submitted back to the server. Let's fix that. Open up the post form partial and add a hidden field for the `:author_id`.

```erb
<!-- posts/_form.html.erb -->

<%= form_for(@post) do |f| %>
  <label>Post title:</label><br>

  <%= f.hidden_field :author_id %>

  <%= f.text_field :title %><br>
  <label>Post Description</label><br>
  <%= f.text_area :description %><br>
  <%= f.submit %>
<% end %>
```

If we reload the new post page for the author and inspect the source, we should see something like this:

```html
<input type="hidden" value="1" name="post[author_id]" id="post_author_id">
```

Great. That part's working, but we need to carry that `author_id` with us even further.

Remember [Strong Parameters](http://guides.rubyonrails.org/action_controller_overview.html#strong-parameters)? We need to update our `posts_controller` to accept `:author_id` as a parameter for a post. So let's get in there and modify our `post_params` method.

```ruby
# controllers/posts_controller.rb

...

private

def post_params
  params.require(:post).permit(:title, :description, :author_id)
end
```

Now we know the `author_id` will be allowed for mass-assignment in the `create` action.

Let's try it out. Go to an author's new post page, and make a post. We should see the author's name in the byline now!

Why didn't we have to make a nested resource route for `:create` in addition to `:new`? 

The `form_for(@post)` helper in `posts/_form.html.erb` will automatically route to `POST posts_controller#create` for a new `Post`. By carrying the `author_id` as we did and allowing it through strong parameters, the existing `create` route and action can be used without needing to do anything else.

### Update and Nested Resource
We can use the same technique to allow us to directly edit an author's posts.
First, we allow the `:edit` action in the nested route:

```ruby
# config/routes.rb

resources :authors, only: [:show, :index] do
  resources :posts, only: [:show, :index, :new, :edit]
end
```

We don't have to change any views because `new` and `edit` both use the same `_form` partial that already has the `author_id`.

Now we need to update our post `show` view to give us the new nested link to edit the post for the author.

```erb
<!-- posts/show.html.erb -->

<h1><%= @post.title %></h1>
<p>by <%= link_to @post.author.name, author_path(@post.author) if @post.author %> (<%= link_to "Edit Post", edit_author_post_path(@post.author, @post) if @post.author %>)</p>
<p><%= @post.description %> </p>
```

And if we try it out, everything should work just fine. Reload the page, click the edit link, and edit the post. Pretty easy, right? We didn't even have to change the controller this time. What's the catch?

The catch is that we've opened ourselves up to a couple of potential bugs or, worse, opportunities for our more playful users to make a mess of our data. Let's work backward, starting with our recent changes to `edit`.

If you go back to your author post edit page, you'll see a URL similar to `http://localhost:3000/authors/1/posts/1/edit`. This tells us that we are editing the `Post` with `id: 1` by the `Author` with `id: 1`. But what if we change that `author_id` in the URL? Try browsing to `http://localhost:3000/authors/123456/posts/1/edit`, and see what happens.

We end up on the same page! But post `1` belongs to author `1` — not author `123456`. In fact, there *is no* author `123456` in the system. How is this happening?

Remember how we didn't have to change the controller when we added the nested resource route for `:edit`? Well, this is the price we pay for taking shortcuts. What we should do is check to make sure that 1) the `author_id` is valid and 2) the post matches the author. So let's fix that now.

```ruby
# controllers/posts_controller.rb

def edit
  if params[:author_id]
    author = Author.find_by(id: params[:author_id])
    if author.nil?
      redirect_to authors_path, alert: "Author not found."
    else
      @post = author.posts.find_by(id: params[:id])
      redirect_to author_posts_path(author), alert: "Post not found." if @post.nil?
    end
  else
    @post = Post.find(params[:id])
  end
end
```

Here we're looking for the existence of `params[:author_id]`, which we know would come from our nested route. If it's there, we want to make sure that we find a valid author. If we can't, we redirect them to the `authors_path` with a `flash[:alert]`.

If we do find the author, we next want to find the post by `params[:id]`, but, instead of directly looking for `Post.find()`, we need to filter the query through our `author.posts` collection to make sure we find it in that author's posts. It may be a valid post `id`, but it might not belong to that author, which makes this an invalid request.

Now if we go back and try our invalid URL (`http://localhost:3000/authors/123456/posts/1/edit`), we should be redirected back to where we belong.

**Top-tip:** One of the downsides of RESTful URL schemes is that curious users can edit the URLs to try to explore the system further. This is how we discovered [all the hidden Netflix genres](http://mashable.com/2016/01/11/netflix-search-codes/#LM6QcfeksZqG). However, this could also lead to security holes in your system, allowing users to potentially mismatch id parameters and wreak havoc in your database, so always guard against that by doing what we've done above.

While we're at it, we should fix up our `new` action to ensure that we're creating a new post for a valid author. Let's make it look like this:

```ruby
# controllers/posts_controller.rb

def new
  if params[:author_id] && !Author.exists?(params[:author_id])
    redirect_to authors_path, alert: "Author not found."
  else
    @post = Post.new(author_id: params[:author_id])
  end
end
```

Here we check for `params[:author_id]` and then for `Author.exists?` to see if the author is real.

Why aren't we doing a `find_by` and getting the author instance? Because we don't need a whole author instance for `Post.new`; we just need the `author_id`. And we don't need to check against the `posts` of the author because we're just creating a new one. So we use `exists?` to quickly check the database in the most efficient way.

But what if `params[:author_id]` is `nil` in the example above? If we just did `Post.new` without the `(author_id: params[:author_id])` argument, the `author_id` attribute of the new `Post` would be initialized as `nil` anyway. So we don't have to do anything special to handle it. It works for us if there is or isn't an `author_id` present.

Which brings us to the last thing we have to do.

When someone creates a new post via our nested route, we automatically assign an author, and everything works great. But what about when they create a new post from the regular old `new_post_path`?

We could just eliminate that route and only allow post creation through the nested resource. That might be a valid choice in some applications. But we've decided we want to be able to select an author at the time of posting if we haven't used the nested route.

Since we're already set up to handle `author_id` on the controller, all we have to do is augment our `posts/_form.html.erb` partial to present a list of authors when none is present.

```erb
<!-- posts/_form.html.erb -->

<%= form_for(@post) do |f| %>
  <label>Post title:</label><br>
  <% if @post.author.nil? %>
    <%= f.select :author_id, options_from_collection_for_select(Author.all, :id, :name) %><br>
  <% end %>
  <%= f.hidden_field :author_id %>

...
```

That gives us a select control if we don't have an author, but we have a problem. We can only have one `:author_id` field. We could put that `hidden_field` in an `else`, which would work, but then we would have a whole bunch of logic cluttering up our view. So let's dump it in our `posts_helper` and clean up that form.

```ruby
# helpers/posts_helper.rb

module PostsHelper
  def author_id_field(post)
    if post.author.nil?
      select_tag "post[author_id]", options_from_collection_for_select(Author.all, :id, :name)
    else
      hidden_field_tag "post[author_id]", post.author_id
    end
  end
end
```

And back in our form partial:

```erb
<!-- posts_form.html.erb -->

<%= form_for(@post) do |f| %>
  <%= author_id_field(@post) %>
  <br>
  <label>Post title:</label><br>
  <%= f.text_field :title %><br>

...
```

Now we should have a selector when we browse to `/posts/new` and a hidden `author_id` field when we browse to `/authors/1/posts/new`.

### Namespaced routes
We're going to explore different ways of routing things in our blog application to help us organize and group certain routes and controllers more logically.

We decide that we want to keep track of some basic blog statistics, such as how many posts and authors we have. We start by creating a `stats_controller.rb` with an `index` action and corresponding view.

We can't actually browse to it yet because we need to set up a route. Let's add it to `routes.rb`:
```ruby
# config/routes.rb

get '/stats', to: 'stats#index'
```

Easy enough, but, after thinking about it, `/stats` isn't something we want to just hang off the root of our blog URL for anyone to see. It's really just for blog admins, and we want to set up a URL scheme to segregate admin things into their own logical space.
We modify our route:
```ruby
# config/routes.rb

get '/admin/stats', to: 'stats#index'
```

Now we can browse to `/admin/stats` for the stats page, and we can no longer go straight to `/stats`.

#### Scoping Routes
Over time, we might decide to add more admin functions, grouping them all together like we did above, until eventually our `routes.rb` looks something like this:

```ruby
# config/routes.rb

...

get '/admin/stats', to: 'stats#index'
get '/admin/authors/new', to: 'authors#new'
get '/admin/authors/delete', to: 'authors#delete'
get '/admin/authors/create', to: 'authors#create'
get '/admin/comments/moderate', to: 'comments#moderate'
```

As you can see, even with only a few more actions in our `admin` section, our routes are getting ugly. Not to mention we're repeating ourselves a lot by typing in `/admin` on all these routes. Yes, even routes should be DRY!

What we need is a way to group all these under `/admin` without typing `/admin` all the time. That's where `scope` comes in.
In routing, `scope` allows us to prefix a block of routes under one grouping. So let's change our stats route:
```ruby
# config\routes.rb

scope '/admin' do
  resources :stats, only: [:index]
end
```

Now we can reload `/admin/stats`, and it still works. Notice our new route is resourced. Now that we don't have to manually prefix `/admin`, we can go back to using resourced routes within the `/admin` scope.

If you run `rake routes`, you'll see that the new `/admin/stats` helpers are `stats_path` and `stats_url`.

#### Scoping with Modules
Scoping works nicely to group our URLs together logically, but what happens when we have a bunch of controllers that are handling admin functions? As the application grows, it's going to be harder and harder to keep track of which controllers are for regular blog functions and which are for admin functions.

We want to group all our admin controllers logically to make it easier to maintain and add to the app, so let's add an `/admin` directory under `/controllers` where all the admin controllers will go:

`mkdir app/controllers/admin`

Now let's move our `stats_controller.rb` into the `/admin` folder.

When you create a new folder under `/controllers`, Rails will automatically pick that up as a `module` and expect you to namespace the controller accordingly. We need to modify our `admin/stats_controller.rb` to look like this:

```ruby
# controllers/admin/stats_controller.rb

class Admin::StatsController < ApplicationController
  def index

    ...

  end
end
```

Now that we have our controller in a module, Rails will expect the views to match. Let's create a new directory at `/app/views/admin/stats` and move our `stats/index.html.erb` into it, so we'll wind up with `/app/views/admin/stats/index.html.erb`.

**Top-tip:** The `views` folder for a controller module (in this case `/admin`) expects a subfolder structure that matches the names of the controllers (in this case `/admin/stats`).

If we try to reload `/admin/stats` now, we will get an error because we need to tell our routes about our new module.

```ruby
# config/routes.rb

scope '/admin', module: 'admin' do
  resources :stats, only: [:index]
end
```

We're telling `scope` that we want to use `/admin` as a URL prefix, and we're also letting Rails know that all of the included routes will be handled by controllers in the `admin` module.

If we reload `/admin/stats`, everything should work just like it did, but now we are logically organizing our controllers.

#### Namespace
Right now, our route is scoped as `scope '/admin', module: 'admin'`, which is fine but perhaps a bit less DRY than we'd like. Fortunately, Rails gives us a shortcut here. When we want to route with a module *and* use that module's name as the URL prefix, we can use the `namespace` method instead of `scope, module`.

```ruby
# config/routes.rb

namespace :admin do
  resources :stats, only: [:index]
end
```

If we reload `/admin/stats`, everything still works, but we've simplified the declaration of the routes. The `namespace` method makes the assumption that the path prefix and module name match, saving us some typing.

**Top-tip:** There is one important difference between `scope '/admin', module: 'admin'` and `namespace :admin`, and it's in the URL helpers. Remember above that using `scope` gave us a `stats_path` helper. But now that we are using `namespace`, run `rake routes` again. You'll see that the helper is now prefixed with `admin_`, so `stats_path` becomes `admin_stats_path`. If you switch from `scope` to `namespace`, take care to update any URL helpers you have in use!

## Rails Authentication
### Cookies and Sessions
Cookies are a way for an HTTP server to ask the user's browser to store a little bit of data for it, and then get that data back from the browser later. They are fundamental to the operation of nearly every contemporary website.

Primarily, cookies are used for log in. They provide a way for us to verify who a user is once, and then remember it for their entire session. Without cookies, you would have to provide your username and password on every single request to the server.

Cookies may also be used to store other information about a user, such as what's in their shopping cart, or what ads you've shown them during their visit.

In this document, we'll cover what cookies are, how they fit into the HTTP response flow, and how you can access them within your Rails application.

#### Shopping
Let's say we want to build an e-commerce site. Users can come to the site, add some items to their shopping cart, and then pay for all of them at the end. Simple.

Where does the user's shopping cart live?

We could create a `Cart` model, which [`has_and_belongs_to_many`][habtm] `Item`s. But that leaves a fundamental problem: how do we know what cart to load when we get a request? When one user requests to see their cart, how can I tell them apart from another user, and make sure they're seeing the right cart?

The flow looks like this:

  * When a user adds something to their cart, their browser will make a `POST` request to `/cart`.
  * Later, to see the content of their cart, they'll send a `GET` request to `/cart`. 



Remember what's included in an HTTP request:
  * an [HTTP verb][rfc_http_methods], like `GET`, `PUT`, or `POST`
  * a path
  * various headers

HTTP servers are typically stateless. They receive requests, process them, return data, then forget about them.

For example, `GET` requests usually encode this information in the path. When you write a route matching `/items/:item_id`, you are telling Rails to pull the value `id` from the request path and save it in `params[:id]`. In your `items_controller`, you'll probably have a method that looks something like:

```ruby
def show
  @item = Item.find(params[:item_id])
end
```

Which loads the row for that item from the database and returns it as an ActiveRecord model object, which your `show.html.erb` then renders.

If we want to be able to retrieve the current cart, we need to have its `id` somewhere in the HTTP request. Specifically, it must be in the path or the headers.

If we want to be able to retrieve the current cart, we need to have its id somewhere in the HTTP request. Specifically, it must be in the path or the headers.

It would be possible, though quite convoluted, to store this information in the path. This would have strange effects: since the path is shown in the browser's URL bar, a user who copies a URL and sends it to a friend ("check out this neat skirt!") would also be copying their shopping cart ID. Upon loading the page, the friend would see what's in the user's cart. Since a cart is owned by a particular user, and may contain private information, this is probably not what we want.

Cookies allow us to store this information in the only other place available to us: `HTTP headers`.

#### What's a cookie, anyway?
Let's see what [the spec][rfc_cookies] has to say:

      This section outlines a way for an origin server to send state
      information to a user agent and for the user agent to return the
      state information to the origin server.

      To store state, the origin server includes a Set-Cookie header in an
      HTTP response.  In subsequent requests, the user agent returns a
      Cookie request header to the origin server.  The Cookie header
      contains cookies the user agent received in previous Set-Cookie
      headers.  The origin server is free to ignore the Cookie header or
      use its contents for an application-defined purpose.

The description is quite technical, so let's look at their example:

      == Server -> User Agent ==
      Set-Cookie: SID=31d4d96e407aad42

      == User Agent -> Server ==
      Cookie: SID=31d4d96e407aad42

In this example, the server is an HTTP server, and the User Agent is a browser. The server responds to a request with the `Set-Cookie` header. This header sets the value of the `SID` cookie to `31d4d96e407aad42`.

Next, when the user visits another page on the same server, the browser sends the cookie back to the server, including the `Cookie: SID=31d4d96e407aad42` header in its request.

Cookies are stored in the browser. The browser doesn't care about what's in the cookies you set. It just stores the data and sends it along on future requests to your server. You can think of them as a hash—and indeed, as we'll see later, Rails exposes cookies with a method that behaves much like a hash.

So how would we use a cookie to store a reference to the user's shopping cart? Let's say that we create a cart the first time a user adds something to their cart. Then, in the response, we might include the header,

      == Server -> User Agent ==
      Set-Cookie: cart_id=273

Only with the `cart.id` of the cart we just saved.

When the user comes back to our site, their browser will include the cookie in their reply:

      == User Agent -> Server ==
      Cookie: cart_id=273

We can look at this HTTP header, get the `cart_id` from it, and look it up using the `ActiveRecord` find method we know and love.

#### Security concerns
Cookies are stored as plain text in a user's browser. Therefore, the user can see what's in them, and they can set them to anything they want.

If you open the developer console in your browser, you can see the cookies set by the current site. In Chrome's console, you can find this under `Resources > Cookies`. You can delete any cookie you like. For example, if you delete your `user_session` cookie on `github.com`, you will find that you've been logged out.

You can also edit cookies, for example with [this extension][edit_this_cookie].

This presents a problem for us. If users can edit their `cart_id` cookie, then they can see other users' shopping carts.

Fortunately, Rails has a solution to this. When you set cookies in Rails, you usually don't manipulate the HTTP headers directly. Instead, you use the `session` method. The `session` method is available anywhere in the Rails response cycle, and it behaves like a hash:

```ruby
  # set cart_id
  session[:cart_id] = @cart.id

  # load the cart referenced in the session
  @cart = session[:cart_id]
```

You can store any simple Ruby object in the session. In fact, we don't need a `Cart` model at all—we can just store a list of items in the session!

Rails manages all session data in a single cookie, named `_YOUR_RAILS_APP_NAME_session`. It *serializes* all the key/value pairs you set with `session`, converting them from a Ruby object into a big string. Whenever you set a `key` with the `session` method, Rails updates the value of its session cookie to this big string.

When you set cookies this way, Rails signs them to prevent users from tampering with them. Your Rails server has a key, configured in `config/secrets.yml`.

```
development:
  secret_key_base: kaleisgreat  # probably not the most secure key ever
```

Somewhere else, Rails has a method, let's call it `sign`, which takes a `message` and a `key` and returns a `signature`, which is just a string:

```ruby
# sign(message: string, key: string) -> signature: string
def sign(message, key):
  # cryptographic magic here
  return signature
end
```

It's guaranteed that given the same message and key, sign will produce output. Also, without the key, it is practically impossible to know what `sign` would return for a given message. That is, signatures can't be forged.

Rails creates a signature for every cookie it sets, and appends the signature to the cookie.

When it receives a cookie, Rails verifies that the signature matches the content (that is, that `sign(cookie_body, secret_key_base) == cookie_signature`).

This prevents cookie tampering. If a user tries to edit their cookie and change the `cart_id`, the signature won't match, and Rails will silently ignore the cookie and set a new one.

Cryptography is a deep rabbit hole. At this point, you don't need to worry about the specifics of how cryptography works, just that Rails and other frameworks use it to ensure that session data which is set on the server can't be edited by users.

#### Tying it all together
In our `items_controller.rb`, we might have an `add_to_cart` method, which is called when the user adds something to their cart. It might work something like this:

```ruby
# Routed from POST /items/:id/add_to_cart
def add_to_cart
  # Get the item from the path
  @item = Item.find(params[:id])
  
  # Load the cart from the session, or create a new empty cart.
  cart = session[:cart] || []
  cart << @item.id

  # Save the cart in the session.
  session[:cart] = cart
end
```

That's it! It's common to wrap this up in a helper method:

```ruby
class ApplicationController < ActionController::Base
  helper_method :current_cart
  
  def current_cart
    session[:cart] ||= []
  end
end
```

So now our controller looks like this:

```ruby
# Routed from POST /items/:id/add_to_cart
def add_to_cart
  # Get the item from the path
  @item = Item.find(params[:id])
  
  # Load the cart from the session, or create a new empty cart.
  current_cart << @item.id
end
```

This way, we can use `current_cart` in our views and layouts too. For example, we may want to show the user how many items are in their cart as part of the layout.

Cookies are foundational for the modern web.

Most sites use cookies, either to let their users log in, to keep track of their shopping carts, or record other ephemeral session data. Almost nobody thinks these are bad uses of cookies: nobody really believes that you should have to type in your username and password on every page, or that your shopping cart should clear if you reload the page.

But cookies just let you store data in a user's browser, so by nature, they can be used for more controversial endeavors. 

For example, Google AdWords sets a cookie and uses that cookie to track what ads you've seen and which ones you've clicked on. The tracking information helps AdWords decide what ads to show you.

This is why, if you click on an ad, you may find that the ad follows you around the Internet. It turns out that this behavior is as effective as it is annoying: people are far more likely to buy things from ads that they've clicked on once.

This use of cookies worries people and the EU [has created legislation around the use of cookies][eu_law].

Cookies, like any technology, are a tool. In the rest of this unit, we're going to be using them to let users log in. Whether you later want to use them in such a way that the EU passes another law is up to you.

## Resources

  * [HTTP RFC Section 9 — Methods][rfc_http_methods]
  * [RFC 6265 — HTTP State Management Mechanism (the cookie spec)][rfc_cookies]
  * [Rails – Accessing the Session][rails_session]
  * [Has and belongs to many][habtm]
  * [EU Cookie Directive][eu_law]

[rfc_http_methods]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html "HTTP RFC 9 — Method Definitions"
[rfc_cookies]: http://tools.ietf.org/html/rfc6265 "HTTP State Management Mechanism"
[edit_this_cookie]: https://chrome.google.com/webstore/detail/editthiscookie/fngmhnnpilhplaeedifhccceomclgfbg?hl=en
[rails_session]: http://guides.rubyonrails.org/action_controller_overview.html#accessing-the-session
[habtm]: http://guides.rubyonrails.org/association_basics.html#the-has-and-belongs-to-many-association
[eu_law]: https://en.wikipedia.org/wiki/HTTP_cookie#EU_cookie_directive

## Login Required
Sometimes you want to require that a user is logged in to access a route. Here's how.

Let's say we have a `DocumentsController`. Its `show` method looks like this:
```ruby
def show
  @document = Document.find(params[:id])
end
```
Now let's add a new requirement: documents should only be shown to users when they're logged in. From a technical perspective, what does it actually mean for a user to log in?  As we saw in the last section, when a user logs in all we are doing is using cookies to add their `:user_id` to their `session`.

The first thing you might do is to just add some code into `DocumentsController#show`:
```ruby
def show
  return head(:forbidden) unless session.include? :user_id
  @document = Document.find(params[:id])
end
```

The first line is a return guard. Unless the session includes `:user_id`, we return an error. `head(:forbidden)` is a controller method that returns the specified HTTP status code—in this case, if a user isn't logged in, we return `403 Forbidden`.

We're using the phrase `unless session.include?` rather than `unless session[:user_id]`, because a user_id of 0 is still a valid user id, but would evaluate falsey, and reject the request.

This code works fine, so you use it in a few places. Now your DocumentsController looks like this:
```ruby
class DocumentsController < ApplicationController
  def show
    return head(:forbidden) unless session.include? :user_id
    @document = Document.find(params[:id])
  end

  def index
    return head(:forbidden) unless session.include? :user_id
  end

  def create
    return head(:forbidden) unless session.include? :user_id
    @document = Document.create(author_id: user_id)
  end

  def update
    return head(:forbidden) unless session.include? :user_id
    @document = Document.find(params[:id])
    # code to update a document
  end    
end
```

That doesn't look so DRY. I really wish there were a way to ask Rails to run a check before any controller action.

Fortunately, Rails gives us a solution: [`before_action`](http://guides.rubyonrails.org/action_controller_overview.html#filters). We can refactor our code like so:

```ruby
class DocumentsController < ApplicationController
  before_action :require_login
  
  def show
    @document = Document.find(params[:id])
  end

  def index
  end

  def create
    @document = Document.create(author_id: user_id)
  end

  private

  def require_login
    return head(:forbidden) unless session.include? :user_id    
  end
end
```

Let's look at the code we've added:
```ruby
before_action :require_login
```

This is a call to the ActionController class method `before_action`. `before_action` registers a filter. A filter is a method which runs before, after, or around, a controller's action. In this case, the filter runs before all DocumentsController's actions, and kicks requests out with `403 Forbidden` unless they're logged in.

What if we wanted to let anyone see a list of documents, but keep the `before_action` filter for other `DocumentsController` methods? We could do this:
```ruby
class DocumentsController < ApplicationController
  before_action :require_login
  skip_before_action :require_login, only: [:index]

  # ...
end
```

This class method, tells Rails to skip the `require_login` filter only on the `index` action.
```ruby
skip_before_action :require_login, only: [:index]
```

### Sessions Controller
We've covered how cookies can be used to store data in a user's browser. One of the most common uses of cookies is for login. In this lesson, we'll cover how to use the Rails session to log users in.

Nearly every website in the world uses what I am going to call the "wristband" pattern. A lot of nightclubs use this pattern as well. You arrive at the club. The bouncer checks your ID. They put a wristband on your wrist (or stamp your hand). They let you into the club. If you leave and come back, the bouncer doesn't look at your ID, they just look for your wristband. If you buy a drink, the bartender doesn't need to see your ID, since your wristband proves you're old enough to buy alcohol.

You arrive at [gmail.com](http://mail.google.com). You submit your username and password. Google's servers check to see if your credentials are correct. If they are, Google's servers issue a cookie to your browser. If you visit another page on gmail, or anywhere on google.com for that matter, your browser will show the cookie to the server. The server verifies this cookie, and lets you load your inbox.

Let's look at what the simplest possible login system might look like in Rails.
The flow will look like this:
   * The user GETs `/login`
   * The user enters their username. There is no password.
   * The user submits the form, POSTing to `/login`.
   * In the create action of the `SessionsController` we set a cookie on the user's browser by writing their username into the session hash.
   * Thereafter, the user is logged in. `session[:username]` will hold their username.

Let's write a `SessionsController` to handle these routes. This controller has two actions, `new` and `create`, which we'll map in `routes.rb` to `get` and `post` on `/login`.

Typically, your `create` method would look up a user in the database, verify their login credentials, and then store the authenticated user's id in the session. We're not going to do any of that right now. Our sessions controller is just going to trust that you are who you say you are.

```ruby
class SessionsController < ApplicationController
	def new
		# nothing to do here!
	end

	def create
		session[:username] = params[:username]
		redirect_to '/'
	end
end
```

There's no way for the server to log you out right now. To log yourself out, you'll have to delete the cookie from your browser.

We'll make a very small login form for `new.html.erb`,
```html
<form method='post'>
  <input name='username'>
  <input type='submit' value='login'>
</form>
```

Ordinarily, we would use `form_for @user`, but in this example, we don't have a user model at all! When the user submits the form, they'll be logged in!

The log out flow is even simpler. We add a `SessionsController#destroy` method, which will clear the username out of the session.
```ruby
def destroy
  session.delete :username
end
```
The most common way to route this action is to `post '/logout'`. This means that our logout link will actually be a submit button that we style to look like a link.

It's tempting, but don't attach this to a `get` route. HTTP specifies that `get` routes don't change anything—logging out is definitely changing something. You don't actually want someone to be able to put a link to `http://www.yoursite.com/logout` in an email or message board post. It's not a security flaw, but it's pretty annoying to be logged out because of mailing list hijinks.

At its base, login is very simple: the user provides you with credentials in a POST, you verify those credentials and set a token in the `session`. In this example, our token was literally the username the user typed. In a more complex app, it would most likely be their user id.

Read more on [Rails Tutorial Chapter 8 — Log in, log out](https://www.railstutorial.org/book/basic_login)

### Using has_secure password
It's quite difficult to manage passwords securely. About once a month, there is another big hack in the news, and all the passwords and credit cards from some poor site show up on BitTorrent.

Rails provides us with tools to store passwords relatively securely so that when hackers break into your servers they don't gain access to users' actual passwords.

Let's imagine a `SessionsController#create` method that does very simple authentication. It goes like this:

```ruby
def create
  @user = User.find_by(username: params[:username])
  return head(:forbidden) unless params[:password] == @user.password
  session[:user_id] = @user.id
end
```

We load the user row, check to see if the provided password is equal to the password stored in the database, and, if it is, set `user_id` in the `session`.

This is tremendously insecure because you then have to store all your users' passwords in the database, unencrypted.

NEVER DO THIS!!!!!

Even if you don't care about the security of your site, people have a strong tendency to reuse passwords. That means that the inevitable security breach of your site will leak passwords which some users also use for Gmail. Your `users` table probably has an `email` column. This means that, if I'm a hacker, getting access to your database has given me the Internet equivalent of the house keys and home address for some (probably surprisingly large) percentage of your users.

So how do we store passwords if we can't store passwords?
We store their hashes. A *hash* is a number computed by feeding a string to a *hash function*. Hash functions have the property that they will always produce the same number given the same input. You could write one yourself. Here's a very simple one:
```ruby
# dumb_hash(input: string) -> number
def dumb_hash(input)
  input.bytes.reduce(:+)
end
```

This `dumb_hash` function just finds the sum of the bytes that comprise the string. It is a hash function since it satisfies the criterion that the same string always produces the same result. We could imagine using this function to avoid storing passwords in the database. Our `User` model and `SessionsController` might look like this:
```ruby
# app/models/user.rb
class User < ActiveRecord::Base
  def password=(new_password)
    self.password_digest = dumb_hash(new_password)
  end
  
  def authenticate(password)
    return nil unless dumb_hash(password) == password_digest
    self
  end
  
  private
  
  def dumb_hash(input)
    input.bytes.reduce(:+)
  end
end

# app/controllers/sessions_controller.rb
class SessionsController < ApplicationController
  def create
    user = User.find_by(username: params[:username])
    authenticated = user.try(:authenticate, params[:password])
    return head(:forbidden) unless authenticated
    @user = user
    session[:user_id] = @user.id
  end
end
```
**Note:** [`try`](http://api.rubyonrails.org/classes/Object.html#method-i-try) is an ActiveSupport method. `object.try(:some_method)` means 
`if object != nil then object.some_method else nil end`.

In this world, we have saved the password hashes in a `password_digest` column in the database. We are not storing the passwords themselves.

You can set a user's password by saying `user.password = *new_password*`. Presumably, our `UsersController` would do this, but we're not worrying about it for the moment.

`dumb_hash` is, as its name suggests, a pretty dumb hash function to use for this purpose. It's a poor choice because similar strings hash to similar values. If my password was 'Joshua', you could log in as me by entering the password 'Jnshub'. Since 'n' is one less than 'o' and 'b' is one more than 'a', the output of `dumb_hash` would be the same.

This is known as a *collision*. Collisions are inevitable when you're writing a hash function, since hash functions usually produce either a 32-bit or 64-bit number, and the space of all possible strings is much larger than either `2**32` or `2**64`.

Fortunately, smart people who have thought about this a lot have written a lot of different hash functions which are well-suited to different purposes. And nearly all hash functions are designed with the quality that strings which are similar but not the same hash to significantly different values.

Ruby internally uses [MurmurHash](https://en.wikipedia.org/wiki/MurmurHash), which produces better results for this:

    > 'Joshua'.hash
     => -3766180385262328513

    > 'Jnshub'.hash
     => 827642026211689321

But Murmur still isn't ideal, because while it does not produce collisions so readily, it is still not difficult to produce them if that's what you're trying to do.

Instead, Rails uses BCrypt. BCrypt is designed with these properties in mind:
  1. BCrypt hashes similar strings to very different values.
  2. It is a *cryptographic hash*. That means that, if you have an output in mind, finding a string which produces that output is designed to be "very difficult." "Very difficult" means "even if Google put all their computers on it, they couldn't do it."
  3. BCrypt is designed to be slow — it is intentionally computationally expensive.

The last two features make BCrypt a particularly good choice for passwords. (2) means that, even if an attacker gets your database of hashed passwords, it is not easy for them to turn a hash back into its original string. (3) means that, even if an attacker has a dictionary of common passwords to check against, it will still take them a considerable amount of time to check for your password against that list.

But what if our attackers have done their homework?

Say I'm a hacker. I know I'm going to break into a bunch of sites and get their password databases. I want to make that worth my while.

Before I do all this breaking and entering, I'm going to find the ten million most common passwords and hash them with BCrypt. I can do around 1,000 hashes per second, so that's about three hours. Maybe I'll do the top five hundred million just to be sure.

It doesn't really matter that this is going to take long time to run — I'm only doing it once. Let's call this mapping of strings to hash outputs a ["rainbow table"](https://en.wikipedia.org/wiki/Rainbow_table).

Now, when I get your database, I just look and see if any of the passwords there are in my rainbow table. If they are, then I know the password.

The solution to this problem is *salting* our passwords. A salt is a random string prepended to the password before hashing it. It's stored in plain text next to the password, so it's not a secret. But the fact that it's there makes an attacker's life much more difficult: it's very unlikely that I constructed my rainbow table with your particular salt in mind, so I'm back to running the hash algorithm over and over as I guess passwords. And, remember, BCrypt is designed to be expensive to run.

Let's update our `User` model to use BCrypt:
```ruby
# Gemfile:
gem 'bcrypt'

# app/models/user.rb
class User < ActiveRecord::Base
  def password=(new_password)
    salt = BCrypt::Engine::generate_salt
    hashed = BCrypt::Engine::hash_secret(new_password, salt)
    self.password_digest = salt + hashed
  end

  # authenticate(password: string) -> User?
  def authenticate(password)
    # Salts generated by generate_salt are always 29 chars long.
    salt = password_digest[0..28]
    hashed = BCrypt::Engine::hash_secret(password, salt)
    return nil unless (salt + hashed) == self.password_digest
  end
end
```

Our `users.password_digest` column really stores two values: the salt and the actual return value of BCrypt. We just concatenate them together in the column and use our knowledge of the length of salts — `generate_salt` always produces 29-character strings — to separate them.

After we've loaded the User, we find the salt which we previously stored in their `password_digest` column. We run the password we were given in `params` through BCrypt along with the salt we read from the database. If the results match, you're in. If they don't, no dice.

You don't have to deal with all this yourself. Rails provides a method called `has_secure_password` that you can use on your ActiveRecord models to handle all this. It looks like this:

```ruby
class User < ActiveRecord::Base
  has_secure_password
end
```

You'll need to add `gem 'bcrypt'` to your Gemfile if it isn't already.

[`has_secure_password`](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html) adds two fields to your model: `password` and `password_confirmation`. These fields don't correspond to database columns! Instead, the method expects there to be a `password_digest` column defined in your migrations.

`has_secure_password` also adds some `before_save` hooks to your model. These compare `password` and `password_confirmation`. If they match (or if `password_confirmation` is `nil`), then it updates the `password_digest` column pretty much exactly like our example code before did.

These fields are designed to make it easy to include a password confirmation box when creating or updating a user. All together, our very secure app might look like this:

```ruby
# app/views/user/new.html.erb
<%= form_for :user, url: '/users' do |f| %>
  Username: <%= f.text_field :username %>
  Password: <%= f.password_field :password %>
  Password Confirmation: <%= f.password_field :password_confirmation %>
  <%= f.submit "Submit" %>
<% end %>

# app/controllers/users_controller.rb
class UsersController < ApplicationController
  def create
    user = User.new(user_params).save
  end

  private

  def user_params
    params.require(:user).permit(:username, :password, :password_confirmation)
  end
end

# app/controllers/sessions_controller.rb
class SessionsController < ApplicationController
  def create
    @user = User.find_by(username: params[:username])
    return head(:forbidden) unless @user.authenticate(params[:password])
    session[:user_id] = @user.id
  end
end

# app/models/user.rb
class User < ActiveRecord::Base
  has_secure_password
end
```

Vide [Review of authentication](https://www.youtube.com/watch?v=gB7lYvfL4J4) 

### OmniAuth
Passwords are terrible. For one thing, you have to remember them. Or you have to use a password manager, which comes with its own problems. Unsurprisingly, some percentage of users will just leave and never come back the moment you ask them to create an account.

And then on the server, you have to manage all these passwords. You have to store them securely. Rails secures your passwords when they are stored in your database, but it does not secure your servers, which see the password in plain text. If I can get into your servers, I can edit your Rails code and have it send all your users' passwords to me as they submit them. You'll also have to handle password changes, email verification, and password recovery. Inevitably, your users accounts will get broken into. This may or may not be your fault, but when they write to you, it will be your problem.

What if it could be someone else's problem?

Like Google, for example. They are dealing with all these problems somehow (having a huge amount of money helps). For example, when you log into Google, they are looking at vastly more than your username and password. Google considers where you are in the world (they can guess based on [your IP address][ip_geolocation], the operating system you're running (their servers can tell because they [listen very carefully to your computer's accent when it talks to them][ip_fingerprinting]), and numerous other factors. If the login looks suspicious — for instance, you usually log in on a Mac in New York, but today you're logging in on a Windows XP machine in Thailand — they may reject it or ask you to solve a [captcha][CAPTCHA].

[Omniauth][omniauth] is a gem for Rails that lets you use multiple authentication providers on your site. You can let people log in with Twitter, Facebook, Google, or with a username and password.

Here's how it works from the user's standpoint:

  1. I try to access a page which requires me to be logged in. I am redirected to the login screen.
  2. It offers me the options of creating an account or logging in with Google or Twitter.
  3. I click "Login with Google". This momentarily sends me to `$your_site/auth/google`, which quickly redirects to the Google signin page.
  4. If I'm not signed in to Google, I sign in. More likely, I am already signed in to Google, so Google asks me if they should let `$your_site` know who I am. I say yes.
  5. I am (hopefully quickly) redirected to `$your_site/auth/google/callback` and, from there, to the page I initially tried to access.

Let's see how this works in practice:

#### Omniauth with Facebook
The Omniauth gem allows us to use the OAuth protocol with a number of different providers. All we need to do is add the Omniauth gem *and* the provider-specific Omniauth gem (e.g., `omniauth-google`) to our Gemfile. In this case, add `omniauth` and `omniauth-facebook` to your Gemfile, and then `bundle`. If we were so inclined, we could add additional Omniauth gems to our heart's desire, offering login via multiple providers in our app.

Next, we'll need to tell Omniauth about our app's OAuth credentials.
Create `config/initializers/omniauth.rb`. It will contain this:
```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :facebook, ENV['FACEBOOK_KEY'], ENV['FACEBOOK_SECRET']
end
```
The `ENV` constant refers to a global hash for your entire computer environment. You can store any key value pairs in this environment, so it's a very useful place to store credentials that we don't want to be managed by Git or stored on GitHub (especially if your GitHub repo is public). The most common error we see from students here is that when ENV["PROVIDER_KEY"] is evaluated in the initializer it returns `nil`. Then later when you try to authenticate with the provider you'll get some kind of 4xx error because the provider doesn't recognize your app.

To recieve these credentials, each provider's process is different. You'll essentially need to register your app with the provider, and they'll give you a set of keys specific to your app.

For Facebook:
Log in to [the Facebook developer's panel](https://developers.facebook.com). Create an app, copy the key (it's called "App ID" on Facebook's page) and the secret, and set them as environment variables in the terminal:

```bash
export FACEBOOK_KEY=<your_key>
export FACEBOOK_SECRET=<your_key>
```

Here's a [quick video](https://youtu.be/1yryyKB7Edk) as this often trips people up (including experienced folks). Make sure you do the last two steps of setting your URL and valid domains. If you don't, Facebook will think you're making a request from an invalid site and will never let the user log in.

Running these commands will make these key-value pairs appear in the ENV hash in Ruby in that terminal. A more lasting way to do this is using the Figaro or Dotenv gems.

Jump into the console to check that you have set the keys properly. If `ENV["FACEBOOK_KEY"]` and `ENV["FACEBOOK_SECRET"]` return your keys you're all set!

We now need to create a link that will take the user to Facebook to login.  Create a link anywhere you'd like that sends the user to "/auth/facebook". We'll need a route, a controller, and a view. I'll only show the view.

```erb
<!-- app/views/static/home.html.erb -->

<%= link_to("login with facebook!", "/auth/facebook") %>
```

**Hot-Tip:** Log out of Facebook before you do this portion so you can see the full flow.

Let's visit this page in the browser and click on the link.
Clicking on the link clearly sends a GET request to your server to "/auth/facebook", but in the browser we end up at "https://www.facebook.com/login.php?skip_api_login=1&api_key=1688265381390456&signed_next=1&next=https%3A%2F%2Fwww.facebook.com%2Fv2.5%2Fdialog%2Foauth%3Fredirect_uri%3Dhttp%253A%252F%252Flocalhost%253A3000%252Fauth%252Ffacebook%252Fcallback%26state%3Dc7e7feeea98f875e7a77d76f7385ea2960db3dc23a397c4b%26scope%3Demail%26response_type%3Dcode%26client_id%3D1688265381390456%26ret%3Dlogin&cancel_url=http%3A%2F%2Flocalhost%3A3000%2Fauth%2Ffacebook%2Fcallback%3Ferror%3Daccess_denied%26error_code%3D200%26error_description%3DPermissions%2Berror%26error_reason%3Duser_denied%26state%3Dc7e7feeea98f875e7a77d76f7385ea2960db3dc23a397c4b%23_%3D_&display=page"

This URL has a whole bunch of parameters all URL [encoded](http://ascii.cl/url-encoding.htm) (which is why they look so strange).  At this point we are at Facebook's site because somewhere in our app Omniauth sent the browser a redirect to that url (which the gem intelligently autogenerated for us).

Once we're at Facebook and the user logs in, Facebook will send the browser ANOTHER redirect with the URL Omniauth told it about in the previous URL. Omniauth always wants Facebook to redirect us back to our server to the route "/auth/whatever_provider/callback".  Along with that request the provider will also send back a whole bunch of information for us.

The URL in your browser should look something like this mess: "http://localhost:3000/auth/facebook/callback?code=AQA_CrhVYnuufhQid-3vS1NvI5rZfk4uPJwFZIymA90JeUR7NDFFy0bHQjbtneLkymqqZlmFbjcg2A0y5zRmaCy0D7k9H46F3j9pm9slzBIN9fM4Q54zAdiVZo2k6XtiMPZ_AG2xEZ8MyiTtbbQOBdaK57PY7lr7iLuFeaVUCUnZC69ddzcq_tLILEkjagSyWXi8WGGshbnIwy9C6d98hnoxl6AJjIi4TC3FScEAxKQ9vH1tXntQ9YvTLNWlWsWUcbefEq1RlywNi3IqGsLnDgyyRcHph0u4-TpnaqZPxHSNdcWCgnYfHK_bSO-R_a3H4Oo&state=60fb843af784e411ea7b5f809e34dd29d5e4eda891d0c4c1#_=_"

You should now see a routing error, `No route matches [GET] "/auth/facebook/callback"`.

Let's add something to handle that redirect:
```ruby
# config/routes.rb

get '/auth/facebook/callback' => 'sessions#create'

# Note that the controller and action you use don't matter, but it's most logical to use the SessionsController because we're going to log the user in by creating a session.
```

Now we create a `SessionsController`. Our goal here is to either create a new user or find the user in our database and log them in.  Facebook sends us a bunch of information back, and Omniauth parses it for us and puts it in the request environment: `request.env['omniauth.auth']`. We can use the information in there to log the user in (if they have an existing account) or create a new user.

Here's a sample of the auth hash Facebook sends us:
```ruby
{
  :provider => 'facebook',
  :uid => '1234567',
  :info => {
    :email => 'joe@bloggs.com',
    :name => 'Joe Bloggs',
    :first_name => 'Joe',
    :last_name => 'Bloggs',
    :image => 'http://graph.facebook.com/1234567/picture?type=square',
    :urls => { :Facebook => 'http://www.facebook.com/jbloggs' },
    :location => 'Palo Alto, California',
    :verified => true
  },
  :credentials => {
    :token => 'ABCDEF...', # OAuth 2.0 access_token, which you may wish to store
    :expires_at => 1321747205, # when the access token expires
    :expires => true # this will always be true
  },
  :extra => {
    :raw_info => {
      :id => '1234567',
      :name => 'Joe Bloggs',
      :first_name => 'Joe',
      :last_name => 'Bloggs',
      :link => 'http://www.facebook.com/jbloggs',
      :username => 'jbloggs',
      :location => { :id => '123456789', :name => 'Palo Alto, California' },
      :gender => 'male',
      :email => 'joe@bloggs.com',
      :timezone => -8,
      :locale => 'en_US',
      :verified => true,
      :updated_time => '2011-11-11T06:21:03+0000'
    }
  }
}
```
Let's log the user in! (We've omitted the model-related code.)

```ruby
# app/controllers/sessions_controller.rb

class SessionsController < ApplicationController

  def create
    user = User.find_or_create_by(:uid => auth['uid']) do |u|
      u.name = auth['info']['name']
      u.email = auth['info']['email']
    end
    session[:user_id] = user.id
  end

  def auth
    request.env['omniauth.auth']
  end
end
```

That completes the whole OAuth login flow!
Implementing the OAuth protocol yourself is extremely complicated. Using the Omniauth gem along with any Omniauth-provider gem(s) allows users to log in to your site easily, streamlining the process. However, it still trips a lot of people up! Make sure you understand each piece of the flow, what you expect to happen, and any deviance from the expected result. The end result should be getting access to the user's data from the provider in your `SessionsController`, where you can decide what to do with it. Typically, you'll create a new `User` in your database using their provider data (if they are not already an existing user) and/or log them in.

* Watch the [Omniauth Video Review](https://github.com/learn-co-curriculum/omniauth-review-lecture-in-todomvc)
* More about [Managing Environment Variables](https://launchschool.com/blog/managing-environment-configuration-variables-in-rails)

[ip_geolocation]: https://en.wikipedia.org/wiki/Geolocation
[ip_fingerprinting]: https://en.wikipedia.org/wiki/TCP/IP_stack_fingerprinting
[CAPTCHA]: https://en.wikipedia.org/wiki/CAPTCHA
[yak]: https://en.wiktionary.org/wiki/yak_shaving
[omniauth]: https://github.com/intridea/omniauth





