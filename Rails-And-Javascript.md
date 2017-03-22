# Rails and Javascript
## Asset Pipeline
For a long time, we treated JavaScript and CSS as an afterthought in developing web applications. All of our asset code — things like images, stylesheets, and JavaScripts — was kept in a massive folder called `public` and served outside of the context of our Rails application. As the web evolved, that no longer made sense.

The asset pipeline is the Rails answer to managing stylesheets, JavaScripts, and images.

A lot of files go into creating web applications. The CSS and JavaScript files alone can be hard to organize. What folders do we create? Which files go where? The Asset Pipeline provides an answer for this problem. We have to keep things very organized in our application, but, by keeping separate files and folders for each concept or unit of code, we have 2 problems.

1. How does Rails know where things are? Is the calendar JS file in `app/assets/javascripts/calendar.js` or `vendor/javascripts/calendar.js`?
2. We don't want to serve each file separately as this will make our page load very slow. It makes sense for us to maintain separate small files for readability and organization, but, for the browser, we'd rather smash all those small files together and load 1 JS file and 1 CSS file. This process is called concatenation.

Let's talk about our first problem: how does Rails know where to look? The Asset Pipeline has a concept called Asset Paths for handling this. Just like in BASH where we have a PATH environment variable that is a combination of folder paths, the Asset Path is a combination of folder paths for Rails to look for assets in. Let's take a look at an example of how our Asset Path is configured.

```ruby
Rails.application.config.assets.paths =>
[
  "/Users/avi/asset-test/app/assets/images",
  "/Users/avi/asset-test/app/assets/javascripts",
  "/Users/avi/asset-test/app/assets/stylesheets",
  "/Users/avi/asset-test/vendor/assets/javascripts",
  "/Users/avi/asset-test/vendor/assets/stylesheets",
  "/Users/avi/.rvm/gems/ruby-2.2.3/gems/turbolinks-2.5.3/lib/assets/javascripts",
  "/Users/avi/.rvm/gems/ruby-2.2.3/gems/jquery-rails-4.1.0/vendor/assets/javascripts",
  "/Users/avi/.rvm/gems/ruby-2.2.3/gems/coffee-rails-4.1.1/lib/assets/javascripts"
]
```
If we put an asset in any of these folders, we can access them via the URL '/assets' in our application. If you have additional folders for Rails to search, you can add the folders to the Asset Path. This is done in the file `config/initializers/assets.rb`.

```ruby
Rails.application.config.assets.paths << "New Path"
```

We can put assets anywhere, configure our Asset Path, and access them via a single /assets URL.

### Manifests and Concatenation
Now that we can put files anywhere, how do we get them to be included in our web pages? The Asset Pipeline uses a manifest file to tell Rails what to load. This manifest file is a central location where we can list all the CSS and JS files our application needs. This isn't a feature of JS or CSS but rather the asset pipeline. Here is an example of what our JS manifest file looks like:

File: app/assets/javascripts/application.js
```
//= require jquery
//= require calendar
```
When you include the manifest file in your layout with the `javascript_include_tag`, the asset pipeline will look for all of the files listed in the Asset Path. Notice how we require calendar. This file lives in `app/assets/javascripts/calendar.js`, yet we only specified the name and not the full path. The Asset Pipeline will search all the configured paths for a file with the name we provided.

Now that we solved the question of discoverability, let's talk about concatenation. Like we discussed earlier, we don't want to load our files in the browser one by one. It's better to perform one download than a bunch of small downloads from our browser. The manifest files we configure in Rails will automatically concatenate the files listed in them into one file in production. When we are developing our application this might not be the best option since it can make debugging hard but Rails will actually serve each file separately when we are running in development mode. No need to do anything.

Finally, the sprocket directives that power our asset manifests will be covered in detail later.

### Preprocessing

Being able to combine files and load them from a set of predefined locations in our application is a great benefit of the Asset Pipeline. 

That's only the beginning. Because we're loading assets through Rails, we can preprocess the files using popular languages like **SCSS** for writing better CSS and **Coffeescript** for cleaner JS. If you make an asset named theme.css.scss, you are telling the asset pipeline to run the file through the SCSS preprocessor before serving theme.css to the browser. The SCSS preprocessor compiles the file into CSS. The only thing we had to do was provide the correct file extension, `.scss`, to the file and the asset pipeline knows to run it through the SCSS preprocessor.

### Fingerprinting
The last benefit we will talk about is Fingerprinting, but first let's talk about the problem it helps us solve. When we serve files to the browser, they are likely to be cached to avoid downloading them again in the future. What's caching you might ask?

Caching something means keeping a copy of a time-consuming operation locally so that you don't have to redo the expensive operation again if the inputs and outputs are going to be exactly the same. Caches are usually key value stores, where the value is the answer to the expensive operation and the key is something that's unique to that item. 

If you request a page from the server and then request the same page from the server again, the quickest way to get that request fulfilled is to actually keep a copy of what you got last time locally. Browsers cache lots of the responses they get to requests they've made by using the headers that get sent with the response. The headers tell the browser how long the page remains 'fresh' before it 'expires.' Once the page has expired, the browser will make a new request for the page to refresh its cache. We say that the fastest request is the request that's not made. 

It's also often said that cache invalidation is one of the two hard problems in computer science, so think carefully when you start caching things! Caching saves bandwidth for us and provides a speed boost for the user. This is great until you change the file and you want all of your users to get the new one instead of the old version they have stored in their browser cache. 

But how do we let the browser know we've modified the file? If the new version has the same name as the old version, the browser will continue using the old file from its cache. We need a way to change the filename when the contents change so that browsers won't keep serving the old file.

From the [Rails Guides Primer](http://guides.rubyonrails.org/asset_pipeline.html#what-is-fingerprinting-and-why-should-i-care-questionmark)
"Fingerprinting is a technique that makes the name of a filename dependent on the contents of the file. When the file contents change, the filename is also changed. For content that is static or infrequently changed, this provides an easy way to tell whether two versions of a file are identical, even across different servers or deployment dates.

When a filename is unique and based on its content, HTTP headers can be set to encourage caches everywhere (whether at CDNs, at ISPs, in networking equipment, or in web browsers) to keep their own copy of the content. When the content is updated, the fingerprint will change. This will cause the remote clients to request a new copy of the content. This is known as cache busting.

The technique sprockets uses for fingerprinting is to append a hash of the content to the end of the file name. For example, take a CSS file named `global.css`. Sprockets will add the hash `908e25f4bf641868d8683022a5b62f54` to the end of the file name like so:
```
global-908e25f4bf641868d8683022a5b62f54.css
```

If you happen to be using an older version of Rails (Rails 2.x), the strategy used to be to append a date-based query string to every asset linked with a built-in helper. This looked like so:
```
global.css?1309495796
```

The query string strategy has several disadvantages:

- Not all caches will reliably cache content where the filename only differs by query parameters.
    + Steve Souders recommends "avoiding a querystring for cacheable resources." 5-20% of your requests will not be cached. Query strings in particular do not work at all with some CDNs for cache invalidation.
- The file name can change between nodes in multi-server environments.
    + The default query string in Rails 2.x is based on the modification time of the files. When assets are deployed to a cluster, there is no guarantee that the timestamps will be the same, resulting in different values being used depending on which server handles the request.
- Too much cache invalidation.
    + When static assets are deployed with each new release of code, the mtime (time of last modification) of all these files changes, forcing all remote clients to fetch them again, even if the content of those assets has not changed.

Fingerprinting fixes all these problems by ensuring that filenames are consistent based on their content.

Fingerprinting is enabled by default for production and disabled for all other environments. You can enable or disable it in your configuration through the `config.assets.digest` option."

**CONCLUSION**: The Asset Pipeline is definitely more complex then just serving assets from a public folder and it can be hard to debug. Learning how to use it will pay off in the long run by saving us time and headaches. Just think about all the problems it solves for us.

1. Asset Paths
2. Manifests and Concatenation
3. Preprocessing
4. Fingerprinting

Finally, definitely check out the DHH Keynote where he introduces the asset pipeline, https://www.youtube.com/watch?v=cGdCI2HhfAU. It's good — I promise.

## JS Manifests
Manifest files have special characteristics about them that differentiate them from plain old JS files. If you open your JavaScript manifest, `app/assets/javascripts/application.js`, you will see that some of the lines start with `//=`. This is called a directive, and it tells sprockets that this isn't a normal JS comment. 

While there are a number of [different directives](https://github.com/rails/sprockets#sprockets-directives) available, let's learn about the one that is used to list the JS files we want to include in our application. This directive, `require`, tells sprockets that the file we are specifying should be loaded. One thing to note is you don't have to put the file extension. If your file is `main.js` then you only need to type `//= require main`.

You may notice another directive in your manifest file. The `require_tree` directive tells sprockets to load all files in the given folder. By default, the manifest file has `//= require_tree .` which will include all JS files in the same folder that `application.js` is located. This makes adding new JS files into our application really easy but can cause problems. 

As your application grows, you may not want all of the JS loaded everywhere. For example, say you have two pages that have a similiar button. You want those two buttons to behave differently even though they look the same. If all JS is loaded all the time, then the browser will not know which JS should be applied to each button. In the end, it's generally better to control which JS is being loaded for each page. Additionally, the files will be loaded in alphabetical order. Often, external libraries will have a dependency on another JS file being loaded before it, and all your **JavaScript will error out if this load order is not honored**. Given that things load in alphabetical order, it's unlikely things will magically be loaded in the right order. 

Check the console in your browser to see if you are getting these types of errors, and, if so, manually require each file in the order you need it to be loaded and get rid of `require tree`.

One last thing to remember: when you require something in your manifest file, the path you provide must be the asset path. If you have the file `comments.js` in the folder `/assets/javascripts/blog`, you would need to use `//= require blog/comments` to include it in our manifest file.

### Loading a Manifest File in your layout
Now that we have our manifest file, we need to include it in our application. The only thing we need to do is use the `javascript_include_tag` in our application layout file. This is just a Rails helper that generates a script tag instructing the browser to load that JS file.

**File: app/views/layouts/application.html.slim**
```erb
<%= javascript_include_tag "application" %>
```

Sprockets will then take care of loading our manifest file and determining which assets to load. If we are in development mode, each asset will be loaded individually.

```html
<script src="assets/jquery.min.js" />
<script src="assets/bootstrap.min.js" />
<script src="assets/blogs.js" />
```

In production mode, all the assets will be combined into a single file.

```html
<script src="/assets/application-908e25f4bf641868d8683022a5b62f54.js" />
```

## Page Specific Javascript
As we create our application, using a single file for all of our JS can cause a lot of headaches. As the file gets bigger and bigger it becomes harder to find anything. We want to organize our JS into different files based on their functionality. One way to do this is to have page-specific JS files.

### Page-Specific JS in a manifest
By default, when you use the [Rails Generators](http://guides.rubyonrails.org/generators.html) to create a new resource, Rails will create page-specific JS and CSS files. This is where we want to put any JS that is specific to the web pages associated with the resource. For example, say we have a Blog resource. We add JS that loads the comments for a blog after the page loads. This bit of JS should be put in the file `app/assets/javascripts/blogs.js.coffee` ([CoffeeScript](http://coffeescript.org) is the default way to create JS for Rails). From here we just need to make sure our JS file is included in the JS manifest.

**File: `app/assets/javascripts/application.js`**
```javascript
//= require blogs
```

### Controller Specific JS
When the browser loads our JavaScript, it parses the entire file and runs it. With a big application this can be a lot of JS. Different pages might start to have functionality we don't want to share accross the application. An option to allow for a page-specific JS file to be loaded only with the pages we want is to use the name of the controller.

```erb
<%= javascript_include_tag params[:controller] %>
```

Instead of adding blogs.js to the manifest file, we could instead load the file based off of the controller's name.  We can place this in the head or below the body of our layout and it will load the JS file that matches the name of the controller.

A request made to `blogs#index` would result in params:
`{controller: 'blogs', action: 'index'}` and the `javascript_include_tag` will now load the blogs.js file.  If we visit a page from a different controller, the `javascript_include_tag` will include the JS specific to that controller.

The downside of this is we'd no longer be getting the benefits of asset concatenation or caching.  The browser will have to make a separate request for this file in addition to the request for the main concatenated `application.js` file.  The benefit of this strategy could be that we're less likely to invalidate the cache for our entire JS file if it is in pieces.

### Using content_for

The final way to include page-specific JS is to use a `content_for :js` block in your layout.  We can either put this in the head or below the body of the layout.

**File: app/views/layouts/application.html.erb**

```erb
<head>
  <meta>
  <%= javascript_include_tag 'application' %>
  <%= yield :js %>
```

Then in your view

**File: app/views/blogs/show.html.erb**

```erb
<% content_for :js do %>
  <script>
    alert('Some page-specific JS');
  </script>
<% end %>
```

Anything placed in our script tag will run only on our show page.

### Class Based Targeting
While both of those solutions work, some people find that they aren't the most elegant solutions to the problem.  One way you can continue to keep things simple with the asset pipeline is to continue to have it concatenate all of your javascript together, but wrap all your pages in a div with a specific class for that page.

```erb
<%# app/views/layouts/application.html.erb %>

<body class="<%= controller_name %> <%= action_name %>">
  <%= yield %>
</body>
```

Assuming your `contact` page action was inside a controller named PagesController, the rendered result would be the following:

```erb
<body class="pages contact">
  ...
</body>
```

Now you could write some javascript like this
```javascript
$(".pages.contact").click(function() {
  console.log("only runs on the contact page!")
})
```
## Resources
* [Rails Guides Primer](https://github.com/learn-co-curriculum/what-is-the-asset-pipeline/edit/master/README.md) 

## External Javascript Libraries
Loading JavaScript into our application can be done many different ways in Rails. We can use HTML script tags for JS files located on another server. We can place third party JS files in our vendor folder. Finally we can even use gems to load the JS libraries we need.

### External JavaScript
Not all of our JavaScript files will be loaded from our application directories. Sometimes we may want to load JS from a CDN. This can allow us to save on bandwidth and if we have a CDN setup for our application, it will help with file download speeds for users throughout the world. To load JS like this, we create HTML script tags in our application layout file.

```html
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" />
```

### Vendor Assets
Our applications will grow as we add more and more features. We might end up with a lot of different JS libraries along the way. The problem is all of these external JS files will need to be downloaded one by one and this will slow page load times. **At some point, we will probably decide to make these external JS files to be internal JS files.**

We don't really want to clutter our main `assets/javascripts` folder with code written and maintained by other people. This is why Rails provides the `vendor/assets/javascripts` folder. We can place third party JS libraries in here and add them to our JS manifest file. We also get the added benefit of having all of these external JS files combined into one file with all our application JS files. One thing to consider when doing this is if you are using a popular library, a user has probably already downloaded this JS file from another site they visited and is sitting in their cache. By adding this to your manifest you'd be forcing them to download the code again.

### Gems
Manually adding JS to our vendor directory can be cumbersome and hard to maintain. New versions of the JS files come out and unless we're keeping track of every release, it's easy to fall behind. Luckily, many of the popular JS libraries we use have gems. These gems package the JS files and when installed, add them to our asset path allowing us to require the JS inside of our manifest file.

For example, let's install the jQuery gem in our Gemfile. Once we `bundle install`, we are able to add `//= require jquery` to our manifest file. Now jQuery will be loaded by Rails. Plus, we can use bundler to update jQuery when new versions are released. Handling updating is particularly helpful for bigger JS frameworks that might have many JS files and dependencies. This makes our life simpler, but it also may cause problems. It's possible we don't want to update to the newest version of every JS file. The API of the library could have changed causing our code to break. It also is going to require the user to re-download the JS libary when there may be no need to. We also (as in the previous section) would be concatenating our JS libraries together which might lose the benefits of caching those libraries.

## CSS Manifests
CSS makes our web applications look good but can be hard to manage. As our application grows, so do the amount of stylesheets we need to manage. The Asset Pipeline can help us manage this chaos much like with JavaScript.

Like our JavaScript manifest, the CSS manifest has a special syntax that differentiates it from a regular CSS file. The `*= require` directive is very similar to its JS counterpart. We are just using a CSS comment instead of a JS comment.
```
/*
*= require main
*/
```

_Note_: In a CSS manifest file you must open the CSS comment block with `/*` and then each directive appears on an individual line starting with `*=`. You close the comment block with `*/`. It's a little different than the JS directive comments of `//=`, which are individual, valid JS comments.

### `require` directives
When we require a CSS asset in our manifest, if it is located in one of the configured folders, it will be included in our application. One thing to remember is when you require something the path you provide must be the asset path. For example, if you have the file `app/assets/stylesheets/blogs/main.css` you will need to require it like this, `*= require 'blogs/main'`.

### Loading a Manifest File in your layout
Loading our CSS manifest file into our application is just as easy as it was with JS. We create a `stylesheet_link_tag` in our application layout, and Sprockets takes care of loading the manifest and determining which assets to include.

```erb
<%= stylesheet_link_tag 'application' %>
```

In development mode, each CSS file will get its own link tag. This allows for easier debugging. A manifest file that looks like this:

```css
/*
*= require main
*= require blogs
*= require posts
*/
```

Would create this in our application layout's head tag (assuming fingerprinting/digests are turned off):

```html
  <link rel="stylesheet" href="/assets/main.css" /> 
  <link rel="stylesheet" href="/assets/blogs.css" /> 
  <link rel="stylesheet" href="/assets/posts.css" /> 
```

#### Manifests in Production
In production mode, Sprockets will take all of our CSS files and create one large CSS file. It will also [minify](https://developers.google.com/speed/docs/insights/MinifyResources) the contents, removing unneeded whitespace to reduce the overall size of the file. Both of these things will speed up page load times for our users. If we look at our previous manifest example:

```css
/*
*= require main
*= require blogs
*= require posts
*/
```

We would only get a single link tag from this in our application manifest.
```html
<link rel="stylesheet" href="/assets/application-4dd5b109ee3439da54f5bdfd78a80473.css" /> 
```
The CSS in this file would look like this:
```css
body{background-color:#FFF;font-size:14px;margin:0}a{color:#1B97F2;text-decoration:none}.clear{clear:both}ul{margin:4px 0;padding-left:17px}ul.horizontal{list-style:none;margin:0;padding:0}ul.horizontal li{margin:0;padding:0;float:left}#flash_notice,#flash_alert{padding:10px 0;text-align:center;color:#FFF}
```
Notice the lack of whitespace? That's the minification we talked about earlier. This is great for production use, but it's a bit hard to read. This is why when we are in development mode we get individual, unminified files instead.

- https://github.com/rails/sprockets#sprockets-directives
- http://guides.rubyonrails.org/asset_pipeline.html

## External CSS Frameworks in Rails
Loading stylesheets into our application can be done a few different ways in Rails. We can use HTML link tags for CSS located on another server. We can place third party CSS files in our vendor folder.  We can also use gems to load the CSS frameworks we need like Twitter Bootstrap.

### External Stylesheets
So far we have been loading CSS from our asset directories. You aren't required to do it this way, but it does allow us to easily manage our CSS. The other way to load CSS is by using standard HTML `<style>` tags placed in the `<head>` tag of our application layout. There are benefits to using this approach. We can load stylesheets from other peoples CDNs without having to set one up ourselves. This can save us bandwidth and help with download speeds for users throughout the world. To load CSS like this, we create HTML link tags in the `<head>` of our application layout file.

```html
<head>
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" />
</head>
```

### Vendor Assets
Managing a lot of different third party CSS files can be hard. We may lose track which files are for which CSS frameworks. Also, separate link tags mean these files will need to be downloaded one by one on the browser, and this will slow page load times. At some point, we will probably decide to have these external CSS files be internal CSS files.

We don't really want to clutter our main `assets/stylesheets` folder with CSS files maintained by others. Rails provides the `vendor/assets/stylesheets` folder for these types of CSS files. We can place our third party CSS frameworks in here and add them to our CSS manifest file. We get the added benefit of having all external CSS files combined into one file with all our application CSS files.

### Gems
Manually adding CSS to our vendor directory can also be cumbersome and hard to maintain. New versions of CSS frameworks are released, and it's easy to fall behind. Luckily for us, many of the popular CSS frameworks have gems. These gems package up these CSS frameworks and, when installed, add them to our asset path, allowing us to require them inside of our CSS manifest file.

To install the Twitter Bootstrap gem in our Gemfile, you will need to add `gem "bootstrap-sass"` to your Gemfile and run `bundle install`. Once that completes, you are able to add `*= require bootstrap` to the CSS manifest file. Now, Bootstrap will be loaded by Rails. We can also use bundler to update Twitter Bootstrap when new versions are released. Handling updating is particular helpful for bigger CSS frameworks like Bootstrap that have many CSS files.

- [Bootstrap SASS Gem](https://github.com/twbs/bootstrap-sass)

## Asset Preprocessors in Rails
The Asset Pipeline is very powerful. We have seen this with just asset concatentation alone. Imagine having to manually combine dozens of JS and CSS files and update the layouts every time we want to make a change and deploy our application. The Asset Pipeline does this for us. On top of that, it also allows us to use preprocessors like [Coffeescript](http://coffeescript.org/), [SASS](http://sass-lang.com/), and ERB. But what is a preprocessor?

CSS and JavaScript have been around awhile. They do a good job styling our pages and creating cool client-side behavior. They aren't always the easiest to work with, though. There have been a lot of improvements to these languages, but because those upgrades require people to upgrade their browsers, we can't always use them. Wouldn't it be great if we could write CSS and JS the way we wanted and have a program turn it into the correct syntax? This is why we have preprocessors.

Because the asset pipeline parses all of our asset files, we are able to write CSS and JS in a language that isn't exactly CSS and JS. By using file extensions, the asset pipeline can recognize these files as not normal CSS and JS and run them through a preprocessor, which compiles to valid CSS and JS. If the Asset Pipeline sees `main.js.coffee`, it knows this is a Coffeescript file and will run it through the Coffeescript preprocessor. The asset pipeline runs through each file extension in reverse order, running the preprocessor for that extension. For example, a file that ends in .js.erb will have the erb preprocessor run on it first, the goal being a file that is purely valid JavaScript. You can use Ruby to programatically write JS and even use variables from your controller in your JS!

In development mode, the asset pipeline will run the preprocessor on any file that needs to be processed every time it's requested. This is SLOW! **In production, you might have had to run `rake assets:precompile`. This runs the preprocessors once, outputs all the now-static JS and CSS, and then allows the webserver to serve them without ever touching Rails.** This is much quicker!

### Coffeescript
If you have written any JavaScript, you know it has many quirks. For example, the difference between `==` vs `===` when testing equality, or how to use [Inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain). Coffeescript takes all of these things that make using JavaScript hard and abstracts them away. What we are left with is a very Ruby-like syntax. Take the following examples pulled from the Coffeescript documentation.

```coffeescript
launch() if ignition is on

volume = 10 if band isnt SpinalTap

letTheWildRumpusBegin() unless answer is no

if car.speed < limit then accelerate()

winner = yes if pick in [47, 92, 13]

print inspect "My name is #{@name}"
```

If you look closely, you can see this syntax looks a lot like Ruby. Once we run it through the preproccesor, we get the following:
```javascript
var volume, winner;

if (ignition === true) {
  launch();
}

if (band !== SpinalTap) {
  volume = 10;
}

if (answer !== false) {
  letTheWildRumpusBegin();
}

if (car.speed < limit) {
  accelerate();
}

if (pick === 47 || pick === 92 || pick === 13) {
  winner = true;
}

print(inspect("My name is " + this.name));
```

That's plain old JS with all of its quirks, but we didn't have to write it.

### SASS
While CSS might not be as complex as JavaScript, it still has its problems. Without language features like selector nesting, basic inheritance, and variables, it can be hard to keep CSS manageable. CSS3 promises to provide much needed enhancements to the aging CSS 2.1 standard, but, just like JS, we can only use these new features once a majority of people upgrade to supported browsers. This is where SASS comes in. SASS is a sort of enhanced CSS that, once run through the preprocessor, turns into plain CSS.

Here is a simple example of the nesting that SASS provides.

```css
.error {
  a {
    color: red;
    text-decoration: none;
  }
  span {
    color: #ff1a1a;
    text-decoration: underline;
  }
}
```

The code above will turn into the following once run through the preprocessor:

```css
.error a {
  color: red;
  text-decoration: none;
}
.error span {
  color: #ff1a1a;
  text-decoration: underline;
}
```

Notice how we avoided having to repeat `.error`. In SASS, if we need to change `.error` to `.error-text`, we only have to change it in one place. In big applications, that can save us from making a lot of mistakes.

## Images
Most web applications have images. Just like JavaScript and Stylesheets, the Asset Pipeline makes dealing with images easier. The Asset Pipeline helps us build paths and deal with caching problems.

Where do we put our images so the Asset Pipeline can find them? We can put them in the `app/assets/images` directory. By default, this path is already included in the asset paths. It's possible to add more image paths to the asset paths by adding to `Rails.application.assets.paths`. Once the additional path has been added, we can access it using the standard `/assets` url. The cool thing here is we can use one URL path that maps to a bunch of different folders. For example the URL "/assets/logo.png" will look in all the folders in the asset paths for that image, but no matter where it's located its URL never changes.

If you pop open the console and type in `Rails.application.assets.paths` you'll see an array of the paths Rails considers to be asset paths. So technically, you could add an image to `assets/stylesheets` and Rails would find it, but that would be confusing to your coworkers. Because it's an array, you can easily shovel in more directories if you have images in places not on the list. In your `application.rb` you could do something like this.

```ruby
#application.rb
config.assets.paths << Rails.root.join("lib", "myrandomimagefolder")
```

### `asset_path` Helper
The first of the helpers available to help us create image paths is the `asset_path` helper. It provides a way for us to get a relative path to an image file in the Asset Path. To use this helper, we specify the path relative to the assets folder.

```ruby
asset_path('logo.png')
```

If the asset pipeline finds the file, it will return the path to the
image.

```
/assets/logo.png
```

If the file is in a sub-folder in the images directory (images/banner), we need to include that as well.

```ruby
asset_path('banner/logo.png')
```

```
/assets/banner/logo.png
```

If the asset pipeline can't find the file, the path returned will be `/` plus the file name.

```
/logo.png
```

The `asset_path` helper is great for image paths located in our CSS files.

```css
.logo {
  background-image: url(<%= asset_path('logo') %>);
}
```

### `image_tag` Helper
Most of the time we don't only care what the URL is for an image, we want to generate an HTML tag that will actually display the image for a user.
The `image_tag` helper creates an `<img>` tag with the path to the image.

```html
<%= image_tag "logo.png" %>
```

Will create:

```html
<img src="/assets/logo.png" />
```

If the asset pipeline can't find the file, an image tag will be created with the name of the file plus the '/' path.

```html
<img src="/logo.png" />
```

Since this image doesn't exist we will see a broken image on our web page.

### Fingerprinting
Just like with our JS and CSS, browser caching can cause problems when we decide to update an image. When we use either `asset_path` or `image_tag` to display our images, the Asset Pipeline will add a fingerprint to the file. This will only happen if we are running our application with fingerprinting on. By default it's on in both production and development. If you want to turn it off you can go to the environment file `config/environment/development.rb` and change `config.assets.digest = false`.

If we were to look at one of these paths, it would look like this:

```
/assets/logo-331238805bdaebb4b05e9385bc1261f8.png
```

A digest is appended to the file name. If the file is updated, the digest will change. We won't have to worry about users having old versions of image files when we make changes.

### Image Tags
It is possible to predict the path to a image file and create our own image tag. This is not recommended since it will limit your ability to move your file and avoid caching problems that the helpers fix. ALWAYS USE THE HELPERS.

## Bootstrap and the asset Pipeline
Starting a web application can be overwhelming. There are so many decisions to make that it can be hard to manage. The last thing we want is to make a great application that looks terrible and doesn't respond to well to different screen sizes. It would be great if we could just add something to our application that made structuring our UI as easy as Rails makes everything else. [Twitter Bootstrap](http://getbootstrap.com/) does just that. It's a grid-based CSS framework that provides the ability to create great looking, responsive websites.

[Twitter Bootstrap](http://getbootstrap.com/) has some really good documentation as well. If you browse to their site and click on [CSS](http://getbootstrap.com/css/) in the menu, you will see how to structure your HTML and CSS in a way that allows for an easy, responsive UI.

If you click on [Components](http://getbootstrap.com/components/), you will see how to use all the reusable components that Bootstrap provides. These are common parts of websites like buttons, dropdowns and navigation.

Finally, if you click on [JavaScript](http://getbootstrap.com/javascript/), you will see how to use Bootstrap's JavaScript component to enhance our application. It makes creating modals and collapsing menus super easy.

When including Bootstrap in our application, we will most likely want to include the whole framework. This can be done a few different ways.

This first option, which is not preferred because it doesn't take advantage of the asset pipeline, is to use a `<link>` tag in the `<head>`.

```html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" integrity="sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7" crossorigin="anonymous">
```

and a `<script>` tag at the bottom of the `<body>` tag.

```html
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" integrity="sha384-0mSbJDEHialfmuBBQP6A4Qrprq5OVfW37PRR3j5ELqxss1yVqOtnepnHVP9aJ7xS" crossorigin="anonymous"></script>
</body>
```

Our preferred way is to use a gem. To do this, include the following in our Gemfile:

```ruby
gem 'bootstrap-sass'

```

Now run `bundle install`. Finally, include the following in your CSS manifest file:

```css
@import "bootstrap-sprockets";
@import "bootstrap";
```

And the following in your JavaScript manifest file.

```JavaScript
//= require bootstrap-sprockets
```

That's it! Now we have Boostrap ready to go, and any additional updates only require a `bundle update`.

### Using Bootstrap
Let's say you've created an amazing design for your web application. You tweaked the margins and padding just right, and the content aligned just the way you want. The whole time you've had your browser in full screen so you decide to shrink your browser window to see what happens only to discover that your awesome design doesn't look so awesome now. Next, you take out your phone and browse to your application. To your dismay, your website is even worse on your phone. It's hard to read and requires a ton of scrolling and zooming to see anything.

To solve our problem, we could create completely new designs for each screen size, but this can easily become hard to manage. Every time you add a new feature, it has to be added in multiple places and customized for that specific layout. This is the problem that [Responsive Web Design](https://en.wikipedia.org/wiki/Responsive_web_design) aims to fix. By using [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries) and careful design, we can create a website that changes depending on the screen size. Twitter Bootstrap helps us manage the complexities of responsive web design by providing a grid system to work with.

#### Grid Layout
Bootstrap's grid system divides the screen into rows with 12 columns. These columns contain our content. We can choose how to lay out our site by creating 12 individual columns or fewer bigger columns. For example, here is a grid divided into 12 columns:

```html
<body>
  <div class="container">
    <div class="row">
      <div class="col-lg-1">
        <p>Column #1</p>
      </div>
      <div class="col-lg-1">
        <p>Column #2</p>
      </div>
      <div class="col-lg-1">
        <p>Column #3</p>
      </div>
      <!-- ... columns 4 through 11 ... -->
      <div class="col-lg-1">
        <p>Column #12</p>
      </div>
    </div>
  </div>
</body>
```

Each column would be very narrow and probably not able to fit much content. If we want, we could also just have two bigger columns:

```html
<body>
  <div class="container">
    <div class="row">
      <div class="col-lg-6">
        <p>Column #1</p>
      </div>
      <div class="col-lg-6">
        <p>Column #2</p>
      </div>
    </div>
  </div>
</body>
```
Notice how instead of 12 `<div>` tags with the CSS class `col-lg-1` we use 2 `<div>` tags with the CSS class `col-lg-6`. The combined total of these two columns is still 12. This is an important aspect of Bootstrap. We should always be able to add the columns together to get a total of 12.

From here, there are a lot of different ways we could configure our grid to respond to different screen sizes in different ways. Refer back to the documentation and examples to help.

#### References
- [Twitter Bootstrap](http://getbootstrap.com/)
- [Responive Web Design](https://en.wikipedia.org/wiki/Responsive_web_design)
- [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)

## Gems selection
We've been using gems like crazy at this point, from RSpec for testing to Rails for ... Rails-ing. Almost any general problem we need to solve has already been solved by someone else and released as a gem, allowing Ruby and Rails developers to reach incredible levels of productivity because we get to skip all that wheel-reinventing that Java programmers do all day.

**//Flat-fact:** Reinventing wheels is actually what Java programmers do as a side hustle while they're waiting for compiles.

Working in Ruby is such a joy not just because of the language itself, but also because of the people who freely give back to their fellow developers in the form of gems.

The wealth of gems available for everyone to use in their projects is one of the things that sets Ruby apart from other languages, but it can also lead to *Gem Madness* - a condition where gem-crazed developers look for a gem to solve every problem without considering if a gem is really needed. Before we go looking for a gem to solve a problem, we should take a little time to decide if the problem we want to solve is really so big that we need a gem, or if we might be able to figure out (or Google) a way to just write the code ourselves.

We can actually look for gems right on our command line. Try this out in your console:

`gem search ^twitter$ -d`

The `gem search` command can take a regular expression and can be very handy if you know the name (or part of the name) of a gem and want to find it quickly.

[Ruby Toolbox](https://www.ruby-toolbox.com/) is a site that aggregates and categorizes gems, and ranks them according to stats like total downloads, releases, and active commits.

Ruby Toolbox is *comprehensive*, which is a fancy way of saying "it's kind of a lot." Great if you're looking for something a little more obscure or really want to explore all the options, but sometimes we want a more curated list of which gems are popular in the community.

Enter [Awesome Ruby](https://github.com/markets/awesome-ruby) and [Awesome Rails Gems](https://github.com/hothero/awesome-rails-gem), two open-source, community-maintained lists of the most popular gems for Ruby and Rails by category.

### Qualities To Look For When Choosing A Gem
Adding a gem to your project means you are taking on a *dependency* on outside code. If that gem has security problems, your project has security problems. If that gem has bugs, your project has bugs. If that gem (or one of the gems it depends on) doesn't keep up with newer versions of Rails, you might be stuck dealing with compatibility problems.

So let's look at some strategies for selecting high quality, reliable, and safe gems for our project.

This is one time when we want to care about popularity contests. The more popular a gem is, the more likely it is to be maintained, to keep up with security issues and new Rails versions.

Beyond that, the more popular a gem is, the more likely you are to be able to find help if you run into problems. Popular gems have a lot of questions and answers on StackOverflow. People like to blog about popular gems so they can get those sweet, sweet clicks. All that adds up to you feeling confident that you can use that gem.

Some ways to test the popularity are:

1. Look it up on Ruby Toolbox. See how many downloads there are, how recently it's been committed to, and how many versions there are.
2. See if it's on the Awesome Ruby or Awesome Rails lists mentioned above.
3. Google the gem name and see what's there and, importantly, how recent it is. Try things like "gem name tutorials" and "gem name errors."
4. Search StackOverflow for the gem and read what people say about it.
5. Seach GitHub for usage of the gem. You'll need to read the gem's documentation or code and pick out a class or method to search for. For example, searching GitHub for `has_attached_file`, which is a primary method in the [Paperclip](https://github.com/thoughtbot/paperclip) gem, yields over 100K results, so it's obviously in wide use.

We want our gems to be popular, but we also need them to be well-maintained. A well-maintained gem will be far less likely to cause you problems down the road.

Some ways to determined if a gem is well-maintained are:

1. Read the code. Is it put together in the way you'd expect? Can you understand it? Does it follow Ruby idioms and styles?
2. Look for documentation. Is it well-documented? The README, at a bare minimum, should tell you how to get the gem up and running. Even better is a wiki with examples and sample code.
3. Look at issues and pull requests. How long do they stay open? How active are the committers in addressing issues and PRs? Are there problems with a specific version that haven't been resolved? **Hint:** This could also be an opportunity to do some open source contributing!
4. Look for tests. Is there a `/test` or `/spec` directory? How many tests are there? Is there a link to a continuous integration build result? Examining the tests is always a great way to learn how the gem works and how well it's put together.

## Paperclip & ImageMagick
In our blog application, we want to be able to show the author's headshot, so we're going to turn to the popular gem [Paperclip](https://github.com/thoughtbot/paperclip) to help us out. We could certainly write our own code to handle the image uploads, but as we'll see, Paperclip offers so much in terms of integrating with our ActiveRecord models, configuring storage options, and processing images after upload.

To set up Paperclip, first we need to install the [ImageMagick](http://www.imagemagick.org/script/index.php) dependency. Paperclip uses ImageMagick to resize images after upload.

On OS X, the preferred way to install ImageMagick is with [Homebrew](http://brew.sh/):

`brew install imagemagick`

Once we have ImageMagick, let's add Paperclip to our Gemfile:

```ruby
# Gemfile

#...
gem "paperclip"
```

Run `bundle install` to finish it up. If your Rails server is already running, remember to restart it so that it has access to the new gem.

### Setting up the upload
Now that we have Paperclip installed, let's jump right into adding avatar images to our `Author` model.

First, we need to wire up our model to use Paperclip's `has_attached_file` method, and tell it what attribute name we want to use to access the attached file. In this case, we'll go with `avatar`.

```ruby
# models/author.rb

class Author < ActiveRecord::Base
  has_many :posts
  has_attached_file :avatar
  validates_attachment_content_type :avatar, content_type: /\Aimage\/.*\z/
end
 ```

The `validates_attachment_content_type` validator is provided by Paperclip, and ensures that we get an image file when we expect one. This validator is required by default.

Now we need to add the `avatar` field to our table via a migration. Paperclip provides a generator for us, so we can run:

`rails g paperclip author avatar`

This will generate a migration that looks something like:

```ruby
class AddAttachmentAvatarToAuthors < ActiveRecord::Migration
  def self.up
    change_table :authors do |t|
      t.attachment :avatar
    end
  end

  def self.down
    remove_attachment :authors, :avatar
  end
end
```

**Note:** Paperclip provides two new methods for use in migrations: `add_attachment` and `remove_attachment`. Because these are custom attachment methods, Rails won't know how to automatically reverse an `add_attachment` migration, so you need `up` and `down` methods in the migration rather than simply using `change`.

Run `rake db:migrate` to add the column.

Now we need to set up the form view to give us a way to upload an avatar. Let's add that to the author form partial:

```erb
# views/authors/_form.html.erb

<%= form_for @author, html: {multipart: true} do |f| %>
  <%= f.label :avatar %>
  <%= f.file_field :avatar %><br>
  <%= f.label :name %>
  <%= f.text_field :name %><br>
  <%= f.label :bio %>
  <%= f.text_area :bio %>
  <%= f.submit %>
<% end %>
```

Adding `html: { multipart: true }` as a parameter to `form_for` will generate a form that knows it will be submitting both text and binary data to the server. Then we use the `file_field` helper to generate the appropriate input and "choose" button to attach the file.

Finally, we need to update our strong params in the controller to allow the new `avatar` field to be used for mass assignment:

```ruby
# authors_controller.rb

# ...

  private

  def author_params
    params.require(:author).permit(:bio, :name, :avatar)
  end
```

Now if we run our server with `rails s` and browse to `/authors/new`, we should be able to create an author with an avatar.

### Displaying the avatar
We've attached the avatar, but now we need to display it to the user. Here's where Paperclip really starts to shine.

Let's add the avatar to our author `show` view:

```erb
# views/authors/show.html.erb

<h1><%= @author.name %></h1>
<%= image_tag @author.avatar.url %>
<p><%= @author.bio %></p>
<p>Posts:</p>
# ...
```

Simple as that! Paperclip provides the `url` method on our `avatar` so that no matter where it's stored, we can always use it in an `image_tag`. Now we're starting to see why using Paperclip is more efficient than doing it ourselves.

Let's also add the avatar to the author `index` view, so we can see them in the list.

```erb
# views/authors/index.html.erb

#...
  <% @authors.each do |author| %>
    <li>
    <%= image_tag author.avatar.url %>
    <%= link_to author.name, author_path(author) %><br>
    <%= author.bio %>
    </li>
  <% end %>
#...
```

### Setting Default Avatars
In another stroke of awesomeness, Paperclip gives us an elegant way of dealing with missing avatars that doesn't involve us having to go into every view that displays an avatar and write a bunch of logic.

Let's go back to our model and tell `has_attached_file` what the default is when no file has been attached:

```ruby
# author.rb

class Author < ActiveRecord::Base
  has_many :posts
  has_attached_file :avatar, default_url: ':style/default.png'
  validates_attachment_content_type :avatar, content_type: /\Aimage\/.*\z/
end
```

You can store default images to match each style, e.g. in this case we would have a thumbnail default as well as an original default. You just need the supporting folders in your `app/assets/images` directory to match the style names ("original" is the default, unprocessed style).

**Note:** We have to place an image named `default.png` in our `app/assets/images/thumb` and `app/assets/images/original` folders (already provided), that part doesn't automatically happen via Paperclip (Image)Magick.

Reload `/authors`. No more broken images!

### Generating Thumbnails With Paperclip
Depending on how big an author's photo is, our `index` page might have wildly different-sized images in the list.

We can easily constrain those with CSS, but what we really want to do is create a thumbnail to be used in the list so we aren't loading a bunch of giant images and slowing things down.

```ruby
# author.rb

class Author < ActiveRecord::Base
  has_many :posts
  has_attached_file :avatar, default_url: ':style/default.png', styles: { thumb: "100x100>" }
  validates_attachment_content_type :avatar, content_type: /\Aimage\/.*\z/
end
```

And just like that, Paperclip will process the image and create a thumbnail style for us. To use it in the view, simply pass the symbol for the style to the `url` method:

```erb
# views/authors/index.html.erb

#...
  <% @authors.each do |author| %>
    <li>
    <% image_tag author.avatar.url(:thumb) %>
    <%= link_to author.name, author_path(author) %><br>
    <%= author.bio %>
    </li>
  <% end %>
#...
```

If we go to `/authors/new` and create an author, then return to `/authors`, we'll see that the thumbnail has been generated for the new author, but now one or more of our existing author's thumbnails is a broken image.

That's because we added images to those authors before we defined the `:thumb` style, so we need to tell Paperclip to update our existing images with the new style. Fortunately, Paperclip provides a rake task for exactly
that.

In the terminal, run `rake paperclip:refresh:missing_styles`, then refresh that `/authors` page and we should have thumbnails for everyone.

## Kaminari
We're going to be using the Kaminari gem to paginate our blog posts so that each page is more manageable.

Kaminari is a Japanese word meaning "thunder", and the obvious link between that and this lesson is that the [Kaminari](https://github.com/amatsuda/kaminari) gem makes pagination a
slam dunk.

To get started, we'll add Kaminari to our Gemfile:

`gem 'kaminari'`

Then run `bundle install` to get it installed.

Now let's run `rails g kaminari:config` to generate a configuration file for Kaminari. You can find the file in `config/initializers`. Let's look inside:

```ruby
# config\initializers\kaminari_config.rb

Kaminari.configure do |config|
  # config.default_per_page = 25
  # config.max_per_page = nil
  # config.window = 4
  # config.outer_window = 0
  # config.left = 0
  # config.right = 0
  # config.page_method_name = :page
  # config.param_name = :page
end
```

These are the defaults. The one we're interested in is `default_per_page`, which sets up how many results will be in each page. Let's change that to 10 (don't forget to un-comment it).


```ruby
# config\initializers\kaminari_config.rb

Kaminari.configure do |config|
  config.default_per_page = 10
# ...
```
Restart your Rails server to make sure the new gem and initializer are picked up.

Okay. Now let's get into the controller and set our query up to use Kaminari.

```ruby
# controllers\posts_controller.rb

# ...
  def index
    @posts = Post.order(created_at: :desc).page(params[:page])
  end
# ...
```

Here we're getting posts by most recent and then using the `page` method of Kaminari to get a "page" (ten, in our case) of results. We're passing `params[:page]` to the `page` method so that we can control *which* page we get. And if `params[:page]` is `nil`, we'll get the first page, so it works by default.

If you reload your `/posts` page, you should see that we're now limited to ten results, which is much more manageable!

But how do we get to the next page of results?

Kaminari provides us with plenty of helpers to output navigation controls. Let's start by adding regular pagination controls with the `paginate` helper:

```erb
# views\posts\index.html.erb

<h1>Blog Posts!</h1>
<%= paginate @posts %>

<% @posts.each do |post| %>
# ...
```

Reload the page and we now have the ability to go next, previous, first, last, or by page number.

If we want to add a little more contextual information, we can use the `page_entries_info` helper, like this:

```erb
<h1>Blog Posts!</h1>
<%= page_entries_info @posts %>
<%= paginate @posts %>
<% @posts.each do |post| %>
# ...
```

And we can further customize our display, like, displaying only two page numbers on either side of the current page by passing in a value for `window`:

```erb
<h1>Blog Posts!</h1>
<%= page_entries_info @posts %>
<%= paginate @posts, window: 2 %>
<% @posts.each do |post| %>
# ...
```

More customization options for `paginate` can be found in the Kaminari [README](https://github.com/amatsuda/kaminari).

## ActiveAdmin



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

# Consuming APIs
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

## APIs and Faraday
Let's use the [search](https://developer.foursquare.com/docs/venues/search) venue API to find a place to grab some coffee. Read through the documentation and use Postman to construct an API call to find coffee shops near you.

We want to pass parameters of `near` and `query`, along with our `client_secret`, `client_id`, and `v` parameters.

If we did it right, we should get a JSON response with an array of `venues`, each of which should conform to the [venue](https://developer.foursquare.com/docs/responses/venue) documentation.

We're going to use [Faraday](https://github.com/lostisland/faraday) to access the Foursquare API from our Rails application. The basic app is set up, and you can code along as we add the feature to search for nearby coffee shops.

Faraday is an HTTP client library that abstracts and standardizes some lower-level HTTP functions and makes it easy to build requests and get responses from an API.

**Advanced:** In this lesson, and indeed in most cases, you'll use the built-in [Net::HTTP](http://ruby-doc.org/stdlib-2.3.0/libdoc/net/http/rdoc/Net/HTTP.html) library for HTTP functions. However, other libraries offer different advantages in categories like performance ([Patron](http://toland.github.io/patron/)) or multi-threading capability ([Typhoeus](https://github.com/typhoeus/typhoeus#readme)). Using a library like Faraday, that wraps and abstracts these lower-level libraries, allows you to maintain the same client code even if you change the underlying library later.

Add Faraday to the `Gemfile` and run `bundle install`:

```ruby
gem 'faraday'
```

Our `searches/search.html.erb` is already set up to post a `:zipcode` param to our `searches_controller`, so let's get in there and add the Foursquare API call with Faraday:

```ruby
# searches_controller.rb
  def foursquare
    Faraday.get 'https://api.foursquare.com/v2/venues/search' do |req|
      req.params['client_id'] = client_id
      req.params['client_secret'] = client_secret
      req.params['v'] = '20160201'
      req.params['near'] = params[:zipcode]
      req.params['query'] = 'coffee shop'
    end
    render 'search'
  end
```

Let's break this down. We use `Faraday.get(url)` to make a `request` to the API endpoint.

We know we need to set some `params` from our tests in Postman, and Faraday gives us an easy way to do this by passing a block to the `get` method and adding any parameters we need through the request object via a hash of `params`, very similar to the way we use params in Rails.

To keep it simple, we're just going to render the `search` template again with the result.

**Hint:** Remember to replace `client_id` and `client_secret` with your own ID and secret!

Let's run our Rails server and navigate to `/search`, and make a search!

Okay. Anticlimactic.

![jess welp](http://i.giphy.com/5q2TF9Kz4g6iI.gif)

### The Response Object

When we're working with Faraday, any time we make a *request* (such as `Faraday.get`), the returned object represents a *response*. The two most interesting parts of the response object are the `body`, which is where the server response, such as error messages or JSON results will be, and the `status`, which is the [HTTP code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) the server returns from the request.

So let's make a change and get that response into a variable we can use:

```ruby
# searches_controller.rb
  def foursquare
    @resp = Faraday.get 'https://api.foursquare.com/v2/venues/search' do |req|
      req.params['client_id'] = client_id
      req.params['client_secret'] = client_secret
      req.params['v'] = '20160201'
      req.params['near'] = params[:zipcode]
      req.params['query'] = 'coffee shop'
    end
    render 'search'
  end
```

And in our template, let's take a look at that response `body`.

```erb
# views/searches/search.html.erb
<h1>Search for Coffee Shops Near Me</h1>
<%= form_tag '/search' do %>
  <%= label_tag :zipcode %>
  <%= text_field_tag :zipcode %>
  <%= submit_tag "Search!" %>
<% end %>
<div>
  <%= @resp.body%>
</div>
```

Let's run another search and see what we get.

Great! A lot of JSON! That's progress.

### Parsing The Response

We know we're getting back JSON, and we can see from the [documentation](https://developer.foursquare.com/docs/venues/search) that we can expect a response that includes an array of `venues`.

Looking at this response `body`, we see something like this:

`{"meta":{"code":200,"requestId":"56c0be42498e71008fcc8cc1"},"response":{"venues":[{"`

Okay, we see the `response` node, and we see it has a `venues` node, so that looks right. We also see a `meta` node that gives us some information that's, well, metadata on our request. We'll look at this more later.

So lets get those `venues` out of this JSON object and into a thing we can use in Ruby. We'll do that with the built-in `JSON.parse` method:

```ruby
# searches_controller.rb
# ...
    @resp = Faraday.get 'https://api.foursquare.com/v2/venues/search' do |req|
      req.params['client_id'] = client_id
      req.params['client_secret'] = client_secret
      req.params['v'] = '20160201'
      req.params['near'] = params[:zipcode]
      req.params['query'] = 'coffee shop'
    end

    body_hash = JSON.parse(@resp.body)
    @venues = body_hash["response"]["venues"]
    render 'search'
```

So we parse the `body` of the response into a variable called `body_hash`, which is now a Ruby `Hash`. We can then access the `venues` under the `response` key.

In our `search.html.erb`, we can change `<%= @resp.body %>` to `<%= @venues %>` and re-run our search, and we can see the venues are now represented as an `Array` of `Hash`es that we can further parse.

We'll look at the documentation again and decide what fields we want, and come up with something like this:

```erb
# search.html.erb
# ...
<div>
  <% if @venues %>
    <ul>
    <% @venues.each do |venue| %>
      <li>
        <%= venue["name"] %><br>
        <%= venue["location"]["address"] %><br>
        Checkins: <%= venue["stats"]["checkinsCount"] %>
      </li>
    <% end %>
    </ul>
  <% end %>
</div>
```

We're now successfully using the Foursquare API to get coffee shops near the user!

![schmidt pizza](http://i.giphy.com/OJ8hVSLYbpQ08.gif)

### Handling Errors

Okay, let's search again, but this time don't enter a zipcode. Oof. That's ugly.

Now we could add some validation here and make sure that there's a zipcode, but we can't necessarily prevent every possible error with the API, because we aren't in control of the API.

So we need to understand how the API reports errors, and then handle them.

We said before that the two interesting pieces of the `response` object are the `body`, where all the data is, and the `status`. The status contains the HTTP code.

In most cases, a good API call will result in a status of `200`, which is the HTTP equivalent of the thumbs-up emoji.

If we make this request in Postman without a value in the `near` parameter, we'll get something like this:

```javascript
{
  "meta": {
    "code": 400,
    "errorType": "param_error",
    "errorDetail": "Must provide parameter ll",
    "requestId": "56c0cccd498e39675a357932"
  },
  "response": {}
}
```

And there will be a message that says `Status 400 bad request`. A status code of 400 is the HTTP equivalent of this:

![New Girl Facepalm](http://i.giphy.com/A1UF1VKksVOIo.gif)

So we have a `status` of 400, a `meta` node with some error information, and a `response` node that's empty, which makes sense, because we haven't made a good request.

Using this information, we can add some error handling to our application.

```ruby
# searches_controller.rb
  @resp = Faraday.get 'https://api.foursquare.com/v2/venues/search' do |req|
    req.params['client_id'] = client_id
    req.params['client_secret'] = client_secret
    req.params['v'] = '20160201'
    req.params['near'] = params[:zipcode]
    req.params['query'] = 'coffee shop'
  end

  body = JSON.parse(@resp.body)
  if @resp.success?
    @venues = body["response"]["venues"]
  else
    @error = body["meta"]["errorDetail"]
  end
  render 'search'
```

**Note:** We could have checked for `@resp.status == 200` as well, but Faraday provides us with a nice abstraction in `.success?`, which abstracts the low-level HTTP details into something readable and *intention-revealing*, which is a hallmark of good code.

And in our view, let's handle the possibilities:

```erb
# search.html.erb
# ...
<div>
  <% if @error %>
    <p><%= @error %></p>
  <% elsif @venues %>
    <ul>
    <% @venues.each do |venue| %>
      <li>
        <%= venue["name"] %><br>
        <%= venue["location"]["address"] %><br>
        Checkins: <%= venue["stats"]["checkinsCount"] %>
      </li>
    <% end %>
    </ul>
  <% end %>
</div>
```

Now we're using the API's error response to display what went wrong. We might decide that the default error message is not as friendly as we'd like, and choose to display a friendlier message to our users. That's one of many considerations when using an API - how much to alter the data you receive, but it's also one of the best things about using an API, because you get to make those choices.

**Top-tip:** Remember, not all APIs will structure their responses the same way. Always read the documentation!

### Handling Timeouts

One thing you must always be aware of when accessing resources across the internet on servers out of your control is the possibility of a request timeout.

Request timeouts can happen for any number of reasons, from network outages to code on the API server that just takes too long to execute.

To handle the timeout error, we just need to `rescue Faraday::TimeoutError` in our controller.

Luckily, we can also force a timeout by setting the request's `timeout` value. Normally you'd set this to a higher number than the default if the resource you were requesting was consistently too slow, however, if we set it to `0`, we can make it timeout to test our error handling:

```ruby
# searches_controller.rb
# ...
  begin
    @resp = Faraday.get 'https://api.foursquare.com/v2/venues/search' do |req|
        req.params['client_id'] = client_id
        req.params['client_secret'] = client_secret
        req.params['v'] = '20160201'
        req.params['near'] = params[:zipcode]
        req.params['query'] = 'coffee shop'
        req.options.timeout = 0
      end
      body = JSON.parse(@resp.body)
      if @resp.success?
        @venues = body["response"]["venues"]
      else
        @error = body["meta"]["errorDetail"]
      end

    rescue Faraday::TimeoutError
      @error = "There was a timeout. Please try again."
    end
    render 'search'
```

If we run our search again, we should see our timeout error.

Now let's delete that `req.options.timeout = 0` line, because we would never want to actually force a timeout on every request.  Now we have a Foursquare API search that successfully finds coffee shops near the user, handles response errors, and guards against timeouts!

## Using OAuth With APIs
We're going to expand on our Coffee Shop example to use OAuth with the Foursquare API to perform actions on behalf of an individual user.

Until now, we've been authenticating to APIs at the *application* level using a Client ID/Secret pair with each request.

As we learned, this level of application authentication is important in that it allows the API provider to ensure good behavior by any client application.

Application-level authentication like this gives us access to application-level functions, for instance, venue search. This is an application-level function because you don't need to be any particular user to search for a coffee shop. In fact, if we look at the [venue search](https://developer.foursquare.com/docs/venues/search) API documentation, we see an entry: **Requires Acting User: No** at the top.

Compare that to the documentation for [list friends](https://developer.foursquare.com/docs/users/friends), and we see that this function does require an acting user, which makes sense, because the application doesn't have friends, an individual user does.

So, if we want to show our user what their friends are doing, we need a way of authenticating the user to Foursquare through our application.

It's not enough for a user to be logged in to Foursquare and use our app, because Foursquare needs to know that we are acting on *behalf* of a user and that the user has *allowed* that action.

We could write some code to log in on behalf of the user, but that would require us to ask the user for their Foursquare login credentials. Think about that as if you're the user. Are you going to give some random website your login and password for Foursquare? The very thought should make you very suspicious of our motives.

Beyond the security issues, there's the problem of keeping things up to date. If we ask the user to log in to Foursquare on every request, that's a horrible experience.

However, if we store the user's Foursquare login info, we have whole other problems.

The first is that we'd have to store them unencrypted, in plain text, because Fourquare expects a person to log in with their unencrypted credentials. So we have another security problem. Now we have a database full of unencrypted Foursquare credentials waiting for anyone to get in there and grab them all.

Beyond the security issue is a logistical one. If the user changes her password on Foursquare, we'll never know about it, and our app will break. Now she has to update her Foursquare credentials in *two* places, which is cumbersome to say the least.

Okay, how do we authenticate a user without them entering their username and password into our application? [OAuth](https://en.wikipedia.org/wiki/OAuth).

OAuth provides a way for one application to authenticate to another on behalf of a user by means of a revokable, expirable *token*. If our application has a Foursquare token for the user, and someone gets access to our database of tokens, all of them can be revoked and a hacker never gets access to individual user credentials.

An OAuth token also provides a standard way for our application to tell Foursquare "hey, this user says we can do things for her", because part of implementing OAuth requires the user to take action on the provider (Foursquare, in this case), explicitly authorizing our application.

### Authentication flow
OAuth authentication workflow involved three steps at a high level.

1. Request access from the provider site (often via redirect to a
   special form)
2. Redirect back to our site with a code
3. Request a token from the provider using the code

Take a look at the [Access token section of the Foursquare API Documentation](https://developer.foursquare.com/overview/auth), specifically the second subsection, under **Code (preferred)**. It describes how to do those three steps with Foursquare.

#### Checking Authentication
The first thing we want to do is figure out if a user has already authenticated to Foursquare in this session.

Ultimately, users will be considered "logged in" if they have an access token stored in their session. So, let's create a private method `#logged_in?` in your `ApplicationController` that will return false if `session[:token]` is nil and true otherwise:

```ruby
# application_controller.rb

private
  def logged_in?
    !!session[:token]
  end
```

#### Redirect Users To Request Foursquare Access

The first step in the OAuth flow is to direct the user to Foursquare to request access if we haven't already.

According to the [docs](https://developer.foursquare.com/overview/auth), that URL looks like this:

```
https://foursquare.com/oauth2/authenticate
    ?client_id=YOUR_CLIENT_ID
    &response_type=code
    &redirect_uri=YOUR_REGISTERED_REDIRECT_URI
```

Part of our URL includes passing a `redirect_uri` parameter, so we'll need to set that up. Update your [Foursquare app](https://foursquare.com/developers/apps) and add a Redirect URI. Let's set it to `http://localhost:3000/auth` and save the app.

Because we're going to be using our client ID/secret a lot, instead of always typing it in, let's use a `.env` [file](https://github.com/bkeepers/dotenv) to hold our `FOURSQUARE_CLIENT_ID` and `FOURSQUARE_SECRET` values. Once that's set up, we'll be able to access these values as `ENV['FOURSQUARE_CLIENT_ID']` and `ENV['FOURSQUARE_SECRET']`, which is much easier to keep track of. Follow the instructions to set up Dotenv and add your app's client ID and secret. Don't forget to restart your server after you change any values in `.env`!

**Top-tip:** Dotenv is a great way to keep configuration variables for development, but always remember to add `.env` to your `.gitignore` so you don't share your secrets with everyone else!

Okay, that should handle everything we need to make this first request, so now we need to set up the redirect.

Write another private method `#authenticate_user` that will redirect the user to `https://foursquare.com/oauth2/authenticate` _if_ the user is not already logged in. Then we'll set up a `before_action` to check authentication.

```ruby
# application_controller.rb
before_action :authenticate_user

private

  def authenticate_user
    client_id = ENV['FOURSQUARE_CLIENT_ID']
    redirect_uri = CGI.escape("http://localhost:3000/auth")
    foursquare_url = "https://foursquare.com/oauth2/authenticate?client_id=#{client_id}&response_type=code&redirect_uri=#{redirect_uri}"
    redirect_to foursquare_url unless logged_in?
  end

  def logged_in?
    !!session[:token]
  end
```

Once you've implemented `#authenticate_user`, set the authentication as a `before_action` in your `ApplicationController`. In your `SessionsController`, skip the `before_action` with `skip_before_action :authenticate_user, only: :create`. Now, whenever users do not have an access token stored in their session, they will be redirected to the Foursquare authorization URL.

Let's try it out. Start your Rails server and try to hit the `/search` page. You should get redirected to Foursquare! Hit the "Allow" button and let's see what happens.

Error. Okay. That's good. That means it's working so far. Now we need to implement step two.

#### Foursquare Redirects Back To Our Site With A Code

When you registered your application, you set your redirect URL to `http://localhost:3000/auth`. This is where Foursquare is sending users after the login process. Now we need to handle that request.

In your `routes.rb` file, add a route for `get '/auth', to: 'sessions#create'`. This will route that redirect to our `SessionsController` and a `create` action, which is where we'll get the token. So now let's implement that.

#### Request A Token From Foursquare With The Code

Back to our handy [documentation](https://developer.foursquare.com/overview/auth)! Foursquare redirects users with a code that can be accessed through `params` and exchanged for an access token with a second GET request. We'll need to provide our ID, secret, and the code we just got.

We're going to use [Faraday](https://github.com/lostisland/faraday) to send this request in our controller action. If all goes well, according to the docs we can expect a JSON response with an `access_token`, so we'll parse that and put it in our `session[:token]`.

```ruby
# sessions_controller.rb
skip_before_action :authenticate_user

def create
  resp = Faraday.get("https://foursquare.com/oauth2/access_token") do |req|
    req.params['client_id'] = ENV['FOURSQUARE_CLIENT_ID']
    req.params['client_secret'] = ENV['FOURSQUARE_SECRET']
    req.params['grant_type'] = 'authorization_code'
    req.params['redirect_uri'] = "http://localhost:3000/auth"
    req.params['code'] = params[:code]
  end

  body = JSON.parse(resp.body)
  session[:token] = body["access_token"]
  redirect_to root_path
end
```

**Top-tip:** Make sure to skip the `authenticate_user` `before_action` when you're creating a session, otherwise you'll end up in an infinite loop of trying to figure out who the user is, which is a very existential bug.

#### Use the access token to access the API

Now that users have their API tokens, they can make calls to all of the API endpoints as long as those tokens are included in the request. Back in our [Foursquare auth docs](https://developer.foursquare.com/overview/auth), under the **Requests** section, we see that all we have to do now is add a `oauth_token` parameter to any request with the user's token.

Let's look again at the [friends](https://developer.foursquare.com/docs/users/friends) documentation and add a friends list to our application. First, let's add a route to `/friends`:

```ruby
# routes.rb
# ...
get '/friends', to: 'searches#friends'
```

And then handle that in our controller:

```ruby
# searches_controller.rb

def friends
  resp = Faraday.get("https://api.foursquare.com/v2/users/self/friends") do |req|
    req.params['oauth_token'] = session[:token]
    # don't forget that pesky v param for versioning
    req.params['v'] = '20160201'
  end
  @friends = JSON.parse(resp.body)["response"]["friends"]["items"]
end
```

**Top-tip:** Like many API providers, Foursquare gives you a way to try API calls right from the documentation. Try it with the [friends list](https://developer.foursquare.com/docs/explore#req=users/self/friends) to examine the response JSON.

Finally, let's set up our view:

```erb
# views/searches/friends.html.erb
<ul>
  <% @friends.each do |friend| %>
    <li><%= "#{friend['firstName']} #{friend['lastName']}" %></li>
  <% end %>
</ul>
```

## Posting to APIs
We're going to keep working with our Foursquare application and allow users to add tips to venues. The solution from the OAuth reading is included.

**Note:** Don't forget to set up your `.env` file (or copy it from the previous lesson)!

If we look at the [Add Tip documentation](https://developer.foursquare.com/docs/tips/add), we'll see that we need to make this request via POST. Up until now we've been making GET requests, because we're just asking for data. But once we start changing data on the server, we're in POST territory.

According to the docs, we'll need to POST to `https://api.foursquare.com/v2/tips/add` with a `venueId`, some `text`, and, since it requires an acting user, our `oauth_token`. If you need a refresher on how to do OAuth with Foursquare, click the [learn more link](https://developer.foursquare.com/overview/auth) next to where it says **Requires Acting User**.

Let's start with a form in our search results:

```erb
# search.html.erb
<h1>Search for Coffee Shops Near Me</h1>
<%= form_tag '/search' do %>
  <%= label_tag :zipcode %>
  <%= text_field_tag :zipcode %>
  <%= submit_tag "Search!" %>
<% end %>
<div>
  <% if @error %>
    <p><%= @error %></p>
  <% elsif @venues %>
    <ul>
    <% @venues.each do |venue| %>
      <li>
        <%= venue["name"] %><br>
        <%= venue["location"]["address"] %><br>
        Checkins: <%= venue["stats"]["checkinsCount"] %><br>
        <%= form_tag '/tips' do %>
          <%= hidden_field_tag :venue_id, venue["id"] %>
          Add tip: <%= text_field_tag :tip %><%= submit_tag "Add Tip" %>
        <% end %>
      </li>
    <% end %>
    </ul>
  <% end %>
</div>
```

We need to keep track of the venue ID for when we create the tip, so we'll hold onto it in a `hidden_field_tag` on the tip form.

**Note:** Now is a good time to delete the line that outputs `session[:token]`.

Now we need to set up a route for our form to POST to, and a route to see our tips after we've added them:

```ruby
# routes.rb
# ...
resources :tips, only: [:index, :create]
```

Let's get into the `TipsController` and set up the `create` action:

```ruby
# tips_controller.rb
#...
  def create
    resp = Faraday.post("https://api.foursquare.com/v2/tips/add") do |req|
      req.params['oauth_token'] = session[:token]
      req.params['v'] = '20160201'
      req.params['venueId'] = params[:venue_id]
      req.params['text'] = params[:tip]
    end

    redirect_to tips_path
  end
```

This looks similar to other calls we've made, except we're doing `Faraday.post` instead of `Faraday.get`. We're passing in our values as `params` just like with a GET request (more on this in a minute), and we get a response.

In this case, we want to show that it worked, so we're going to redirect to `tips_path` and show the user their tips.

Read the [List Detail](https://developer.foursquare.com/docs/lists/lists) document and see if you can set up the `index` action of our `TipsController`.

```ruby
# tips_controller.rb
def index
  resp = Faraday.get("https://api.foursquare.com/v2/lists/self/tips") do |req|
    req.params['oauth_token'] = session[:token]
    req.params['v'] = '20160201'
  end
  @results = JSON.parse(resp.body)["response"]["list"]["listItems"]["items"]
end
# ...
```

Looks very similar to our `friends` action.

It seems a little crazy that we have to dig so deep (`["response"]["list"]["listItems"]["items"]`) just to get to the actual tips list, but that is how deep they are nested in the response. You'll need to pay careful attention to the documentation and the response to make sure you're grabbing what you need from the response body.

And let's round it out by displaying the tips in our `index.html.erb` template:

```erb
# tips/index.html.erb
<h1>My Tips</h1>
<ul>
<% @results.each do |item| %>
  <li><strong>><%= item["venue"]["name"] %></strong><br>
    <%= item["tip"]["text"] %></li>
<% end %>
</ul>
```

Now we can load our Rails server, go to `/search`, search for coffee shops, add a tip to one we like, and see our tips!

### POST Params vs POST Body

The Foursquare API endpoints we've been using all have used *parameters* to pass data. For a GET request, those are querystring parameters. For a POST, they are `www-form-urlencoded` parameters - that is, they act as if we had created a `form` and clicked `submit`.

Other API providers, like Github, may require that POST data instead be JSON-formatted text in the request `body`. A request has a `body` just like a response does.

Fortunately, Faraday makes it easy to POST with JSON in the body of the request. Instead of doing:

```ruby
Faraday.post("https://url/to/api") do |req|
  req.params['my_param'] = my_value
end
```

You can do this:

```ruby
Faraday.post("https://url/to/api") do |req|
  req.body = "{ "my_param": my_value }"
end
```

All you have to do is assign the request `body` to a properly formatted JSON string!

In Postman, there's a section under the URL entry that has a place where you can enter a **Body** as well, so you can test APIs that take params or JSON in the body equally well.

## Refactoring: Service Objects
If we think back to *Single Responsibility Principle*, and the purpose of the components of MVC, we can come to the conclusion that we're forcing our controllers to know too much about Foursquare and the business logic of the data we get from the API when controllers are really supposed to be shuffling data back and forth between models and views.

We want to move our business logic out of our controllers, but how? We aren't going to use an `ActiveRecord` Model, because we're not dealing with our own database.

We are, however, dealing with data from *someone's* database, and the business logic of consuming and transforming that data, so we need something else.

**Service Objects**.

A service object is an object that we can use to encapsulate the inner workings of some business or *domain* logic that isn't strictly the responsibility of a single `ActiveRecord` model.

If you can imagine a complex CRM system, the act of creating a new customer might involve also setting up a sales pipeline, creating tasks and calendar items for a salesperson, and other related stuff. That all doesn't belong in the `Customer` model, but it also certainly doesn't belong in the `CustomersController`, so we would encapsulate it into a service object.

Similarly, in our system, things like dealing with OAuth, or knowing how to query the Foursquare API don't belong in our controllers or models, so we need service objects.

According to a [post by Tom Pewiński in Ruby Weekly](https://netguru.co/blog/service-objects-in-rails-will-help):

> As I understand it, a Service Object implements the user’s interactions with the application. It contains business logic that coordinates other artefacts. You could say it is the core of the application.

We're going to use service objects to execute our interactions with the external Foursquare API.

### Refactoring Authentication
#### Extracting A Service Object

Currently, your `SessionsController` `create` action looks something like this:

```ruby
# sessions_controller.rb
  def create
    resp = Faraday.get("https://foursquare.com/oauth2/access_token") do |req|
      req.params['client_id'] = ENV['FOURSQUARE_CLIENT_ID']
      req.params['client_secret'] = ENV['FOURSQUARE_SECRET']
      req.params['grant_type'] = 'authorization_code'
      req.params['redirect_uri'] = "http://localhost:3000/auth"
      req.params['code'] = params[:code]
    end

    body = JSON.parse(resp.body)
    session[:token] = body["access_token"]
    redirect_to root_path
  end
```

Create the folder `app/services` and create a file `foursquare_service.rb` within that folder. Then define a `FoursquareService` class.

```ruby
# app/services/foursquare_service.rb

class FoursquareService
end
```

Now, let's move the API interaction from `SessionsController` into `FoursquareService`. Define a method called `#authenticate!`. Our arguments will be the client ID, client secret, and code that we need to authenticate with Foursquare:

```ruby
  def authenticate!(client_id, client_secret, code)
    resp = Faraday.get("https://foursquare.com/oauth2/access_token") do |req|
      req.params['client_id'] = client_id
      req.params['client_secret'] = client_secret
      req.params['grant_type'] = 'authorization_code'
      req.params['redirect_uri'] = "http://localhost:3000/auth"
      req.params['code'] = code
    end
    body = JSON.parse(resp.body)
    body["access_token"]
  end
```

Looks very familiar, right? We've just moved the code from the controller to the service object, almost as-is. The only real difference is we pass in the parameters from the controller.

**Advanced:** This is an example of an ["Extract Method"](http://refactoring.com/catalog/extractMethod.html) refactoring. Anywhere you can simply cut code from one place into a new method is a place where you might be violating Single Responsibility Principle.

#### Back to the Controller

Now that we've started to set up our `FoursquareService` object, what do we need to do in our `SessionsController`? Instantiate a new `FoursquareService` object and call our `#authenticate!` method!

```ruby
# sessions_controller.rb
def create
  foursquare = FoursquareService.new
  session[:token] = foursquare.authenticate!(ENV['FOURSQUARE_CLIENT_ID'], ENV['FOURSQUARE_SECRET'], params[:code])
  redirect_to root_path
end
```

Now our controller is much cleaner. All it has to know is that there's a `FoursquareService` that can provide an OAuth token.

We pass in the client ID and secret via our `.env` environment variables because just as it's not the responsibility of the controller to know the internals of the Foursquare API, it's not the responsibility of the Foursquare service to know where to go to get system data.

If we run our Rails server, everything should still work as expected. To verify, trying accessing `/search` in a private tab that won't have your session already stored.

### Refactoring Friends

We've cleaned up the OAuth flow for the `SessionsController` using a service object. Now let's look at doing similar for our `friends` action.

Extract the Foursquare API call from the `friends` action in `SearchesController` and put it into a new `friends` method in the Foursquare service object.

```ruby
# services/foursquare_service.rb
# ...
  def friends(token)
    resp = Faraday.get("https://api.foursquare.com/v2/users/self/friends") do |req|
      req.params['oauth_token'] = token
      req.params['v'] = '20160201'
    end
    JSON.parse(resp.body)["response"]["friends"]["items"]
  end
```

Here we go as far as to return just the part of the JSON response that we need to build a friends list, rather than forcing the controller or view to know how to pull the right data. Since the method is named `friends`, it makes sense that it would just return a representation of the friends.

Then in our controller, we can update the `friends` action to use the service object:

```ruby
# searches_controller.rb
# ...
  def friends
    foursquare = FoursquareService.new
    @friends = foursquare.friends(session[:token])
  end
```

Now if we reload our `/friends` page, we should still see our friends!

## Summary

We've learned that service objects help us encapsulate business logic that doesn't belong in a controller or `ActiveRecord` model, allowing us to keep our controllers ["skinny"](http://robdvr.com/fat-models-skinny-controllers-skinny-models-skinny-controllers/) and observe the Single Responsibility Principle.

We've also extracted controller code into our service object and refactored our controllers to use our new service.

# Building APIs
We've seen how to *consume* various APIs, now let's talk about *providing* one.

The choice to build and provide an API boils down to one of two options:

1. Your data is valuable to more than just your application, or your application has valuable integration points for other applications to provide new and interesting services, so you build an *external* API and open it up to third-party developers.
2. Your data is only valuable to your company, but you access it in multiple ways (e.g. a web application, iOS application, and Android application all for your company), or you have a distributed/service architecture, or you're even just using a front-end framework with Rails as an API back-end, so you build an *internal* API to provide common access to all internally-managed clients.

## External API Considerations
External APIs are extremely valuable to the growth of the web. Because of external APIs you can watch your Uber driver approach, plot your course with Google maps, get a text when the driver is close, and pay without ever touching your wallet.

Because an external API is intended to be used by a wide variety of developers, with whom you wouldn't have much direct contact, there are a lot of things to account for when building one.

**Documentation:** An external API must be *extensively* documented to support your developer community. This includes not only functional specifications, but code samples, sometimes in multiple languages (human AND computer!).

Sometimes you'll want to provide tools to live-test the API, such as Foursquare does. You might even provide libraries (such as a gem) for the languages you support.

**Versioning:** We saw this with the Foursquare API. Versioning is an important consideration, and you have to make sure that you don't release code in such a way that it suddenly breaks applications that rely on your API.

**Authentication and Security:** Allowing external access to your data is a serious thing, so your external API needs robust access control and security. We came across this when consuming the Foursquare and Github APIs. We had to register our application, pass ID and secret pairs to the API, and sometimes use OAuth tokens on behalf of a user - all security implementations that need to be set up and maintained by the API provider.

**Quality of Service:** Once you have third-party developers relying on your API, there are expectations around quality, uptime, and response time.

You also have to consider things like **rate limiting**, where you monitor access and only allow a certain number of API calls per application in a given timeframe.

These are all important considerations if you plan to make your API public.

## Internal API Considerations

An internal API is a great way to provide access to data for multiple internal applications. While there are still many things to consider, the concerns for internal APIs are generally less strenuous because you are usually in control of all of the applications that will have access (or you can talk to the people who are in control).

**Documentation:** Documentation is still important, but with an internal API you generally won't have to provide support for such a wide array of consumers, and the docs can be a little more utilitarian.

**Versioning:** Versioning is still a consideration. Depending on your development process, it's possible that not all of your apps will be able to respond to changes at the same time, so you may still need versioning to support that. However, the consumers will all be known, so you won't have to worry as much about some rogue developer steadfastly holding on to the beta version of your API.

**Authentication and Security:** Security is still a concern, of course, but again, you'll be in control of what applications can access your API.

We don't want to store access keys or passwords on the client of your internal API in cookies or JavaScript, because then it wouldn't be secure. And depending on the use of the API, it might not make sense to do user-based authentication at all. So how do we ensure that only valid web applications are accessing our internal API?

Browsers implement something called [Same Origin Security Policy](https://en.wikipedia.org/wiki/Same-origin_policy), which prevents a page from running a script to access data from a different origin.

This is great because by default it means your API can't be accessed by another domain or website.

But what if you want to give another domain access to your API? To do that, we can configure [Cross Origin Resource Sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing), or **CORS**. CORS allows us to specify that certain other sites are allowed access while still blocking others.

**Note:** This type of access control is called *whitelisting*. That is, creating a list of allowed domains. This is the opposite of *blacklist* control, where you allow anything except for a list of explicitly blocked sites.

We won't be doing much with this in this unit, but there are great tools like [rack-cors](https://github.com/cyu/rack-cors) out there to help.

**Quality of Service:** You don't get to slack on this one with your internal APIs. In fact, there's a good chance that your internal API is mission-critical and handles more requests than an external API might.

You might not need to worry about rate limiting, but performance, uptime, and response time are still extremely important no matter who's accessing your API.

## Returning String Data
As developers, we do our best to hide it, but all of the Internet is just a bunch of strings being passed around from place to place. We put formats around our strings so that we can render them in an agreed-upon way, but it's all just strings. All you have to do is drop into your terminal and try: `curl https://twitter.com`. curl is a tool to transfer data from or to a server, doing all sorts of URL manipulations and transfers, using one of the supported protocols (DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET and TFTP).

When the browser requests this URL, this is what it sees. Just one big string of markup (HTML) and data. Then it interprets the markup and turns it into Twitter. But on the console, with nothing to turn the HTML into something, we just see the string. So if the Internet is just strings, do we always need the markup?

Okay, let's say we have a pretty basic post title and content here, with a teaser to only show the first bit of the post body. We want to give them a way to interact and ask for the whole post body, so we'll add a button to fill in the rest.

We want to do this without redirecting to the post `show` path, so we're
going to use an AJAX request to replace the truncated content with the
full body.

The first thing we'll need is a route and controller action to get the
body of the post.
```ruby
# routes.rb
# ...
get '/posts/:id/body', to: 'posts#body'
```

And in our controller:
```ruby
# posts_controller.rb
# ...
  def body
    post = Post.find(params[:id])
    render plain: post.description
  end
```

Note the `render plain:` call in the controller. **Normally, in a RESTful action, we allow the controller to implicitly render a template with the same name as the action**. Here, however, we want to *explicitly* render plain text, so we call `render` with the `:plain` option.

**Note:** There are a lot of options for `render`, including plain text, files, or nothing at all. Read more about `render` in the [Layouts and Rendering](http://guides.rubyonrails.org/layouts_and_rendering.html#using-render) RailsGuide.

Now that we have that action, we can hit it by browsing to `/posts/:id/body` and see that we are just rendering plain text. This route isn't useful at all to the HTML portion of the site, because it has no markup, so we have to consume it another way. That means we just wrote our first API endpoint! Okay, we have an API endpoint where we can get the rest of a post body. Now it's just a matter of providing a way for the user to request it, and getting the string response onto the page.

Let's start by adding a button. In our `_post.html.erb` partial, we want to provide a button for each post to request the body.

It has to be tied to each post so that we can know which post's body we're requesting, which means we'll need to make use of the `post.id` to pass along to our route.

```erb
# _post.html.erb
<h1><%= post.id %>: <%= post.title %></h2>
<div><%= truncate post.description %></div>

<button class="js-more" data-id="<%= post.id %>">More...</button>
```

**Note:** We're using the [`truncate` helper](http://api.rubyonrails.org/classes/ActionView/Helpers/TextHelper.html#method-i-truncate) here to only show a portion of the potentially long `description` string. By default, `truncate` will cut off everything after 30 characters.

We need a way to attach a `click()` event listener to the button in JavaScript. Since there will be multiple buttons on the page that need to respond to the click, we're using `class="js-more"` as an identifier, rather than an `id` attribute. Prefixing the `class` name with `js-` is a common way to communicate that this class is used as a JavaScript selector.

We also need to tie the button to a `post`, since it has to ask for the right content, so we're using a [data-* attribute](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes) of `data-id` to hold the `post.id` for us.

Now that we have that, let's wire up a `click()` listener to our `js-more` buttons and request the post body.

We'll use `jQuery` to wire it up in [document `.ready()`](https://api.jquery.com/ready/) in our `index.html.erb` template. Why not the `_post` partial? We only want to do this code once, and the partial will be loaded as many times as we have posts, so putting it in the main template ensures we just use it one time.

```js
// can be also put in the application.js file
$(function () {
	$('.js-more').on('click', function() {
		// debugger;
		var id = $(this).data('id');
		$.get("/posts/" + id + "/body", function(data) {
			alert(data);
		});
	});
});
```
We've set up a `click` listener for all our `.js-more` buttons, and used the `.get()` method to make an AJAX request to our `/posts/:id/body` route using the `id` we stored in the `data-id` attribute on the button.

The last thing we need to do is swap out that `alert(data)` call with actually putting the response into the body of our post.

If we look back at `_post.html.erb`, we see that our body is in a `<div>`. All we need to do is replace the inner text of that `<div>`. To do that, we need to identify it with the `post.id`.

```erb
# _post.html.erb
<h1><%= post.id %>: <%= post.title %></h2>
<div id="body-<%= post.id %>"><%= truncate post.description %></div>

<button class="js-more" data-id="<%= post.id %>">More...</button>
```

Now we've identified the element as `body-id`, so each `div` will have a predictable `id`, like `body-1`, `body-2`, and so on.

With that in place, we just need to update our JavaScript to find the right `div` by `id` and replace the text:

```erb
# posts/index.html.erb
# ...
<script type="text/javascript" charset="utf-8">
$(function () {
  $(".js-more").on('click', function() {
    var id = $(this).data("id");
    $.get("/posts/" + id + "/body", function(data) {
      // Replace text of body-id div
      $("#body-" + id).text(data);
    });
  });
});
</script>
```

We simply use the `#` selector to find the right `body-id`, and call `.text(data)` to replace the text of the `div` with what we got back from our API call.

If we reload the page now, and click "More..." on our posts, we should see the truncated text replaced with the full text! We've just implemented and consumed your first internal API endpoint!

What if we wanted all of the data from a post, and not just the body, from that API endpoint? What if we wanted to display the `show` page without doing a page refresh? Certainly we wouldn't want to make a bunch of separate requests to endpoints like `/post/:id/body` and `/post/:id/title` and `/post/:id/author`, would we? There has to be a better way!

## DIY JSON Serializer
Continuing with our blog application, we'll be improving our blog post API to provide more structure and more efficient data access. Make sure to run `rake db:seed` to set up some starting data!

Last time, we created an endpoint for the single piece of data we wanted to access. That worked well enough, but doesn't scale. If we want to use AJAX to get all the data from a blog post, we'd be creating so many endpoints, and making so many GET requests.

What we really need is one endpoint to make one request and get back structured data representing our blog post.

If we're directly accessing a post from our web browser, the answer is simple. The controller renders a template that contains HTML and the data we need.

We could still do this for an API endpoint, but then that HTML would probably get in our way. We want the data, not the formatting. This, like so many things, comes back to **Separation of Concerns. An API endpoint is only concerned with returning the data needed**; it's up to the API consumer, in this case our AJAX call, to decide how to display it.

This is where **serialization** comes in. Serialization is the process by which **we take executable code, in our case a Ruby object, and represent it as a string that can be consumed anywhere** (remember, the Internet is just strings) and then reconstructed back into usable code.

Remember that a Ruby *object* is an instance of a *class*, meaning it has been initialized and is running in memory. Serializing an object retains the *state*, or current values of all the object's attributes, when turning it into a string. This differs from a class definition because the class definition tells us what *any* object *could* look like, whereas a serialized object tells us what *one* object *does* look like. It's an important distinction.

Most of what a modern web API does is gather and serialize objects to be passed to the consuming code as a string over HTTP. So how do we do this serialization?

At one time, the standard was XML. XML is a structured, tag-based markup language similar to HTML. The tags are specified by the API provider and ideally describe the object being serialized in an organized, human-readable way.

An example of serializing a post into XML might be:
```erb
<post>
  <id>1</id>
  <title>This is a blog post</title>
  <description> Lorem ipsum sit amet dolor...</description>
  <author>
    <authorId>2</authorId>
    <name>Stephen King</name>
  </author>
</post>
```

If we look at that, it's pretty easy to figure out what's going on, right? When the web was first becoming more dynamic, and web services/APIs were in their nascent stages, XML was the lingua franca of inter-system communication. That's where **AJAX** came from: *Asynchronous JavaScript and XML*.

So **this is a `Post` instance serialized to XML**. The XML describes the state of the object, and we can find where to pull out individual bits of data, or even reconstruct the object in memory on a new system. In this way, XML was a great tool.

But XML had its problems as well. Just as browsers have a hard time agreeing on how to parse tag-based HTML, parsing XML is a chore. An XML parser has to essentially *read* through a document, finding the start and end tags of nodes and deciding what to do with the data. Searching XML documents with [XPATH](https://en.wikipedia.org/wiki/XPath) is cumbersome, and constructing an XML document can be a tedious exercise in string building and tag matching.

In addition, XML has the potential to inflate the size of the result exponentially. Look at the `<authorId>` tag in the example above. There's 20 bytes of metadata per one byte of data. Even in a system with hundreds of authors, that's an inefficient way of representing things. As the Internet grew and the demands on API speed and efficiency, especially as smartphones came into play over cellular networks, the size of the data being passed from API to consumer became *important*.

### JSON
**JSON stands for "JavaScript Object Notation", and was created as a way to use JavaScript to not only consume data from an API, but also serialize that data for consumption**. Since so much data was being used in AJAX calls by the mid 2000s, it only made sense to start offering that data in the native language of the calling code.

Beyond that, there's significant advantages to JSON. If we serialize
that same `Post` to JSON, it looks like this:
```javascript
{
  "id": 1,
  "title": "This is a blog post",
  "description": "Lorem ipsum sit amet dolor...",
  "author": {
    "id":2,
    "name": "Stephen King",
  }
}
```

This is still very readable, and it's easy to see what represents what, but just in this one example we've shaved almost 50 bytes from the string. That might not seem like much, but it's an almost 25% improvement in size, which, when stacked up against your mobile data plan, means 25% more times you get to obsessively check your Facebook likes.

The other great benefit of JSON is the structure. It's just a dictionary. A set of key-value pairs. And accessing the values of a dictionary is *super easy* compared to transversing the nodes of a tag-based document. We do it all the time with Ruby hashes. Not only is it more efficient, but it's also a function that's native to the language, rather than needing to build or use a library for parsing XML.

### Our Own JSON Serializer
So it makes sense to provide JSON to our API consumer, but we're running a Rails app, so we need to find a way to serialize our Ruby objects to JSON.

We need to return JSON from our controller, but it's not the controller's *responsibility* to serialize objects, so we'll start fresh with a new class.

Create a `app/serializers` directory, and add a `post_serializer.rb` file to it.

**Advanced:** Rails will automatically load any code found in any folders under `app/`, so you're encouraged to create new folders for new types of files that aren't `models`, `views`, or `controllers`. In a big Rails application you might find folders like `app/services`, `app/serializers`, `app/decorators`, and others. This is just one of many ways to keep your project organized.

Inside, we need to *serialize* a `Post` object. Again, **serialization is just turning the object into a string, in this case a JSON string**. We know how to build strings, so let's get to it.

```ruby
# serializers/post_serializer.rb
class PostSerializer
  def self.serialize(post)

    # start with the open brace to create a valid JSON object
    serialized_post = '{'

    serialized_post += '"id": ' + post.id.to_s + ', '
    serialized_post += '"title": "' + post.title + '", '
    serialized_post += '"description": "' + post.description + '", '

    # the author association can also be represented in JSON
    serialized_post += '"author": {'
    serialized_post += '"name": "' + post.author.name + '"}'

    # and end with the close brace
    serialized_post += '}'
  end
end
```

**Top-tip:** Take care with your quotations. We need to single-quote our strings because JSON requires double-quoted keys and string values.

Now that we have our serializer, let's update our `body` action to use it:

```ruby
# posts_controller.rb
# ...
  def body
    post = Post.find(params[:id])
    render json: PostSerializer.serialize(post)
  end
```

Note that we changed to `render json:` - we're still just rendering a string, but we need to tell the requestor that it's a properly formatted JSON string, so that we can operate on it with JavaScript. However, it's still a plain-text string.

Now if we browse to `/posts/:id/body` (pick an `id` from the `/posts` page), we will see our JSON!

**Top-tip:** If you haven't installed [JSONView](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en) in Chrome, now would be a great time. It helps immensely with reading your JSON and even validates it and gives you errors if there's a problem!

Now, `body` isn't really accurate anymore, since we're now returning a serialized `Post`, so let's change the route and action to `post_data`:

```ruby
# routes.rb
# ...
  get 'posts/:id/post_data', to: 'posts#post_data'
```

```ruby
# posts_controller.rb
# ...
  def post_data
    post = Post.find(params[:id])
    render plain: PostSerializer.serialize(post)
```

### Consuming JSON From The API
Now what happens if we reload our `/posts` page and try our `More`
button?

Nothing! Or, more specifically, if we look in the Chrome JavaScript console, we'll see a `404` error because we changed the route to the resource. So let's fix that first.

```erb
# posts/index.html.erb
# ...
<script type="text/javascript" charset="utf-8">
$(function () {
  $(".js-more").on('click', function() {
    var id = $(this).data("id");
    // change the URL to the new route
    $.get("/posts/" + id + "/post_data", function(data) {
      // Replace text of body-id div
      $("#body-" + id).text(data);
    });
  });
});
</script>
```

Okay, now if we reload and try again, we can see that our button now replaces the truncated body with the JSON. This makes sense. Before we were just returning a single value, but now we have a whole object. So we'll need to alter the code to just pull the `description` from the response JSON.

```erb
# posts/index.html.erb
# ...
<script type="text/javascript" charset="utf-8">
$(function () {
  $(".js-more").on('click', function() {
    var id = $(this).data("id");
    $.get("/posts/" + id + "/post_data", function(data) {
      // Replace text of body-id div
      $("#body-" + id).html(data["description"]);
    });
  });
});
</script>
```

As you can see, we can access the JSON just like any other dictionary, and get the value for the `"description"` key.

### Using JSON To Build The Show Page

Now we want to set up our blog so that we can click a link to see the next entry when we're reading the current one, but we don't want to have to refresh the page.

We can reuse our `post_data` API, since that's exactly what a `Post` show page should have on it!

We need to update the HTML part of our template with some metadata we can use to identify the different elements, and add our `Next` link.

```erb
# posts/show.html.erb
<div class="authorName"><%= @post.author.name %></div>
<a href="#" class="js-next" data-id="<%=@post.id%>">Next...</a>
<h1 class="postTitle"><%= @post.title %></h1>
<p class="postBody"><%= @post.description %></p>
```

Note that we are using `data-id` on the `<a>`. We'll need to keep that updated so that we know what post to request next.

Okay, now let's add the JavaScript to handle this.

```erb
# posts/show.html.erb
# ...
<script type="text/javascript" charset="utf-8">
$(function () {
  $(".js-next").on("click", function() {
    var nextId = parseInt($(".js-next").attr("data-id")) + 1;
    $.get("/posts/" + nextId + "/post_data", function(data) {
      $(".authorName").text(data["author"]["name"]);
      $(".postTitle").text(data["title"]);
      $(".postBody").text(data["description"]);
      // re-set the id to current on the link
      $(".js-next").attr("data-id", data["id"]);
    });
  });
});
</script>
```

Here we grab the `data-id` attribute value, use `parseInt()` so we can cast it and add one to it so we can request the *next* post.

After that, it's just a matter of using `$.get()` to hit our `post_data` endpoint, and replace the `text` of each part of our show page.

If we reload and go to our first post, then click `Next`, we get that smooth AJAX data refresh we've been looking for.

At least until we get to the last post.

## Using `.to_json` and `respond_to``
Last time out we created a `PostSerializer` and used it to serialize a `Post` to JSON. It worked great, but doing all that string concatenation and keeping track of the different quotes was kind of a nightmare. Imagine having to write serializers by hand for objects with more than four fields! This is something people do every day in Rails, so there has to be a better way, right?

Of course there is. Rails provides the `to_json` method which will take our object and, well, convert it to JSON. Let's see it in action. In our controller, let's swap our call to the `PostSerializer` for a `to_json`.

```ruby
# posts_controller.rb
# ...
  def post_data
    post = Post.find(params[:id])
    render json: post.to_json
  end
```

Okay, well, surely it can't be that simple. Let's load up our Rails server and browse to `/posts`. Click one of the "More" buttons, and, just like that, it updates the post body. We didn't have to change a thing. Think of it as a testament to how great a job we did writing our own serializer.

Now if we click on the first post and use our `Next...` link, that should mostly work too.

I say mostly, because it's not updating the author name. That's something we added in to our serializer, but by default, `to_json` only serializes the main object, not any associations. How can we change that?

You can tell `to_json` what associated objects to include, using the `include` option.
```ruby
# posts_controller.rb
# ...
  def post_data
    post = Post.find(params[:id])
    render json: post.to_json(include: :author)
  end
```

Now if we reload that post show page and click `Next`, the author should update as well.

If we browse to `/posts/id/post_data`, we can see the raw JSON of our object. It should look something like this:

```javascript
{
  id: 1,
  title: "A Blog Post By Stephen King",
  description: "This is a blog post by Stephen King. It will probably be a movie soon.",
  created_at: "2016-02-22T00:29:21.022Z",
  updated_at: "2016-02-22T00:29:21.022Z",
  author_id: 1,
    author: {
      id: 1,
      name: "Stephen King",
      hometown: null,
      created_at: "2016-02-22T00:29:20.999Z",
      updated_at: "2016-02-22T00:29:20.999Z"
    }
}
```

It's great that `to_json` gives us all this, but we don't really need all of it.

A good API endpoint should return *only* the data that is needed, nothing more. So how do we get rid of that stuff?

It turns out `to_json` gives us ways to exclude data as well, using the `only` option, similar to how we'd specify certain routes for a resource.

```ruby
# posts_controller.rb
# ...
  def post_data
    post = Post.find(params[:id])
    #render json: PostSerializer.serialize(post)
    render json: post.to_json(only: [:title, :description, :id],
                              include: [ author: { only: [:name]}])
  end
```

We can use `only` both on the main object and the included objects.

**Top-tip:** Notice that we have to pass `author:` inside an array for `include` now that we are specifying additional options.

Reloading the `/posts/id/post_data` page now gives us something more like this:
```javascript
{
  id: 1,
  title: "A Blog Post By Stephen King",
  description: "This is a blog post by Stephen King. It will probably be a movie soon.",
  author: {
    name: "Stephen King"
  }
}
```

Which is exactly the data we need to get the job done.

### Responding To Requests With Different Formats
If we think about what we've been doing when we load `/posts/id/post_data`, we're really just requesting a `Post` resource, same as if we were on the Post `show` page. In fact, that's exactly what we're doing in AJAX on the Post `show` page, requesting the data for that page and replacing the values.

Given what we know about REST, and about DRY (don't repeat yourself), it seems like the `post_data` route and action are redundant. If we just want to request the post resource for `show`, we should be able to do that in one place.

In the desktop application world, we identify formats by *file extension*, so we know that `file.txt` is a plain text file, and `file.gif` is an awesome animated gif file.

Rails provides us with a similar way to do this, [using `respond_to`](http://apidock.com/rails/ActionController/MimeResponds/InstanceMethods/respond_to).

If we go into our `show` action and add a `respond_to` block, we can specify what to render depending on if the request is looking for HTML or JSON.

```ruby
# posts_controller
# ...
  def show
    @post = Post.find(params[:id])
    respond_to do |format|
      format.html { render :show }
      format.json { render json: @post.to_json(only: [:title, :description, :id],
                              include: [author: { only: [:name]}]) }
    end
  end
```

Now if we browse to `/posts/id`, we get the HTML page as expected. HTML is the default format for any request. We could also browse to `/posts/id.html`, and get the same thing.

But if we browse to `/posts/id.json`, we now get our serialized post in JSON form!

Now let's update the code in our `show.html.erb` to use the `show` route.

```erb
# posts/show.html.erb
# ...
$(function () {
  $(".js-next").on("click", function() {
    var nextId = parseInt($(".js-next").attr("data-id")) + 1;
    $.get("/posts/" + nextId + ".json", function(data) {
      $(".authorName").text(data["author"]["name"]);
      $(".postTitle").text(data["title"]);
      $(".postBody").text(data["description"]);
      // re-set the id to current on the link
      $(".js-next").attr("data-id", data["id"]);
    });
  });
});
```

Instead of doing a `$.get()` to `/posts/id/post_data`, we are now getting `/posts/id.json`. If we reload the page and click out `Next` button, everything still works and we don't have to change any of the code to extract the JSON values!

## Using Active Model Serializer
In the last iteration of our blog application, we saw that using `to_json` was an easy way to serialize objects to JSON without having to create our own serializer. However, if we look at our current controller code:

```ruby
# posts_controller.rb
# ...
  def show
    @post = Post.find(params[:id])
    respond_to do |format|
      format.html { render :show }
      format.json { render json: @post.to_json(only: [:title, :description, :id],
                              include: [author: { only: [:name]}]) }
    end
  end
```

It's clear that even a little bit of customizing the output of `to_json` can get ugly real quick. Imagine if the post had `comments` and comments had `users` and pretty soon we're getting real deep in the weeds trying to keep track of all the `include`s and `only`s in a single line of `to_json`.

Or imagine using `to_json` to render something like this venue response from the Foursquare API:

```javascript
{
  meta: {
    code: 200
    requestId: "56cb5db4498e20e3892dd035"
  }
  notifications: [
    {
      type: "notificationTray"
      item: {
        unreadCount: 41
      }
    }
  ]
  response: {
    venue: {
      id: "40a55d80f964a52020f31ee3"
      name: "Clinton St. Baking Co. & Restaurant"
      contact: {
        phone: "6466026263"
        formattedPhone: "(646) 602-6263"
      }
      location: {
        address: "4 Clinton St"
        crossStreet: "at E Houston St"
        lat: 40.72107924768216
        lng: -73.98394256830215
        postalCode: "10002"
        cc: "US"
        city: "New York"
        state: "NY"
        country: "United States"
        formattedAddress: [
          "4 Clinton St (at E Houston St)"
          "New York, NY 10002"
        ]
      }
    canonicalUrl: "https://foursquare.com/v/clinton-st-baking-co--restaurant/40a55d80f964a52020f31ee3"
    categories: [
    {
      id: "4bf58dd8d48988d16a941735"
      name: "Bakery"
      pluralName: "Bakeries"
      shortName: "Bakery"
      icon: {
        prefix: "https://ss3.4sqi.net/img/categories_v2/food/bakery_"
        suffix: ".png"
      }
      primary: true
    }
    {
      id: "4bf58dd8d48988d143941735"
      name: "Breakfast Spot"
      pluralName: "Breakfast Spots"
      shortName: "Breakfast"
      icon: {
        prefix: "https://ss3.4sqi.net/img/categories_v2/food/breakfast_"
        suffix: ".png"
      }
    }
    {
      id: "4bf58dd8d48988d16d941735"
      name: "Café"
      pluralName: "Cafés"
      shortName: "Café"
      icon: {
        prefix: "https://ss3.4sqi.net/img/categories_v2/food/cafe_"
        suffix: ".png"
      }
    }
  ]
// ...
// this is just the first 5%. There's so much more.
```

Forget "ugly" or "cumbersome", it might be nearly impossible to keep track of all that inside a single `to_json` call, and it would certainly be frustrating to try to go in and change any of it later. And let's face it. With all that data to track, we're extremely likely to mistype something and introduce bugs.

### ActiveModel::Serializer

ActiveModel::Serializer, or AMS, provides a convention-based approach to serializing resources in a Rails-y way.

What does that mean? At a basic level, it means that if we have a `Post` model, then we can also have a `PostSerializer` serializer, and by default, Rails will use our serializer if we simply call `render json: post` in a controller.

How is that different than when we created our own `PostSerializer` by hand and used it in the controller? Firstly, we had to explicitly call our `PostSerializer.serialize` method to do the work, whereas the convention-based approach of AMS makes it an implicit call.

But second, and more importantly, AMS doesn't require us to do the tedious work of building out JSON strings by hand. We'll see it in action shortly.

You may have seen [JBuilder](https://github.com/rails/jbuilder) files pop up when scaffolding things in your Rails 4 applications.

JBuilder is another serialization tool that was included by default in Rails 4. JBuilder takes the approach that the JSON serialization is more of a view function than a controller function, and as such, you could create something like:

```erb
# app/views/posts/show.json.jbuilder

json.title @post.title
json.description @post.description
json.author do
  json.name @post.author.name
end
```

This is a nice and flexible way to do things, but as you can see, also somewhat repetitive just for a few attributes.

You can split hairs as to whether or not rendering JSON is a view-level template responsibility or a controller-level responsibility - there's valid arguments either way (as there so often is in programming).

In Rails 5, however, the goal was to allow developers to create lean, efficient, API-only Rails applications. M and C without the V. With the popularity of mobile apps and robust front-end frameworks like Ember.js and Angular.js, there was a need to strip Rails down to just what is needed to serve as an API, and ActiveModel::Serializer, not being tied to the View layer, is how the Rails team chose to move forward.

We have our blog application from the previous lesson. Let's refactor it to use AMS.

First we need to add the gem, as it's not built-in to Rails 4 yet.
```ruby
# Gemfile
#...
gem 'active_model_serializers'
```

Run `bundle install` to activate the gem. Now we need to generate an `ActiveModel::Serializer` for our `Post`. Thankfully, the gem provides a generator for that. Drop into your console and run:

`rails g serializer post`

**Note:** If you are using your old code, make sure to delete the existing `post_serializer.rb` from the `app/serializers` directory before running the generator.

If we look at the generated `post_serializer.rb`, it should look something like this:

```ruby
class PostSerializer < ActiveModel::Serializer
  attributes :id
end
```

We want to get some more information out of it, so let's add a couple attributes.

```ruby
class PostSerializer < ActiveModel::Serializer
  attributes :id, :title, :description
end
```

To make use of our new serializer, we need to get rid of the `to_json` stuff in our controller:

```ruby
# posts_controller.rb
 def show
    @post = Post.find(params[:id])
    respond_to do |format|
      format.html { render :show }
      format.json { render json: @post}
    end
  end
```

Remember that we said calling `render json: @post` would implicitly use the new ActiveModel::Serializer to render the post to JSON? Let's see it in action. Restart your Rails server and browse to `/posts/1.json` and look at the results. It should look like this:

```javascript
{
  id: 1,
  title: "A Blog Post By Stephen King",
  description: "This is a blog post by Stephen King. It will probably be a movie soon."
}
```

Worked like a charm! Nothing we didn't want, and our controller is back to a clear, non-messy state.

### Rendering An Author

What's missing that we had before? The author name. So how do we do that?

Because AMS is modeled after the way Rails handles models and controllers, rather than build serialization of the author into the post, as we have in the past, we need to create a new `AuthorSerializer`.

`rails g serializer author`

And add the author name to the list of attributes:

```ruby
class AuthorSerializer < ActiveModel::Serializer
  attributes :id, :name
end
```

Now to test this out, let's modify our `authors_controller#show` action to handle a JSON request:

```ruby
class AuthorsController < ApplicationController
  def show
    @author = Author.find(params[:id])
    respond_to do |f|
      f.html { render :show }
      f.json { render json: @author }
    end
  end
end
```

And load up `/authors/1.json`. We should see something that looks like this:

```javascript
{
  id: 1,
  name: "Stephen King"
}
```

But how do we add the author name to our post JSON?

Again, we lean on those Rails conventions. If we add a `has_one :author` to our `PostSerializer`:

```ruby
class PostSerializer < ActiveModel::Serializer
  attributes :id, :title, :description
  belongs_to :author
end
```

Reload `/posts/1.json`, and we will now see our author information.

```javascript
{
  id: 1,
  title: "A Blog Post By Stephen King",
  description: "This is a blog post by Stephen King. It will probably be a movie soon.",
  author: {
    id: 1,
    name: "Stephen King"
  }
}
```

And now if we reload our first post show page and click through our `Next` button we can see that everything works exactly the same as before!

Now what if next we were building out our Author show page and wanted to render a list of an author's posts along with the author's information?

Well, it's as simple as adding a `has_many :posts` to the `AuthorSerializer`!

```ruby
class AuthorSerializer < ActiveModel::Serializer
  attributes :id, :name
  has_many :posts
end
```

### Rendering With Explicit Serializers
 Let's suppose that when we display a Posts Author, we don't need all the information being rendered from the AuthorSerializer. Since our post JSON really just needs an author's name, we might want to do a simpler serialization of the author for those purposes.

Let's make a new `PostAuthorSerializer`:
`rails g serializer post_author`

And let's add the bare minimum of what we need for the author to be embedded in a post:

```ruby
class PostAuthorSerializer < ActiveModel::Serializer
  attributes :name
end
```

But how do we get the `PostSerializer` to use this instead of the default? We have to *explicitly* give it a serializer to use rather than relying on the convention:

```ruby
class PostSerializer < ActiveModel::Serializer
  attributes :id, :title, :description
  belongs_to :author, serializer: PostAuthorSerializer
end
```

Now we're telling AMS to render `:author` with `PostAuthorSerializer` instead of the default.

So if we reload `/authors/1.json` we should see the author with their posts, and if we reload `/posts/1.json` we should see our post with just the simple author information.

In this way, AMS is a powerful way to compose an API with explicit, easy-to-maintain serializers, rather than try to keep track of what things you do and don't want to render at the controller or view level.

## Receiving API POSTs
So far, we've been focused on enhancing how we read our blog, but what about creating blog posts? How can we apply what we've been doing with JSON and AJAX to make creating a post a better experience?

In our `posts/new.html.erb`, we already have a form. If we click the button, it will submit as normal and the code in the controller's `create` action will do whatever it's set up to do.

What we want to do is set it up so that we use this form, but use jQuery and AJAX to submit it, so that we can handle a JSON response and display the newly created blog post without redirecting to the post `show` page.

Our first order of business is to prevent the default form submit action and do our own thing. To do this, we need to hook up an event handler to the form's `submit` event, and then block the form from doing an HTML submit as it normally would.

```erb
# posts/new.html.erb
<%= form_for(@post) do |f| %>
  <div class="field">
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </div>
  <div class="field">
    <%= f.label :description %><br>
    <%= f.text_field :description %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>

<script type="text/javascript" charset="utf-8">
  $(function () {
    $('form').submit(function(event) {
      //prevent form from submitting the default way
      event.preventDefault();
      alert("we r hack3rz");
    });
  });
</script>
```

We keep our regular `form_for` and just add an event listener for `$('form').submit()` to our document `ready()`.

Next, we stop the form from actually submitting by calling `event.preventDefault();`. This is super important, otherwise all our hard work will be undone.

Finally, we'll just toss up an `alert` to make sure we're on the right path.

If we reload `/posts/new` now, and submit the form, we should get our `alert`, but no page refresh and no new post created. We've successfully hijacked the form `submit`! This is as close as we may ever come to being l33t.

We need to get the form values and POST them to `/posts`, which is the route for creating a new post.

```erb
# posts/new.html.erb
# ...
<script type="text/javascript" charset="utf-8">
  $(function () {
    $('form').submit(function(event) {
      //prevent form from submitting the default way
      event.preventDefault();

      var values = $(this).serialize();

      var posting = $.post('/posts', values);

      posting.done(function(data) {
        // TODO: handle response
      });
    });
  });
</script>
```

In here, we're making use of the very handy [jQuery `serialize()` method](http://api.jquery.com/serialize/), which takes our form data and serializes it for us. Sure, we could individually select each form element and build our own JSON data, but we've done enough of that in this unit, so we've earned this shortcut!

Next we use jQuery `post()`, much like we've been using `get()` to retrieve data. We pass it the URL and our `values`.

Finally, we're using the `posting` object to specify what should happen when the AJAX request is `done`. This is where we'll need to handle the response.

**Advanced:** The jQuery `post()` method returns a [jqXHR](http://api.jquery.com/jQuery.ajax/#jqXHR) object, which we're storing in our `posting` variable. These `jqXHR` objects implement the [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) interface, which is used for deferred or asynchronous operations.

If we look at our running Rails server, we'll probably see something like this:

```erb
Started POST "/posts" for ::1 at 2016-02-23 01:48:39 -0500
Processing by PostsController#create as */*
  Parameters: {"utf8"=>"✓", "authenticity_token"=>"k2thnauWV/3diGaOesdnSZaygMsA7tR7N0qa/HVk4M2IO4Q0HCuOnTF43NgaAZr/2N/5iHyKu6boAB/fVb0sww==", "post"=>{"title"=>"123abc", "description"=>"easy as"}}
```

That looks just like a regular POST with regular `params`. Since Rails does the work of creating a `params` hash for us, it doesn't matter if it comes from the form or from an AJAX request.

Okay, so we did create a post. But if we look in our controller, we're supposedly redirecting to our `post_path`. And if we look further into our running Rails server, we'll see that it *did* redirect after creating our post.

When we do an AJAX request, we're expecting data to be returned so that we can deal with it on the client side. So when our controller redirected and rendered the post `show` page, it actually sent that data to the `.done()` method.

You can see this in action by adding a `console.log(data)` inside the `.done()` function and then inspecting the response.

```javascript
//...
  posting.done(function(data) {
    // TODO: handle response
    console.log(data);
  });
//...
```

You'll see the full HTML of the `show` page for that new post.

#### A Side-note About HTTP Codes
HTTP status codes exist to let the client know what kind of response they are getting, and what to do with it.

The code for the kind of redirect that happens when we put `redirect_to` in our controller is `302`. When a browser makes a request, and gets a `302` code, it knows that it needs to follow the "redirect" to the given link and load that page next.

**Advanced:** The `302` redirect is considered a *temporary*, or *found* redirect, as in, "You found the right thing, but now go to this URL." There's other `300` codes as well, and they all deal with redirects, such as the `301` redirect, which tells the requestor that the resource they are looking for has moved permanently. Check out the full list of HTTP codes [here](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#3xx_Redirection).

AJAX requests don't follow redirects because they *can't*. If they did, then you would lose any code that happens in the `done()` function and just go to a new page that doesn't have that JavaScript code.

So when an AJAX request sees a 302, it chooses not to follow the link and instead just takes whatever the rendered data is.

Rails handles HTTP codes for us when we do HTML requests, but when we build JSON APIs, we need to be cognizant of using the appropriate codes to let the client know what to expect.

### Rendering the Response
What we really want to do is get a JSON representation of the post we just created so that we can use it to display the new post without redirecting or refreshing the page.

Let's get into the controller, get rid of that redirect, and use the ActiveModel::Serializer that we already have.

```ruby
# posts_controller.rb
# ...
  def create
    @post = Post.create(post_params)
    render json: @post, status: 201
  end
```

**Note:** We are specifying the `status: 201` for this request, rather than the standard `200`, which means `OK`. Technically, this is a successful request and could be considered `200`, but we want to specify what happened more granularly, and `201` means that the resource was `created`.

With our controller code in place, we need to make some additions to our template to handle the response and display it on the page.

```erb
<%= form_for(@post) do |f| %>
  <div class="field">
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </div>
  <div class="field">
    <%= f.label :description %><br>
    <%= f.text_field :description %>
  </div>
  <div class="actions">
    <%= f.submit %>
  </div>
<% end %>

<div id="postResult">
  <h2 id="postTitle"></h2>
  <p id="postBody"></p>
</div>

<script type="text/javascript" charset="utf-8">
  $(function () {
    $('form').submit(function(event) {
      //prevent form from submitting the default way
      event.preventDefault();

      var values = $(this).serialize();

      var posting = $.post('/posts', values);

      posting.done(function(data) {
        var post = data;
        $("#postTitle").text(post["title"]);
        $("#postBody").text(post["description"]);
      });
    });
  });
</script>
```

Very similar to what we've been doing when we use AJAX to `get()` a resource, we just parse the JSON and add it to the DOM. To keep it simple, we just have one `<DIV>` that has placeholders for the data, and we fill it in when we get that response.


# AJAX GET in Rails
The basic process:
1. Hijack the click event
2. Once we're in the click event, we fire and AJAX request to GET the data  
3. Once we have the data we need to place it in the DOM

Our HTML setup for the interface we create has a link and should load comments in a specific div once we click that link
```html
  <%= link_to "Load Comments", post_comments_path(@post), :class => "load_comments" %>

  <div class="comments"></div>
```

## First version: the server responds with HTML

### Using $.ajax()
In it's simplest form hijacking a link means:
```js
$("a[href='/posts/1/comments']").on("click", function(e){
  e.preventDefault();
  alert("You clicked on the link!")
  })
```

Let's put our js in the app/assets/javascripts folder that describes our page: post.js (let's not use coffee script).
To make sure to get our javascript fire since it is called from the <head>, we need to put it in a `$(document).ready()` function.

So now we can target this link and use the low level jQuery `.ajax()` function
```js
// shorthand to $(document).ready(function){...}
$(function(){

  $("a.load_comments").on("click", function(e){

    $.ajax({
        method: "GET",
        url: this.href
    }).success(function(response){ // success is when the server responds with a 2xx
        // console.log(data)
        // debugger (debugger is our binding.pry)
        $('.comments').html(response);
    }).error(function(error){
        alert("We broke!")
    });

    e.preventDefault();
  });
})
```

This ajax call will now send a request to our CommentsController#index method
```ruby
def index
  @comments = @post.comments
  # implicit rendering of the full layout + comments.index.html (render 'comments/index') if we say nothing
  # we want it without the layout
  render 'comments/index', layout: false
  # we could actually just write
  # render layout: false
end
```
This would render whatever we have defined in out 'comments/index' template, like
```erb
<ol>
  <% @comments.each do |comment| %>
    <li><%= comment.content %></li>
  <% end %>
</ol>
```
but of course with the data, like '<ol><li>First comment</li><li>Another one</li><li>And again</li></ol>'.

Not that we can test our `.error()` handler by raising an error in our controller:
```ruby
def index
  @comments = @post.comments
  raise "Error!!!!".inspect
  render 'comments/index', layout: false
end
```

### using $.get()
We can use the `jQuery.get();`

It makes our javascript simpler:
```js
$(function(){

  $("a.load_comments").on("click", function(e){

    $.get(this.href).success(function(response){
      $('.comments').html(response);
    });

    e.preventDefault();
  });
})
```

CONCLUSION: This method sending back HTML is great because it's really quick. But sometimes you don't have control over the server so you cannot request HTML. Let's see what happens when you're given JSON.

## Second Version: the server responds with JSON
What we basically want first, is to be able to view
`localhost:3000/posts/1/comments.json`

The Rails convention for rendering JSON is
```rb
def index
  @comments = @post.comments
  render json: @comments
  # instead of our previous
  # render layout: false
end
```
This will automatically take the @comments object and convert it to JSON.

So let's do our request:
```js
$(function(){

  $("a.load_comments").on("click", function(e){

    $.get(this.href).success(function(json){
      // debugger
    });

    e.preventDefault();
  });
})
```
Out $.get() request is smart enough to realize that the string returned by the server is actually a JSON, and it parses it. In the past you'd have to use $.getJSON() explicitly, or use $.ajax() and declare a dataType: "json".

What we'll do is changing just a bit the html.erb setup of our view to be working simply with <li> tags
```html
  <%= link_to "Load Comments", post_comments_path(@post), :class => "load_comments" %>

  <div class="comments">
    <ol>
    </ol>
  </div>
```

So let's do our request:
```js
$(function(){

  $("a.load_comments").on("click", function(e){

    $.get(this.href).success(function(json){
      var $ol = $('div.comments ol')  // $ is a convention for variable that are jQuery object
      $ol.html("")                    // we make sure to empty the <ol>

      // we iterate, just like a block in ruby
      json.forEach(function(comment){
        $ol.append("<li>" + comment.content + "</li>")
      })
    });

    e.preventDefault();
  });
})
```

## Third version: using getScript (remote true)
Instead of manually triggering ajax, where the browser knows what to do when you click on the link, we will create a server side ajax model.

We want to get a response that is made of javascript, and contains instructions of what to do.
```js
$(function(){

  $("a.load_comments").on("click", function(e){

    $.ajax({
      url: this.href,
      dataType: 'script'
    })

    e.preventDefault();
  });
})
```

And we can actually abstract this even more and comment it all out if we use `remote: true`
```html
  <%= link_to "Load Comments", post_comments_path(@post), :class => "load_comments", :remote => true %>

  <div class="comments">
    <ol>
    </ol>
  </div>
```
This basically says that rails will automatically fire an ajax request when the link is clicked, asking for a .js file at the indicated URL and then process the js view that contains all of the indications to make the final rendering.

We're going back to rendering the normal layout (this is rendered if the client asks for html)
```rb
def index
  @comments = @post.comments

  render layout: false
  # this implicitly replaces the whole:
  # respond_to do |format|
  #   format.html {render 'index.html', layout: false}
  #   format.js   {render 'index.js', layout: false}
  # end
end
```
and create a new file: `views/comments/index.js.erb`. Even tough it's technically javascript, we're in our rails stack and can use erb and our instance variables. Rails will process first, then respond to the request by sending the processed js file, containing instructions.

This file will now be rendered by using the naming convention, if the client asks for javascript (dataType: 'script').
```js
var html = "<%= j(render 'comments/comments') %>"

$("div.comments").append(html);
```
We use the `j` actionView method because we want to escape html and serve a valid javascript string.

Let's create a partial `app/views/comments/_comments.html.erb` to hold our template that we'll then insert into the dom.
```html
<ol>
  <% @comments.each do |comment| %>
    <li><%= comment.content %></li>
  <% end %>
</ol>
```

This techniques enables to simplify the client side code. This enables to keep all the code into the rails stack, and has massive support from the Rails community. This technique is only in rails, and differs from the 2 previous methods, with a lot of client code, like in Ember, Angular, React and jQuery.



