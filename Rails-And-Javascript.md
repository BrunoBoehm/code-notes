# Rails and Javascript

## Workers
### Long-running tasks in Rails 
Most of the things we do in Rails apps revolve around simple tasks that affect only one or a few database objects at a time, and are handled in milliseconds, quickly enough that the impact on the request/response time of our pages is minimal.
Sometimes, however, we need to do something that takes longer to do and heavily impacts page load time, like sending a bunch of emails, uploading large files, manipulating a lot of records, or generating a report. Let's look at how that might happen.

A common task in many systems is importing records from a Comma-Separated Value (CSV) file. You might need to do this for any number of reasons, from seeding a new database to integrating and sharing data from another system.

Our sales team has just aquired some hot leads and wants them in the system right away, in our simple "customer database" app. It has a list of all customers at `/customers`.

Turns out we'll be getting new leads via a CSV every week, so instead of importing them manually, we'll just build a feature where the sales team can do it themselves.

We'll let them do the import right from the customers `index`, so let's just add a route to take the file upload:

```ruby
# config\routes.rb

  resources :customers, only: [:index]
  post 'customers/upload', to: 'customers#upload'
```

Then let's add the controls to upload a file to our `index` view:

```erb
# views\customers\index.html.erb

<h1>Our Customers</h1>
<%= form_tag customers_upload_path, multipart: true do %>
  <%= file_field_tag :leads %>
  <%= submit_tag "Import Leads" %>
<% end %>
<ul>
<% @customers.each do |customer| %>
# ...
```

Finally, let's get into our controller and handle this file upload.
Check out `db/customers.csv`. The first row defines the fields, and isn't part of the dataset, so that's called a `header`. 

Each row is ordinal, like an array, so position 0 is email, position 1 is first name, and position 2 is last name.

Armed with all that, we can build our controller method:

```ruby
# controllers\customers_controller.rb

class CustomersController < ApplicationController
  require 'csv'

  def index
    @customers = Customer.all
  end

  def upload
    CSV.foreach(params[:leads].path, headers: true) do |lead|
      Customer.create(email: lead[0], first_name: lead[1], last_name: lead[2])
    end
    redirect_to customers_path
  end

end
```
**Note:** We're using the `CSV` library (`require 'csv'`), which is a standard Ruby library, to do the processing. You can learn more about it [here](http://ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html)

To import a line at a time from a csv at a given `path`
```ruby
CSV.foreach("path/to/file.csv") do |row|
  # use row here...
end
```

Okay, let's reload `/customers`, select `customers.csv` from our project's `db` directory, and click go. Okay, eventually, the page should refresh and we'll see our new customers. Depending on the speed of your computer, it might have taken upwards of a full minute for 25,000 rows. 25,000 might seem like a lot, but systems routinely process millions and millions of rows of data every day.

We've seen how something as straightforward as creating records from a data file can take almost a minute for your server to process and reload a page. And while a minute might seem pretty fast to create 25K records, consider it from the standpoint of the user.

### Sidekiq
If a long-running task takes too long, it will frustrate users and possibly cause time-outs and lead to errors.
In our case, our sales team is happy that we gave them a way to upload leads, but they want to be able to get right back to contacting customers and not have to wait for the uploading and processing to complete.

In Rails, we talk about *workers* as processes that will allow us to execute tasks outside of the main web application thread.

The reason it takes so long to load `/customers` after uploading our file is that we are processing the file on the same thread as the request/response. File processing becomes a *blocking* action on the thread, and nothing else can happen until that's done, causing the sales person to sit and stare at a blank screen for a minute.

Fortunately, in the Rails ecosystem, we have a lot of tools we can use to **move blocking tasks to a background worker and allow the main thread to continue**, providing a much snappier response for the user.

One of the most popular background worker tools is [Sidekiq](https://github.com/mperham/sidekiq).

We'll use Sidekiq to move our leads processing upload to a background thread so that our sales people can go back to work on the `/customers` page right away.

Let's get it started.

First we add Sidekiq to our Gemfile and run `bundle install`:

```ruby
gem 'sidekiq'
```

Sidekiq depends on [Redis](http://redis.io/), which you'll need to install. If you're running on OS X, you can run

```bash
brew install redis
```

and follow the post-install instructions to get everything up and running. On Debian-based systems (including Ubuntu), try

```bash
sudo apt-get install redis-server
```

You can also always [download Redis](http://redis.io/download) manually or exercise your Google-fu to figure out the best installation for your setup.

Sidekiq relies on a `Worker` to define and process a *job*. Let's add an `app/workers` directory and create our first worker:

```ruby
# app/workers/leads_worker.rb

class LeadsWorker
  include Sidekiq::Worker

  def perform(leads_file)

  end
end
```

Now that we have our worker, we need to move the processing from the controller to the worker. All you have to do is take the long-running code out of one place, and put it inside of `perform`:

```ruby
# app/workers/leads_worker.rb

class LeadsWorker
  require 'csv'
  include Sidekiq::Worker

  def perform(leads_file)
    CSV.foreach(leads_file, headers: true) do |lead|
      Customer.create(email: lead[0], first_name: lead[1], last_name: lead[2])
    end
  end
end
```

We've taken the loop that processes the file in our controller, and just put it inside of `perform`. Make sure to `require 'csv'` at the top, and update the `CSV.foreach` to work with `leads_file`.

Now, in our controller, we want to tell it to run this worker rather than process the file inline:

```ruby
# controllers/customers_controller.rb
class CustomersController < ApplicationController

  def index
    @customers = Customer.all
  end

  def upload
    LeadsWorker.perform_async(params[:leads].path)
    redirect_to customers_path
  end
end
```

And that's it. We're now set up to run the file upload in a background worker.

To see it in action, you'll first need to start Redis: `redis-server` (on OS X you can also follow the instructions that `brew info redis` prints to start Redis automatically using launchctl.) Then run your Rails server (don't forget to restart) in one tab, and open a new Terminal tab to run Sidekiq with this command:

`bundle exec sidekiq`

Then go to `/customers` and try it out. Uploading the `db/customers.csv` file should immediately redirect you to `/customers`, where you can continue your work, and periodically refresh to see new entries!

If you watch the tab running Sidekiq, you'll see something like:

`LeadsWorker JID-bca14e512da7b759ad6cf37b INFO: start`

That's how you know the job has started, and you can start refreshing the page.











JUMPING TO...

## What's an API
Let's say you're building an app that allows users to find the nearest coffee shop that is most likely to have an open table where they can sit and work on their novel.
You need to be able to search for coffee shops and find out things like how many people have recently gone there.
One of the great things about the web is that we can find all kinds of data and use it in new and interesting ways. In this case, we know that Foursquare deals with exactly that sort of thing, so we'll try to get data from them.

One thing we could do is screen-scrape Foursquare, but you do so in code so that instead of seeing rendered HTML, you have to `parse` the HTML to see what you want. So, we could go to Foursquare and use their search feature and look at the URL generated by the search to figure out how to create our own request. In this case, searching for coffee shops in New York would generate a URL like this:
`https://foursquare.com/explore?mode=url&near=New%20York&q=coffee%20shops`
We could then use Nokogiri and Ruby to request that page and parse the data.
```ruby
# irb
doc = Nokogiri::HTML(open('https://foursquare.com/explore?mode=url&near=New%20York&q=coffee%20shops'))

doc.css('li.singleRecommendation').each do |recommendation|
  # ...
end
```
We could inspect that source and figure out that the results are all in `<li>` elements with a `class="singleRecommendation"`. Now what happens when Foursquare does a minor CSS tweak and changes `.singleRecommendation` to `.recommendationCard`. Our whole app is broken! And we won't know about it until a user gets an error. And Foursquare has no responsibility to us because scraping their page is not an intended use of their system.

Okay, instead of spending hours and hours parsing their new HTML, let's see if they have an API. Some applications, knowing that their data might be of use to other applications, provide an API as well as the standard web interface.

An API (Application Programming Interface) is a way for one system to interact with another via a well-defined interface. **An interface is any endpoint that can be used to take actions and consume data on a given application**. 

The HTML response of every web site is an interface, for instance. The web interface provides HTML, which is a combination of data (the dynamic values like venue names, addresses, and check-ins) and presentation (HTML tags, CSS classes, JavaScript effects) meant to be consumed by a web browser. This is great for end users who want to see something visually compelling.

The benefit of an API is that it's put together explicitly for the purpose of being used by another system, rather than being viewed by a human, so the data that it returns is unencumbered by the presentation, preventing the consuming code from having to wade through HTML just to find values. A good API just provides data in the easiest format possible for code to digest.

Most modern web APIs use `JavaScript Object Notation`, or `JSON`, as a standard way of describing and defining data. Some APIs, usually older ones, will use `XML` rather than `JSON` to describe data, though that is increasingly rare. However, the remnants of that great stone age are found in AJAX, which stands for Asynchronous JavaScript and XML, XML being the standard when AJAX hit the scene. **When we use AJAX today we're almost always using JSON rather than XML, but we haven't changed the name because AJAJ would be a weird acronym.**

JSON looks a lot like a Ruby hash, and as a result, figuring out how to access a given piece of data is a snap.
```json
{
  "name": "Cuppa Joe's",
  "address": "123 Not A Fake Address Street",
  "city": "Real City",
  "state": "New York",
  "zip": "10015"
}
```
//Flat-fact: JSON is canonically pronounced like the name "Jason", because it makes it easy to carve up data

Most good APIs also use REST, just like Rails, so finding your way around should start to feel familiar and consistent as you learn what resources are available. Keep in mind, though, that you will only have access to what the API creator allows.

### APIs and Postman
The first thing we want to do is install a couple of tools to make our lives a lot easier.
- Postman is a Chrome app that we can use to easily make API requests. Check out the docs and install the app from here.
- JSONView is a Chrome extension that will automatically make JSON data much more readable in Chrome.

Let's try to get venue data on Foursquare. We'll check out the Venues endpoint in the documentation. Venue Detail is the name of the function, or `endpoint`, that we are going to use, and its URI takes the format: `https://api.foursquare.com/v2/venues/VENUE_ID`. This RESTful URI looks a lot like one we'd use in a Rails app to allow access to a given venue. The document tells us that we will need to access this endpoint with the HTTP `GET` verb, and that `VENUE_ID` is a parameter that we will need to provide.

API SECRET: Most API providers require a similar process to use their API: creating an app will give us a client ID and a secret that we can use as credentials for the API. Rhe client ID/secret pair authorizes the application access the API. API providers want to know who is accessing their API. There are several reasons for this, but an important one is quality control.

Let's say one bad application is using the API in unintended ways, perhaps making too many requests in a short period of time. This is problematic because it can negatively affect other API users who are behaving appropriately. By requiring a client ID/secret authorization, Foursquare can know who the offender is and turn off their access, ensuring quality for other API users.

Let's create the app and do the request
`https://api.foursquare.com/v2/venues/40a55d80f964a52020f31ee3?client_id=YOUR_CLIENT_ID&client_secret=YOUR_SECRET&v=20150201`
It should return something like
```json
response: {
  venue: {
    id: "40a55d80f964a52020f31ee3",
    name: "Clinton St. Baking Co. & Restaurant",
    contact: {
    phone: "6466026263",
    formattedPhone: "(646) 602-6263"
  },
  location: {
    address: "4 Clinton St",
    crossStreet: "at E Houston St",
    lat: 40.72107924768216,
    lng: -73.98394256830215,
    postalCode: "10002",
    cc: "US",
    city: "New York",
    state: "NY",
    country: "United States",
    formattedAddress: [
      "4 Clinton St (at E Houston St)",
      "New York, NY 10002",
      "United States"
    ]
  },
// ...
```
Note: Remember that with a `GET` request, we pass parameters via a `querystring` by putting a `?` after the url and then formatting parameters like this: `param=value&param2=value`.

Typing all that into the URL bar of our browser every time is tedious, and makes it easy to make a mistake, so we can use Postman, which will make it much easier to deal with parameters, headers, and anything else we might need to do to explore an API.
Open postman, then enter the base URL in the URL field: `https://api.foursquare.com/v2/venues/40a55d80f964a52020f31ee3`. Then click Params, and you can enter each parameter separately below. Enter the following three things in the appropriate columns:
```
URL Parameter Key        Value
client_id               YOUR CLIENT ID
client_secret           YOUR SECRET
v                       20160201
```
The `v` parameter we passed in was a version parameter that the Foursquare API requires. Try taking it out by deleting the v parameter and value from Postman and hitting "Send" again.

Versioning is an important tool in API use. When an API changes, either because of new features or changed endpoints, not all API clients will be able to keep up with those changes immediately: any application using the API would run the risk of being broken until they updated their code.
By versioning the API, you can guarantee that anyone using the current version can keep using it, and upgrade to the new version when they can. You leave the old endpoints in place (e.g. `https://api.foursquare.com/v1/venues`) and roll out a new one under a new version namespace (e.g. `https://api.foursquare.com/v2/venues`).





