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

## Edit/Update actions
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


