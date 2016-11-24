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
    get         'about',    to: 'static#about'
#   HTTP verb   URL path    method in StaticController

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

Below is the flow that takes place when a user attempts to go to a page on a Rails application:

1. A URL is entered into the browser; this is the HTTP request
2. That request is sent to the server where the application's router interprets the request and sends a message to the controller mapped to that route
3. The controller communicates with the view file mapped to the controller method
4. The server returns that HTTP response, which contains the view page that can be viewed in the browser

Rails gives us two options for how views are mapped between the controller and view files:
- Explicit rendering - for explicit rendering, Rails lets you dictate which view file you want to have the controller action mapped to.
- Implicit rendering - for implicit rendering, Rails follows a standard convention that automatically looks for the view file with the same name as the controller action. Rails core team has built out a number of standardized processes, such as implicit view rendering to help make development life a little easier.

Typically, you will find that you want to utilize the implicit workflow in your day to day coding practice. The rationale is quite practical. Imagine that you are taking over a legacy Rails project. Rails has always had the goal of making the development process as efficient as possible, which is why it is typically best to follow these types of implicit procedures.