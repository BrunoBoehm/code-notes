# Rack
There is a `gem` for Ruby that will help us create a web server at its simplest. This gem is called Rack and it is what Rails is built on top of. 
```ruby
gem 'rack'
```

Before we get to the complexity of Rails, let's first understand HTTP in its simplest form.

To work with Rack we need to create a new class that responds to a single method: #call. All this method needs to do is return a response which consists of the status code, any headers, and the body. This can all be done using the Rack::Response object.

```ruby
class Application
    def call(env)
        resp = Rack::Response.new
        
        # app code begins
        resp.write "Hello, World"
        # app code ends
        
        resp.finish
    end
end
```
We first create a `Rack::Response` object, then add some text "Hello, World" to the body, and complete the response with the `#finish` method. By default, Rack sets our status codes and headers.

The above is the code that will be run whenever there is a request received.

Don't worry about the `env input. This holds all of the request info in it and we will use it later!

We need to actually set up an HTTP web server that will receive the HTTP request, send it through the above `#call` method, and then serve the response to the browser. We do this using a `config.ru` file and the `rackup` command.

```ruby
# config.ru
require_relative "./application.rb"

run Application.new
```
To run this code we then run rackup `config.ru`. Everything goes as planned, you'll see:
```ruby
[2015-11-27 16:48:22] INFO  WEBrick 1.3.1
[2015-11-27 16:48:22] INFO  ruby 2.1.3 (2014-09-19) [x86_64-darwin13.0]
[2015-11-27 16:48:22] INFO  WEBrick::HTTPServer#start: pid=11275 port=9292
```
If you open your browser and go to `http://localhost:9292/` you should see `Hello, World`.
Let's deconstruct this URL a little bit though. 
- The URL is http://localhost:9292/. 
- The protocol is http. That makes sense, but the domain is localhost:9292. `localhost` is normally where a server like google.com goes. In this case, since you are running the server on your computer, localhost is the server name of your own computer. 
- The last part of that URL is the :9292 section. This the "port number" of your server. You may want to run multiple servers on one computer and having different ports allows them to be running simultaneously without conflicting.
- The resource that you are requesting is /. This is effectively like saying the home or default.

To exit the running web server and get back to your terminal press `CTRL-C`. You will have to do this every time you change your code.

## Dynamic Web Apps with Rack
A dynamic web app in Rack is pretty straightforward. Let's say we wanted to create a simple slots game.

```ruby
class Application
 
  def call(env)
    resp = Rack::Response.new
 
    # App code begins
    num_1 = Kernel.rand(1..20)
    num_2 = Kernel.rand(1..20)
    num_3 = Kernel.rand(1..20)
 
    if num_1==num_2 && num_2==num_3
      resp.write "You Win"
    else
      resp.write "You Lose"
    end
    
    now = Time.now

    if now.hour >=12
      resp.write "Good Afternoon"
    else
      resp.write "Good Morning"
    end
    # App code ends
 
    resp.finish
  end
 
end
```
Notice that we only changed the `puts` statements into `resp.write` statements. The `#write` method can be called many times to build up the full response. The response isn't sent back to the user until `#finish` is called. Web servers are just big ruby apps that respond to the user in an `HTTP` response rather than via `puts` statements.

Then run it with rackup `config.ru`.

## The HTTP request: route and GET params
An HTTP Request that our browser sends to the server contains two main sections of information.
- One is headers 
- One is the the resource or path requested (for example, /search or /profile_name)

The path that is requested is the resource that the client wants, which specific part of your server it wants.

The path lives in the HTTP request, and to get to it we have to inspect the env part of our `#call` function.

```ruby
class Application
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
    resp.finish
  end
 
end
```

This [Rack::Request](http://www.rubydoc.info/gems/rack/Rack/Request) instance now has a ton of useful methods. For instance it has a method called `#path`. This will return the path that was requested.

```ruby
class Application
 
  @@items = ["Apples","Carrots","Pears"]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    @@items.each do |item|
      resp.write "#{item}\n"
    end
 
    resp.finish
  end
end
```
If we leave it this way, it will list all of your items no matter what `path` you put in. `localhost:9292/items`, `localhost:9292/cart`, `localhost:9292/flatiron/is/awesome`. All of those URLs work since we're not filtering for path. In other words, no matter what the request is, we send the same response.

Let's filter so that this only works for the `/items` path using the `#path` method of our `Rack::Request` object:
```ruby
class Application
 
  @@items = ["Apples","Carrots","Pears"]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path.match(/items/)
      @@items.each do |item|
        resp.write "#{item}\n"
      end
    else
      resp.write "Path Not Found"
    end
 
    resp.finish
  end
end
```

What if users wanted to check and see if we have Apples available in our list of items? How do other websites handle getting user queries? 
If we go to GitHub and type in "apples" in the search query, we get a URL that looks like this: `https://github.com/search?q=apples`. We have a domain of `github.com`, path of `search`, and then a `?` character. After that character comes `q=apples`.

The section after the `?` is called the `GET` parameters. If you notice in the above example, GET params come in `key/value` pairs. The key in GitHub's case would be q and the value is apples. This is like a `Hash` structure! Thankfully, Rack provides the mechanism to parse the `GET` params and return them to us in a standard `Hash`.

```ruby
class Application
 
  @@items = ["Apples","Carrots","Pears"]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path.match(/items/)
      @@items.each do |item|
        resp.write "#{item}\n"
      end
    elsif req.path.match(/search/)
 
      search_term = req.params["q"]
 
      if @@items.include?(search_term)
        resp.write "#{search_term} is one of our items"
      else
        resp.write "Couldn't find #{search_term}"
      end
 
    else
      resp.write "Path Not Found"
    end
 
    resp.finish
  end
end
```

## Dynamic Routes
When you create a new repository on GitHub, how do URLs like github.com/jmburges/my-repo get generated? In our current examples, we would have to create a new if statement for each possible URL path. 

Since this is a dynamic application, our application can't be rewritten every time a new user signs up. So the concept of "dynamic routes" was created.

Let's assume we have a playlister app which has an array of Songs. First let's look at our `Song` object:
```ruby
#song.rb
class Song
    attr_accessor :title, :artist
end
```
Now we also have our web app:
```ruby
class Application
 
  @@songs = [Song.new("Sorry", "Justin Bieber"),
            Song.new("Hello","Adele")]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    @@songs.each do |song|
      resp.write "#{song.title}\n"
    end
 
    resp.finish
  end
end
```
Similarily to GitHub, we want to be able to go to a URL like `localhost:9292/songs/Sorry` and get all the information on Sorry.
Thankfully, because paths are strings, we can do a regex match against the path. Then we just grab the content after the `/song/` to figure out which Song our user would like.
```ruby
class Application
 
  @@songs = [Song.new("Sorry", "Justin Bieber"),
            Song.new("Hello","Adele")]
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path.match(/songs/)
 
      song_title = req.path.split("/songs/").last 
      # turns /songs/Sorry into Sorry
      song = @@songs.find{|s| s.title == song_title}
 
      resp.write song.artist
    end
 
    resp.finish
  end
end
```
Now our routes are dynamic! We can just add songs, and everything else is taken care of and works for us.

## HTTP Status Codes
Status codes allow your server to tell something special to the client. The responses you send need to be effective to both a human user and to the browser itself.

That means that response messages like `File Not Found` or `Item isn't in the cart` work if there is a human to read the English. Browsers also want to know the status of the response. To get that response, the HTTP protocol has an agreed upon contract for different "status codes". 

A [status code](http://www.tutorialspoint.com/http/http_status_codes.htm) is a 3-digit integer where the first digit represents the class of the response, and the remaining two digits represent a specific status. There are 5 primary values that the first digit can take.

Status Number | Code/Description
------ | ------
1 | 1xx: Informational (request received and continuing process)
2 | 2xx: Success (request successfully received, understood, and accepted)
3 | 3xx: Redirection (further action must be taken to complete request)
4 | 4xx: Client Error (request contains bad syntax and can't be completed)
5 | 5xx: Server Error (server couldn't complete request)

In Rack, we are able to set the response's status code by just setting the status_code attribute. By default, Rack sets a status code of `200`. But when a user selects a route that doesn't exist, we need to set the status to `404`.
```ruby
class Application
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path=="/songs"
      resp.write "You requested the songs"
    else
      resp.write "Route not found"
      resp.status = 404
    end
 
    resp.finish
  end
end
```





