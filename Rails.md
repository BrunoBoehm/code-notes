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


