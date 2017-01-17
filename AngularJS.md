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













