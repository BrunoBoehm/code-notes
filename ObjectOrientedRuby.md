# Object Oriented Ruby
It's natural to wonder, "how can a string of ones and zeroes be referred to as an 'object'?". A code object representing a water pipe (instead of a smoking pipe) might contain values for `length`, `diameter`, `material`, and `manufacturer`. The bundling of these individual pieces of information together begins to form a larger whole, it also bundles customized functions that can be performed on that data (methods).

Object oriented programming, or OOP, is an extremely useful programming paradigm in which we can organize our code according to how real-world objects might interact with one another. We can wrap properties/data and behavior up in classes, and then create instances, or individual "members", of those classes that can interact with one another.

Imagine a phone call between 2 people. Sure, the PEOPLE are real, but what about the phone call? If we think about the phone call through OOP, we can model it too! A phone call has a caller and a receiver, a duration, and even a cost_per_minute. In the real world, it's not a real thing, but in OOP IT IS!

## Classes and instances
Let's say we are building a dog walking app. Our app's users might be dog walkers and dog owners and they can use the app to manage the dog walks. Such an app would need to store information about a potentially large number of dogs.

### Class instances
Think of a `class` like the blueprint that defines how to build an object. The Dog class is different from an individual dog just as the blueprints that show how to build a house are not the actual house.

A Ruby class both contains the instructions for creating new objects and has the ability to create those objects. Calling `.new` on the Dog class is like getting a brand new `dog` object from an *assembly line* which produces a series of similar dog objects based on the same `Dog` template.

Class names begin with **capital letters because they are stored in Ruby constants**. If your class name contains two words, the name should be `CamelCased`.

```ruby
class Dog
  # some code to describe a dog
end

fido = Dog.new
fido #=> #<Dog:0x007fc52c2d7d20>
fido.object_id #=> 70173135795280
fido.methods
```
On the Dog class, we call the `.new` method and that will *instantiate* a new dog. Classes act as a factory for our objects, capable of instantiating new instances. Instantiate means **bringing a new object to life**, a new individual, like a particular dog, like Snoopy or Lassie or Rover. An `instance` is a single occurrence of an object. Instances refer to the individual objects produced from the class.

Notice that every time you make an instance of a class, Ruby tells you that the return value is something that looks like `#<Dog:0x007fc52c2cc588>`. This syntax is called `Ruby Object Notation` and it's just the default way that Ruby communicates to you that you are dealing with an object or instance of a particular class.

We send the fido instance a message `.object_id` by separating the receiving object, fido and the message, object_id by a dot `.`. When we send an object a message through `dot notation`, we are evoking the corresponding method on the object. We are `calling` the object_id method on fido.

### INSTANCE Methods
We're used to defining methods already with the `def` keyword. If we place this method definition within the body of a class, that method becomes a **specific behavior of instances of that class**, not a generic procedure we can just call whenever we want. It represents an objectfs ability to have logic.

```ruby
class Dog
  # Class body
 
  # Instance Method Definition
  def bark
    puts "Woof!"
  end
end
 
fido = Dog.new
fido.bark #> "Woof!"
fido.sit # NoMethodError: undefined method `sit' for #<Dog:0x007fa4e9a9e8a0>

# Let's try just calling bark without fido
bark # NameError: undefined local variable or method `bark' for main:Object
```
We call the methods defined within the object's class Instance Methods because they are **methods that belong to any instance of the class**.

### INSTANCE variables
When we build objects through our own classes we know we can add behavior to the objects through instance methods. But how do we give our objects data?

```ruby
bro_greeting = "Sup, bro?"
```

The `bro_greeting` variable is what's known as a **local variable**, so named because it can only be accessed in a specific, local environment: they are **scoped to the method**.
A local variable that is defined inside one method, for example, cannot be accessed by another method. In order to get around this limitation, we can use **instance variables** inside our Ruby classes. Instance variable belongs to the instance itself / **the scope is to the instance itself**.

Note: `$variable` are global variables, but you shouldn't have to use them, they are for the interpreter itself.

**An instance variable is a variable that is accessible in any instance method in a particular instance of a class.**

```ruby
class Dog
  def name=(dog_name)
    this_dogs_name = dog_name
  end
 
  def name
    this_dogs_name
  end
end
```
Calling `lassie = Dog.new` and `lassie.name = "Lassie"` with `lassie.name` would not work and return `name: undefined local variable or method this_dogs_name`.
That is because `this_dogs_name` is a local variable. A local variable has a local scope. That means that it cannot be accessed outside of the method in which it is defined.

We define an instance variable by prefacing the variable name with an `@` symbol.

**Instance variables are bound to an instance of a class**. Instance variables hold information about an instance, usually an attribute of that instance. Think of instance variables as the containers for instance-specific information. 

Instance variables **can be called on throughout the class, without needing to be passed into other methods as arguments** (as would be the case with local variables).

```ruby
class Dog
 
  def name=(dogs_name)
    @this_dogs_name = dogs_name
    # we are casting the local variable to an instance variable, hoisting/raising the scope of dogs_name to @this_dogs_name
  end
 
  def name
    @this_dogs_name
  end
end
 
lassie = Dog.new
lassie.name = "Lassie"
 
puts lassie.name
```

## Object attributes
Let's define a `setter` method. A setter method is defined with an `=`, equals sign, appended to the name of the method. The `=` is followed by the `(argument_name)`.

```ruby
class Person
 
  def initialize(name)
    @name = name
  end
 
  def name
    @name
  end
 
  def name=(new_name)
    @name = new_name
  end
 
end

kanye = Person.new("Kanye")
 
kanye.name
  => "Kanye"
 
kanye.name = "Yeezy"
kanye.name
  => "Yeezy"
```

Our Person class' `#name` method is referred to as a `getter` or reader method. It returns information stored in an instance variable. In order to make a person's name attribute writable, we need to define a "setter" method.

#### Initializing happens at .new
We can initialize several attributes. **Initializing is making sure the object can do something at the moment it is instantiated**. `#initialize` is a hook / a callback / a lifecycle event, because it is a method automatically fired by another method (everytime you call `#new` on the Baby class).
```ruby
def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end
```

### Object accessors
In Ruby a macro is like a method, just some code, that instead of returning a Ruby datatype returns more Ruby code! **The implementation of macros is considered metaprogramming**: the writing of programs that operate on other programs. We'll implement macros to abstract away the manual, explicit definition of setter and getter methods in a Ruby class.

In object-oriented Ruby, there is a strong convention to have a `setter` and a `getter` method that pertain to the same attribute. For example, a `.name` getter and a `.name=` setter on our Person class. Because this pattern is so common, we find ourself making these manual setter and getter definitions again and again.

```ruby
class Person
  attr_reader :name
  attr_writer :name
end
```

The `attr_reader` macro, followed by the attribute name `:name`, created a getter method for us.
The `attr_writer` macro, followed by the attribute name `:name`, created a setter method for us.
We can now do the following, without explicitly defining a `.name` or `.name=` method ourselves

We can even make it shorter, instead of repeating ourselves (imagine we have 50 attributes for the model)
```ruby
class Person
  attr_accessor :name
end
```

This is the same as the explicit method definition 
```ruby
class Person
 
  def name=(name)
    @name = name
  end
 
  def name
    @name
  end
end
```
The usage of macros is preferred over the explicit definition of setter and getter methods, unless you need to customize the implementation of a method, like in our previous lesson when we defined .name as returning the first and last name variables combined.

We can define more
```ruby
class Person
  attr_accessor :name, :weight, :eye_color
  attr_writer :location # the name of the method is location=
  attr_reader :dna #should be initialized since cannot be written
end
```

## Object Lifecycle
We already know that any Ruby class can produce new instances of itself, via the `<Class Name>.new` method, whether or not that class has an `#initialize` method. However, **if we want each instance of our class to be created with certain attributes, we must define an** `#initialize` **method**. 

>An #initialize method is a method that is called automatically whenever `#new` is used.

A best practice is to say that **initialize is "closed to modification, but open to extension"**. Initialize should be the smallest peace of code, but we can extent its functionalities through other constructor class methods.

The initialize method is what's called a callback method, because it is automatically invoked every time the `#new` method is used to create a new instance of the class. You can also think of the initialize method as a constructor method. A constructor method is invoked upon the creation of an instance of a class and used to help define the instance of that class.

```ruby
class Dog
  def initialize(breed)
    @breed = breed
  end
  
  def breed=(breed)
    @breed = breed
  end
 
  def breed
    @breed
  end  
end 
```


Without `#initialize` we could not have a `@breed` attribute (instance variable), the object would only be `=> #<Dog:0x007fc014895900>`. As soon as we have the `#initialize` that sets `@breed = breed`, then `Dog.new("fido")` will return an object containing one attribute (the instance variable) `=> #<Dog:0x007fc0131a2b08 @breed="fido">`. We can then access/read the @breed instance variable or write it as soon as we have the `attr_accessor` set to `:breed`. 

Now we can call `.new` like this
```ruby
lassie = Dog.new("Collie")
 
lassie.breed #=> "Collie"
```

### Class constants
First, whenever we see `::`, it indicates "name spacing" (eg `Book::GENRES`). In other words, it tells us that the thing on the right side of the colons is a thing defined within the context, or name space, of the thing on the left side of the colons. In this case, this is telling us that there is a constant, GENRES (we know it's a constant because it's written in all caps) that is defined within our Book class: it's a `Class Constant`.

**Class Constants are available to all instances of a particular class**. Whereas instance variables, i.e. title, author, etc., are individual to each instance of a class, class constants are shared among all instances. They all have access to the same data, and if that data should change for some reason, all instances will know about that change.

```ruby
class Shoe
  attr_accessor :color, :size, :material, :condition
  attr_reader :brand

  BRANDS = []

  def initialize(brand)
    @brand = brand
    BRANDS << brand unless BRANDS.include?(brand)
  end

  def cobble
    self.condition = "new"
    puts "Your shoe is as good as new!"
  end
end
```

### Domain model
A domain model is a representation of real-world concepts in software. The concept of **domain modeling is key in object orientation**. In object orientation, we think of our classes as templates for objects. The instances of our classes are thought of as objects. For example, a Person class produces people objects that have attributes and behaviors, as described and enacted by instance methods.

An `AutoPlant` class that produces individual cars and has instance and class methods that handle things like `take_inventory` or `paint_cars`. Such a program not only represents a single object or concept but an entire environment––that of the auto factory. Consequently, this program could be understood as a domain model.

```ruby
class School
  attr_accessor :name, :roster

  def initialize(name)
    @name = name
    @roster = {}
  end

  def add_student(student_name, grade)
    roster[grade] ||= []
    # useful when necessary to create smth if doesn't exist, instead of `if roster[grade].nil?`
    roster[grade] << student_name
  end

  def grade(student_grade)
    roster[student_grade]
  end

  # this method should arrange the students in each grade by alphabetical order
  def sort
    sorted = {}
    roster.each do |grade, students|
    # interesting non-destructive method instead of .sort.collect and .sort!
      sorted[grade] = students.sort
    end
    sorted
  end
end
```

## Self
When we create a class, each new instance of a class is considered to be an object. Every object is aware of itself and we can define methods in which we tell objects to operate on themselves. We do so using the `self` keyword, inside the body of an instance method, **to refer to the very same object the method is being called on**. Every object is, quite literally, self aware.

```ruby
class Dog
  def showing_self
    puts self
  end
end

fido = Dog.new
fido.showing_self
# => #<Dog:0x007faf90a88cd8>
```

Inside the `#showing_self` method we use the self keyword. The self keyword refers to the instance, or object, that the #showing_self method is being called on (the `Dog` instance that is `fido`).

>The beauty of object-oriented programming is that we can **encapsulate**, or wrap up, attributes and behaviors into one object.

```ruby
def adopted(dog, owner_name)
  dog.owner = owner_name
end

# becomes

def get_adopted(owner_name)
    self.owner = owner_name
end
```
If `self` refers to the object on which the method is being called, and if that object is an instance of the `Dog` class, then we can call any of our other instance methods on self. Read more about [metaprogramming](http://yehudakatz.com/2009/11/15/metaprogramming-in-ruby-its-all-about-the-self/).

### 3 golden rules of Self
3 cases for the use of `self` ([read more](https://hackhands.com/three-golden-rules-understand-self-ruby/))

1. Use `self` when setting/getting instance attributes inside a class definition.
2. Use `self` to denote a method within the class definition as a class method.
3. Use `self` to reference the calling object within an instance method definition.

```ruby
def valid?
    self.status == "open" && self.balance > 0 ? true : false
end

#is equivalent to
def valid?
    self.status == "open" && balance > 0
end
```

## CLASS variables and Methods
The `Album` class can have its own variables and methods. We call these class variables and class methods. Let's say you wanted to keep a counter for how many albums you had in your music collection. The current code in our Album class has no way to keep such a count. Looks like we will have to write some code to accommodate this new feature of our program.

When it comes to adding new features or functionalities to our code, we start out by asking a question: whose responsibility is it to enact this behavior or functionality? It isn't the job of the individual albums, but the job of the `Album` class to keep a count of all of instances it produces.

A class variable looks like this: `@@variable_name`.

- **A class variable is accessible to the entire class––it has class scope**. 
- **A class method is a method that is called on the class itself**, not on the instances of that class. 

Class variables store information regarding the class as a whole and class methods enact behaviors that belong to the whole class, not just to individual instances of that class.

```ruby
class Album
  @@album_count = 0 
 
  def initialize
    @@album_count += 1
  end
 
 # this is a class method because it's called on self as the entire class
  def self.count
    @@album_count
  end
end


Album.new
Album.new
Album.new
 
Album.count
  # => 3
```

Here, the `self` keyword refers to the entire class itself, not to an instance of the class. In this case, we are inside the class only, not inside an instance method of that class. So, **we are in the class scope, not the instance scope**.

Here we are using the `@@album_count` class variable, inside of our #initialize method, which is an instance method. We are saying: when a new album is created, access the @@album_count class variable and increment its value by 1.

```ruby
class Song
  attr_accessor :name, :artist, :genre

  @@count = 0
  @@genres = []
  @@artists = []

  def initialize(name, artist, genre)
    @name = name
    @artist = artist
    @genre = genre
    @@count += 1
    @@genres << genre
    @@artists << artist
  end

  def self.count
    @@count
  end

  def self.artists
    @@artists.uniq
  end


  def self.genres
    @@genres.uniq
  end

  def self.genre_count
    genre_count = {}
    @@genres.uniq.each do |genre|
      genre_count[genre] =  @@genres.select {|i| i == genre}.length
    end
    genre_count
  end

  def self.artist_count
    artist_count =  {}
    @@artists.uniq.each do |artist|
      artist_count[artist] = @@artists.select {|i| i == artist}.length
    end
    artist_count
  end
end
```

or 

```ruby
  def self.genre_count
    genre_count = {}
    @@genres.each do |genre|
      if genre_count[genre]
        genre_count[genre] += 1 
      else
        genre_count[genre] = 1
      end
    end
    genre_count
  end
```
### Private methods
Private methods cannot be called by an explicit receiver.  It means we can only call private methods within the context of the defining class: the receiver of a private method is always `self`. Private methods restrict an outsider from calling methods that belong to an object.

```ruby
# initialize is already a private method
phil.initialize
#=>NoMethodError: private method `initialize' called for #<Bartender:0x007fafb4257dd8 @name="Phil">
```

Private methods are a way of encapsulating functionality within a class. For example, a bartender can make a drink at a customer's request. As a customer, you're permitted to ask the bartender for a drink (from a menu of options), but you can't instruct him or her on each step (choosing liquors, mixers, garnish, and stirring everything together). The smaller steps that make up the bartender's job can be considered private methods.

**Private methods also signal to other developers that this method is depended on by other methods in your program**. It signals that they should beware of removing such a method for fear of breaking other parts of the program that they may not realize rely on it.

Private methods, aside from initialize, are usually written with the word `private` above them.

```ruby
class Bartender
  attr_accessor :name
 
  BARTENDERS = []
 
  def initialize(name)
    @name = name
    BARTENDERS << self
    # we shovel the whole object to BARTENDER (not just name)
  end
 
  def self.all
    BARTENDERS
  end
 
  def intro
    "Hello, my name is #{name}!"
  end
 
  def make_drink
    @cocktail_ingredients = []
    choose_liquor
    choose_mixer
    choose_garnish
    # for those 3 methods, Ruby sees the missing receiver and assumes it to be self, or the current object, when each is called, self is an instance of Bartender
    return "Here is your drink. It contains #{@cocktail_ingredients}"
  end
 
  private
  def choose_liquor
    @cocktail_ingredients.push("whiskey")
  end
 
  def choose_mixer
    @cocktail_ingredients.push("vermouth")
  end
 
  def choose_garnish
    @cocktail_ingredients.push("olives")
  end
end


phil = Bartender.new("Phil")
nancy = Bartender.new("Nancy")
 
Bartender.all
#=> [#<Bartender:0x007f94cb16bbd0 @name="Phil">, #<Bartender:0x007f94cb16bb58 @name="Nancy">]

# private methods cannot be called by an explicit receiver
phil.choose_liquor
#=>NoMethodError: private method `choose_liquor' called for #<Bartender:0x007f9f5b03d318 @name="Phil">
```

## Object Models
### Remembering Objects in CLASS variables
Our application needs a way to store or remember a collection of class instances. Whether they are instances of a Game, User or Password class, all of these examples would require our program to keep track of instances that are created. Ruby allows us to do so by using class variables to store new instances as soon as they are created.

```ruby
class Song
  @@all = []
 
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @@all << self
  end
  
  def self.all
  # class method: self refers to the class on which the class method will be called (we are in the class scope)
    @@all.each do |song|
      puts song.name
    end
  end  
  
  def self.clear_all
    @@all.clear
  end  
end
```

In `#initialize` we use the `self` keyword to refer to the new object that has just been created by `#new`. Remember that when `#new` is called, it creates a new instance of the class and then calls `#initialize` on that new instance. So, `#initialize` is technically an instance method. Inside an instance method we are in what is called method scope and `self` will refer to whichever instance the method is being called on.

### Advanced Class Methods
#### Finders
`self.all` defines a class method that simply exposes the value in the class variable `@@all`. This is a `class reader`, very similar to an instance reader method that reads out of an instance-level property such as `@name`.

```ruby
class Person
  attr_accessor :name
  @@people = []
 
  def self.all
    @@people
  end
 
  def initialize(name)
    @name = name
    self.class.all << self
  end
 
  def self.find_by_name(name)
    self.all.detect{|person| person.name == name}
  end
  # self.all in the context of Person.find_by_name is equivalent to Person.all as the scope of the method is the class. 
  # self within a class method is the class itself.
  
end
 
Person.new("Ada Lovelace")
Person.all.detect{|p| p.name == "Ada Lovelace"} # Literal implementation, no abstraction or encapsulation
 
Person.find_by_name("Ada Lovelace") # Abstract implementation with logic entirely encapsulated.
```

We call class methods like `Person.find_by_name` 'finders'. **Finder class methods are responsible for finding instances based on properties or conditions**.

Whenever we use `Person.find_by_name` the intention of our code is revealed. Instead of iterating on an array, our code reads clearly. Instead of describing the implementation of finding a person by name, our code simply says what it is doing, not how. This is called an API. 

You want to build objects that provide a semantic and obvious API. Methods that reveal what the object will do, not how it does that. Always hide the how and show the what.

When we get a CSV (comma-separated values) we can make this first type of code:
```ruby
class Person
  attr_accessor :name, :age, :company
end
 
csv_data = "Elon Musk, 45, Tesla
Mark Zuckerberg, 32, Facebook
Martha Stewart, 74, MSL
"
 
rows = csv_data.split("\n")
people = rows.collect do |row|
  data = row.split(", ")
  name = data[0]
  age = data[1]
  company = data[2]
  person = Person.new
  person.name = name
  person.age = age
  person.company = company
  person
end
people #=> [#<Person @name="Elon Musk"...>, #<Person @name="Mark Zuckerberg"...>...]
```

In an ideal world every time we got CSV data we'd just want the **Person class to be responsible for parsing it**. Could we build something like `Person.new_from_csv`? Of course!

```ruby
class Person
  attr_accessor :name, :age, :company
 
  def self.new_from_csv(csv_data)
    # Split the CSV data into an array of individual rows.
    rows = csv_data.split("\n")
    # For each row, let's collect a Person instance based on the data.
    people = rows.collect do |row|
      # Split the row into 3 parts, name, age, company, at the ", " in the data.
      data = row.split(", ")
      name = data[0]
      age = data[1]
      company = data[2]
 
      # Make a new instance
      person = self.new # self refers to the Person class. This is Person.new
      # Set the properties on the person.
      person.name = name
      person.age = age
      person.company = company
      # Return the person to collect
      person
    end
    # Return the array of newly created people.
    people
  end
end
```

We don't want to put that functionality in the `initialize` method. Initialize should be closed to modification. 

> `initialize` should only handle the most required and common cases of initializing an object. Anything we add to initialize should be permanent and never modified. 

If we need more functionality when making an instance, instead of modifying initialize, we can extend it by wrapping it within a custom constructor.

#### Class operators
Beyond finders and custom constructors that return existing instances or create new instances, class methods can also manipulate class-level data.

```ruby
# Printing each person
Person.all.each do |person|
  puts "#{person.name}"
end

# let's encapsulate this logic within a class method
def self.print_all
  self.all.each{|person| puts "#{person.name}"}
end
```

Additionally, class methods might provide a global operation on data. Imagine that one of the csvs we were provided with has people's names in lowercase letters. We want proper capitalization. 

```ruby
  def self.normalize_names
    self.all.each do |person|
      person.name = person.name.split(" ").collect{|w| w.capitalize}.join(" ")
    end
  end
```
This can even become clearer by encapsulating the normalization task into the `Person` instance and not the class. We teach a `Person` instance how to properly convert its name into a capitalized version. The class method that acts on the global data of all people is simplified and delegates the actual normalization to the original instances.

```ruby
class Person
  attr_accessor :name
  @@all = []
  def self.all
    @@all
  end
 
  def initialize(name)
    @name = name
    @@all << self
  end
 
  def normalize_name
    self.name.split(" ").collect{|w| w.capitalize}.join(" ")
  end
 
  def self.normalize_names
    self.all.each do |person|
      person.name = person.normalize_name
    end
  end
  
  def self.destroy_all
    self.all.clear
  end  
end
```
For more about [Object Models](https://www.youtube.com/watch?v=vENMFapLonA) or [Object Orientation](https://www.youtube.com/watch?v=Z_IoQCVNWtM).

## Object Relationships
So far, we've defined classes that create objects that do not interact with other objects that we've created. In object-oriented programming, however, we write programs that model real-world situations and environments, as well as real-world relatedness: 
- users are associated through friendship, 
- users are associated to shelters and to pets, 
- users are associated to tweets and users might even be associated to each other.

### Belongs To
Let's say we have a Song class that produces individual song objects. Each song belongs to the artist that wrote it. We can build that relationship by creating an `attr_accessor` in the `Song` class for `artist`.

```ruby
class Song
  attr_accessor :title, :artist
  # we have a setter and getter for a song's artist attribute
 
  def initialize(title)
    @title = title
  end
end

class Artist
  attr_accessor :name, :genre
 
  def initialize(name, genre) 
    @name = name
    @genre = genre
  end
 
end
 
drake = Artist.new("Drake", "rap")
hotline_bling = Song.new("Hotline Bling")
hotline_bling.artist = drake
# drake is not a string it is an object

hotline_bling.artist.genre
  # => "rap"
  
hotline_bling.artist.name
  # => "Drake"  
```
Now our relationship between songs and their artists is complete. This is called the `belongs to` relationship. A song can only have one artist (at least in our domain model), so we say that a song "belongs to" an artist. 

The benefit here is that in setting the `artist=` method **equal to a real instance of the Artist class, instead of equal to a simple string**, we are **associating our song to a robust object** that has its own attributes and behaviors.

### Has Many
The inverse of the "belongs to" relationship is the "has many" relationship. If a song belongs to an artist, then an artist should be able to have many songs. 

Having many of something means you own a collection of that thing. Ruby offers us a great way to store collections of data in list form: arrays.

```ruby
class Song
  attr_accessor :artist, :name, :genre
  # each song belongs to the artist that wrote it
 
  def initialize(name, genre)
    @name = name
    @genre = genre
  end
end

class Artist
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
 
  def add_song(song)
    @songs << song
    @song.artist = self
  end
 
  def songs
    @songs
  end
  
  def list_genres  
    self.songs.collect do |song|
        song.genre
    end
  end

  def add_song_by_name(name, genre)
    song = Song.new(name, genre)
    @songs << song
    song.artist = self
  end
  
  def artist_name
    self.artist.name
  end  
end

ninetynine_problems = Song.new("99 Problems", "rap")
crazy_in_love = Song.new("Crazy in Love", "pop")
 
jay_z.add_song(ninetynine_problems)
jay_z.add_song(crazy_in_love)

jay_z.songs
# =>[#<Song:0x007fa96a878348 @name="99 Problems", @genre="rap">, #<Song:0x007fa96a122580 @name="Crazy in Love", @genre="pop">]

jay_z.list_genres
# => ["rap", "pop"]
  
crazy_in_love.artist.name
# => "Jay-Z"

crazy_in_love.artist_name
# => "Jay-Z"
```

### Collaborating objects
Objects can be related to one another directly, when one object contains a reference to another: a bank account and transactions for instance.

Domain modeling is the description of what you want the app to do 
1. Identify your objects
I want to build and app that can load artists, songs and genres, for each of them it can generate an index.html that lists all of the items in the collection and then it can also generate a "show.html" (kanye-west.html) for each individual in the collection.

2. Identify single responsibilities for each model
I need different machines that just do one thing, have encapsulated responsibilities
- Artist, that is responsible for 
-   name
-   songs
-   genres
- Song, responsible for
-   name
-   genre
-   artist
- Genre responsible for 
-   name
-   songs
-   artists
- Site generator is responsible for
-   template like index.html
-   items in the collection

3. Design the interface
Design what you should be able to do (even if you don't know yet how you'll get there)
SiteGenerator.new(Artist.all, "views/artists/index.html", "_site/artists/index.html")
Here we pass in the collection, the template, and where I want the template to be generated

4. Write the code and refactor
class SiteGenerator
    def initialize(collection, template, destination)
    end
end


Video available online on [youtube](https://www.youtube.com/watch?v=iYcQ693LXck)
```ruby
class Author
  attr_accessor :name

  def initialize
    @stories = [] # has_many stories interface
  end

  def stories # has_many stories interface
    @stories.dup.freeze
    # through .dup.freeze we return a duplicate frozen
    # meaning you can't push author.stories << "blabla" 
    # because we force users to add_story through our own interface
  end

  def add_story(story) # has_many stories interface
    raise AssociationTypeMismatchError, "#{story.class} received, Story expected." if !story.is_a?(Story)
    @stories << story
    story.author = self unless story.author == self
  end

  def bibliography
    self.stories.collect{|s| s.name} # You need all stories to be instances of Story because they must respond to #name to work
    # @stories.collect(&:name) #=> Symbol to Proc, less liked by rubyist because "magical" 
  end

  def categories # has_many categories through stories
    self.stories.collect{|s| s.category}.uniq
  end
end
```
And on the Story side
```ruby
class Story
  attr_accessor :name
  attr_reader :author # Belongs to author

  def author=(author) # Belongs to author
    raise AssociationTypeMismatchError, "#{author.class} received, Author expected." if !author.is_a?(Author)
    @author = author
    author.add_story(self) unless author.stories.include?(self)
  end

  attr_reader :category # Belongs to category

  def category=(category) # Belongs to category
    raise AssociationTypeMismatchError, "#{category.class} received, Category expected." if !category.is_a?(Category)
    @category = category
    category.add_story(self) unless category.stories.include?(self)
  end

end
```

As a side note, the `AssociationTypeMismatchError` is defined in `config/environment.rb` and is a `TypeError`
```ruby
require 'pry'

require_relative '../lib/author'
require_relative '../lib/category'
require_relative '../lib/story'

class AssociationTypeMismatchError < TypeError; end
```
And also include `require_relative '../config/environment'` on top of the `spec/spec_helper.rb`

### Has Many Through
We now know the has_many / belongs_to relationship. A song belongs to an artist and an artist has many songs.
```ruby
class Song
  attr_accessor :name, :artist
end

class Artist
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
 
  def add_song(song)
    @songs << song
    song.artist = self
  end
end
```

However, in the real-world, different entities can be connected to one another indirectly as well as directly. For example, in the real world, individual songs belong to a genre.

Let's give the ability for our `Song` instances to belong to a genre. 
```ruby
class Song
  attr_accessor :name, :artist, :genre 
end
```
We also need to build a `Genre` class, so that we can associate individual songs to complex genre objects that can contain other information pertinent to a given genre.
```ruby
class Genre
  attr_accessor :name
 
  def initialize(name)
    @name = name
  end
end
```
Now we can associate a `Song` to a `Genre` by typing `song.genre=` thanks to the `attr_accessor`. Now let's also make it possible to set the `Genre` at initialization.
```ruby
class Song
  attr_accessor :name, :artist, :genre
 
  def initialize(name, genre)
    @name = name
    @genre = genre
  end
end
```
We have an Artist class that produces individual artists that have many songs. We have a Song class that produces individual songs that belong to an artist and belong to a genre.
![has many](http://readme-pics.s3.amazonaws.com/Screen%20Shot%202015-11-03%20at%2012.23.17%20PM.png)

Here we can see that an artist has many songs and that each song belongs to one artist and one genre. This diagram makes it clear to us that an artist does have a connection to genres. That connection exists through the many songs that an artist owns. This is the "has many through" relationship. A given object has many of another type of object. That second object belongs to (or has many) of a third type of object. Therefore, the first object has many of the third object as well.

An artist has a genre through the songs that it has created, so in order to ask an artist about its genre, we have to go through that artist's songs. We'll need to collect all of the songs of a given artist and then collect the genre associated to each of those songs.

```ruby
class Artist
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
 
  def add_song(song)
    @songs << song
    song.artist = self
  end
 
  def songs
    @songs
  end
 
  # we add this instance method
  def genres
    self.songs.collect do |song|
      song.genre
    end
  end
end
```
The return value of the #genres method should be an array of genre objects.

Right now, an artist can tell us about its songs and about its genres. But, a genre can't tell us about its songs and its artists. Let's fix that now.
```ruby
class Genre
  attr_accessor :name
 
  def initialize(name)
    @name = name
    @songs = []
  end
 
  def songs
    @songs
  end
 
  # to add a song to the genre
  def add_song(song)
    @songs << song
  end
  
  # to list artists
  def artists
    @songs.collect do |song|
      song.artist
    end
  end  
end
```
And we just need to to refactor the `Song` class.
```ruby
class Song
  attr_accessor :name, :artist, :genre
 
  def initialize(name, genre)
    @name = name
    @genre = genre
    genre.add_song(self)
  end
end
```

## Object Architecture
In the real-world, different entities (people, animals, cars, you name it) are related in various ways. You are writing a web application in which users are either admins, instructors or students. All of these entities are "users" and have common features, but they all have some unique traits as well.

Well, we could write separate admin, instructor and student class that each contain repetitious code to lend each of these classes shared attributes and behaviors.

Instead, we can use inheritance. The use of inheritance allows us to create a family of classes with shared behavior, while still differentiating those classes. With inheritance, we could inherit the admin, instructor and student classes from a user class. Then, any changes made to the user class would apply to the other class.

While you may not write your own classes that use inheritance very frequently, you will encounter it frequently as a Ruby on Rails web developer.

In Ruby, classes can inherit from one another. This means that they adopt all of the attributes and behaviors (i.e. all of the methods) of the parent, also called the `super` class.

In this domain model, we have class `Vehicle` that will act as the parent, or **super**, class. We will create child classes, also known as **subclasses** for different types of Vehicles, such as `Car`.

```ruby
class Vehicle
  attr_accessor :wheel_size, :wheel_number
 
  def initialize(wheel_size, wheel_number)
    @wheel_size = wheel_size
    @wheel_number = wheel_number
  end
 
  def go
    "vrrrrrrrooom!"
  end
end

class Car < Vehicle
  def go
    "VRRROOOOOOOOOOOOOOOOOOOOOOOM!!!!!"
  end
end
```

### Modules and Mixins
Inheriting one class from another makes sense. The subclass can be understood as a child or subordinate of the super class. For example, a car is a type of vehicle, so it makes sense for the `Car` class to inherit from the `Vehicle` class.

Modules allow us to collect and bundle a group of methods and make those methods available to any number of classes. For example, we'll be defining a `Dance` module and making it available to both the `Dancer` and `Kid` class.

```ruby
module Dance
  def twirl
    "I'm twirling!"
  end
 
  def jump
    "Look how high I'm jumping!"
  end
end
```
If we use `include` keyword, we allow our classes to use all of the methods of the included module **as instance methods**. 
```ruby
class Kid
  include Dance
 
  attr_accessor :name
 
  def initialize(name)
    @name = name
  end
end

# it enables us to call module's methods as instance methods
angelina = Dancer.new
angelina.twirl
// returns "I'm twirling!"
```
In order to lend a module's methods to a class **as class methods**, we use the `extend` keyword.

```ruby
module MetaDancing
  def metadata
    "This class produces objects that love to dance."
  end
end
````
And now in the Dancer class file
```ruby
require_relative './dance_module.rb'

class Dancer
  extend MetaDancing
end

# this enables us to call a class method from the module
puts Dancer.metadata
```
There are two drawbacks to this approach. 
- First, if another developer looks at your modules, there is absolutely no way to determine how those methods are intended to be used. Are they class methods? Are they instance methods? Nobody knows!
- Secondly, we had to build two separate modules that contained methods that were all related to the same functionality (dancing). But because there was no way to designate class methods versus instance methods, we were forced to define two separate modules, which violates the single responsibility principle. 

We're going to refactor the two modules into one, and use nested module namespacing to clarify our code.
```ruby
module FancyDance
  module InstanceMethods
    def twirl
      "I'm twirling!"
    end
  end
 
  module ClassMethods
    def metadata
      "This class produces objects that love to dance."
    end
  end
end
```
Then to use these nested modules
```ruby
require_relative './dance_module.rb'

class Dancer
  extend FancyDance::ClassMethods
  include FancyDance::InstanceMethods
end
```
We refer to the name-spaced modules or classes with `::`. The Parent::Child syntax is called namespacing, and references the parent and child relationship of the nested modules.

Inheritance using the `<` syntax, implies that a class is a type of something. A `BMW` class should inherit from a `Car` class because a `BMW` is **a type of** car: class BMW < Car.

The `::` syntax just denotes a name-space. Doing `BMW::Car` just gives the `BMW` class access to all constants, instance methods, etc, without stating that a BMW is a type of `Car`. The `::` syntax carries all public items over to the inheriting class or module.

It is a common practice to create a folder called `concerns` that holds modules that will be used across classes in an object oriented Ruby project.

It is often useful to use `self.class.all` to refactor an `.initialize` (instance method) method. When we include a module in a class, we are really telling that class to inherit methods from that module.

So, we can use the super keyword to tell our Artist's .initialize method to use the code in the Memorable::InstanceMethods module's .initialize method and also to use any additional code in the Artist's .initialize method. The super keyword, placed inside a method, will tell that method to look up its behavior in the method of the same name that lives in the parent, or super, class.

So, we can use the super keyword to tell our Artist's .initialize method to use the code in the Memorable::InstanceMethods module's .initialize method and also to use any additional code in the Artist's .initialize method.
```ruby
class Artist
  include Memorable::InstanceMethods
  ...
 
  def initialize
    super
    @songs = []
  end
```
This code `super` keyword will look into the `Memorable::InstanceMethods` to find and add the original code
```ruby
module Memorable
  module InstanceMethods
    def initialize
      self.class.all << self
    end
  end
end
```

#### Note about the environment
Instead of requiring individual files within one another, we created an environment file to handle those requirements for us. Any additional files we make should be required by this environment.rb file.
```ruby
require 'pry'

require_relative '../lib/concerns/findable'
require_relative '../lib/concerns/memorable'
require_relative '../lib/concerns/paramable'

require_relative '../lib/artist.rb'
require_relative '../lib/song.rb'

```
Our spec_helper file, which is required by each individual spec file, required only this config/environment.rb file, instead of each and every file from the lib directory. As we start to build larger and more complex programs, it begins to make sense to handle all of our requirements in one place.

#### About tests
We use TDD (test-driven development) for a reason. We write tests to define the desired behavior of our program so that we can write clean, beautiful code. We can first write code that passes those tests and then break our code, fail our tests, write better code and pass our tests again.

This is called the **red, green, refactor** pattern. First tests fail, then you write bad code to get them to pass, then you refactor that bad code into good code. Remember, don't be afraid of broken code! Broken code is the status quo in programming. Your job is often to break something to make it better. Embrace broken code.

### Super
So far, we've seen the benefits of using inheritance to create a group of classes that share certain characteristics and behaviors. However, up until now, the implementation of shared characteristics has been somewhat rigid.
But what if there is a method in the parent class that we want our child to share some of the functionality of? Or what if we want our child class to inherit a method from the parent and then augment it in some way? We can achieve this with the use of the `super` keyword.

Let's say we are working on an education app in which users are either students or teachers. We have a parent, User class, that both our Student and Teacher classes inherit from.

```ruby
class User
  def log_in
    @logged_in = true
  end
end
```
However, when a student logs into our app, we need to not only set their logged in attribute to true, we need to set their "in class" attribute to true. We can augment/supercharge, the `#log_in` method inside of the Student class.
```ruby
class Student < User
  def log_in
    super
    @in_class = true
  end
end
```

## Mass Assignment
At this point, we're very familiar with the fact that methods can be defined to take in arguments. We also know that methods can be defined to take in multiple arguments: `method(argument1, argument2)`. As it currently stands, whoever uses our method needs to remember exactly what order to pass in the arguments. Clearly, we have a need to regulate the passing in of multiple arguments, otherwise we risk to break our methods if one argument is used as the other.

Keyword arguments are a special way of passing arguments into a method. They behave like hashes, pairing a key that functions as the argument name, with its value.
```ruby
# with default argument
def happy_birthday(name: "Beyonce", current_age: 31)
  puts "Happy Birthday, #{name}"
  current_age += 1
  puts "You are now #{current_age} years old"
end

# without default arguments
def happy_birthday(name:, current_age:)
  puts "Happy Birthday, #{name}"
  current_age += 1
  puts "You are now #{current_age} years old"
end
```
Our keyword arguments consist of two key/value pairs, `:name` and `:current_age`. Even if we change the order of our key/value pairs, our method won't break.

Another benefit of using keyword arguments is the ability to "mass assign" attributes to an object. If a method is defined to accept keyword arguments, we can create the hash that the method is expecting to accept as an argument, set that hash equal to a variable, and simply pass that variable in to the method as an argument.

```ruby
person_attributes = {name: "Sophie", age: 26}
sophie = Person.new(person_attributes)
=> #<Person:0x007f9bd5814ae8 @name="Sophie", @age=26>
```

### Mass assignment and metaprogramming
Let's say we want to use the Twitter API to create users for our own application. The scenario is that we are developing a web application and we want our users to be able to sign in via Twitter. Thus, our own users are pulled from Twitter and we need to take the data we get from Twitter––for example a user's name, age and location, and use them to make instances of our own User class. 
```ruby
class User
  attr_accessor :name, :age, :location, :user_name
 
  def initialize(user_name:, name:, age:, location:)
    @user_name = user_name
    @name = name
    @location = location
    @age = age
  end
end
```
For the purposes of this example, we won't get into the specifics of how we request and receive data from the Twitter API. Suffice to say that we send a request to the Twitter API and get a return value of a hash full of user attributes.
```ruby
twitter_user = {name: "Sophie", user_name: "sm_debenedetto", age: 26, location: "NY, NY"}
```
we can use the twitter_user hash to instantiate a new instance of our own User class:
```ruby
sophie = User.new(twitter_user)
 => #<User:0x007fa1293e68f0 @name="Sophie", @age=26, @user_name="sm_debenedetto", @location="NY, NY">
```
But, what if Twitter changes their API without telling us? What if they don't send the age anymore, or add parameters? Our program would break as it is.
We can improve the `User` class, by asking our initialize method to take in some unspecified `attributes` object. The name of the key becomes the name of a setter method and the value associated with the key is the name of the value you want to pass to that method.
```ruby
class User
  attr_accessor :name, :user_name, :age, :location, :bio
 
  def initialize(attributes)
    attributes.each do |key, value| 
        self.send("#{key}=", value)
    end
  end
end
```
The `.send` method calls the method name corresponding to the key's name, with an argument of the value. `self.send(key=, value)` is the same as `instance_of_user.key = value`. The .send method is just another way of calling a method on an object. `sophie.name = "Sophie"` is the same as `sophie.send("name=", "Sophie")`, which is the same as `.sophie.name=("Sophie")`.

```ruby
"Hello".send("reverse")
=> "olleH"
```

With this pattern, we have made our code much more flexible. We can easily alter the number of attributes in the class and change the hash that we initialize the class with, without editing our initialize method. Now, we're programming for the future. If and when that data with which we want to initialize our class changes, we only have to change our attr_accessors.

### Custom Errors
We already know that Ruby has a hierarchy of error, or `Exception`, classes, all of which inherit from the Exception class, like `NoMethodError`, `ArgumentError`, `SyntaxError`...
```ruby
Exception
 NoMemoryError
 ScriptError
   LoadError
   NotImplementedError
   SyntaxError
 SignalException
   Interrupt
 StandardError
   ArgumentError
   IOError
     EOFError
   IndexError
   LocalJumpError
   NameError
     NoMethodError
   RangeError
     FloatDomainError
   RegexpError
   RuntimeError
   SecurityError
   SystemCallError
   SystemStackError
   ThreadError
   TypeError
   ZeroDivisionError
 SystemExit
 fatal
```
Let's say, however, that we are working on a web application in which users can sign in and post pictures to Instagram. But wait! Instagram has been hacked and their entire site is currently down! Since our app relies on sending data to and getting a response from the Instagram site, our app will break and our users won't know why. We can use custom error messages and custom error handling. By handling these custom errors in a particular way, we can soothe our users by redirecting them somewhere useful, showing them some kind of clear and apologetic notice.

To build a custom error, we define an error class that inherits from the `Exception` class. Which class your custom error inherits from will likely depend on the situation in which you want to raise it. However, it is usually a pretty safe bet to inherit your custom error class from the `StandardError` class.

```ruby
class Person
  attr_accessor :name, :partner
 
  def initialize(name)
    @name = name
  end
 
  def get_married(person)
    self.partner = person
    person.partner = self
  end
end
```
Imagine we're trying to tell Beyonce to get_married to "Jay-Z". The problem is that "Jay-Z" is a string, not an instance of the Person class. We would get an error: 
```ruby
custom_errors.rb:10:in `get_married': undefined method `partner=' for "Jay-Z":String (NoMethodError)
```
Let's define a custom error class, `PartnerError`
```ruby
class PartnerError < StandardError
end
```
We have a couple of options. We can simply place the above code inside of the Person class. We could define it outside of our Person class. Or, we can create a module and include that module inside the Person class. 

```ruby
class Person
  attr_accessor :partner, :name
 
  def initialize(name)
    @name = name
  end
 
  def get_married(person)
    self.partner = person
    if person.class != Person 
      raise PartnerError 
    else
      person.partner = self
    end
  end
 
  class PartnerError < StandardError
  end
end
```
This time the error we'd get would be
```ruby
custom_errors.rb:11:in `get_married': Person::PartnerError (Person::PartnerError)
```
We raised our very own custom error. However, our program is still broken. Notice that the puts beyonce.name line at the bottom of our file won't run because it follows the get_married method call, and we called that method in such a way as to raise an error. If only there was a way for us to `rescue` our program when such an error is raised and allow it to keep running...

```ruby
class Person
  attr_accessor :partner, :name
 
  def initialize(name)
    @name = name
  end
 
  def get_married(person)
    self.partner = person
    if person.class != Person
          begin
            raise PartnerError
          rescue PartnerError => error
              puts error.message
          end
    else
      person.partner = self
    end
  end
 
  class PartnerError < StandardError
    def message 
      "you must give the get_married method an argument of an instance of the person class!"
    end
  end
end
 
beyonce = Person.new("Beyonce")
beyonce.get_married("Jay-Z")
puts beyonce.name
```
Now, not only is our custom error message put out, but the program continues to run and will execute the puts beyonce.name line.

## Gems and Bundler
Nothing you ever write will be 100% your code. As a developer there is a new set of outside code you will work with: Libraries. Libraries (or "gems" in Ruby parlance) are just bundles of code that someone else wrote for you to integrate into your code base: Spec for instance.

2 ways to install a gem:
- One way of installing it would be to run `gem install mail` in your terminal and then require the gem in the file where you want to use that gem with `require 'mail'`.
- The other way, assuming you have an application, is to add the following to your Gemfile: 
```ruby
gem 'mail', '~> 2.6', '>= 2.6.3'
```

If you type `gem env` you see your gems and INSTALLATION DIRECTORY tells you where they are located. They are managed by rvm and are shared resources between the projects of your machine.

Let's take the first part of the versioning `'~> 2.6'`. A major version change is reflected by the first number (reading from left to right). Major version changes don't have to be backwards compatible. This means that if your app is built using version 1, and the gem updates to version 2, the new version can potentially break your app. A minor version change is reflected by the number after the first decimal point. All minor version changes have to be backwards compatible. The number after the second decimal point reflects a patch, which is a change to a gem to fix a bug but not introduce new functionality. 

`1.2.3` means major version 1, minor version 2, and a patch version 3.

The `~>` means any minor version change above the one listed. `'~> 2.6'` means any minor version above 2.6. 2.7, 2.8, and 2.9 would work (including patches); but version 3.0 wouldn't work because it indicates a new major version.

The mail gem has a second specification `'>= 2.6.3'`. This means any version greater than or equal to 2.6.3. Because the mail gem has two specifications, both have to be true, so this gem couldn't use version 2.6 because it's lower than 2.6.3.

### Gemfile
The Gemfile is a list of gems your app uses. The Gemfile lets you setup groups, so gems are only loaded under specific circumstances. For example, you might have a gem like Pry in your `development` group because you only need to use Pry to debug when you are in the development phase. Your code in `production`, i.e. when your app is being used by a user, doesn't need to use the Pry gem.

To create the gemfile you just have to type in your directory `bundle init`

**GROUP SYNTAX**
The group syntax uses the keyword `group`, followed by the app environment as a symbol (`:development`, `:test`, and `:production` are the standard environments), followed by the keyword `do`. Inside the block, we list all the gems specific to that group.
```ruby
source "https://rubygems.org"
 
gem "sinatra"
gem 'nokogiri'
gem 'rails', '3.0.0.beta3'
gem 'rack',  '>=1.0'
gem 'thin',  '~>1.1'
gem 'my_gem', '1.0', :source => 'https://gems.example.com'

 
group :development do
  gem "pry"
end
```
There are a lot of gems specific to the testing environment, like rspec and capybara.

**HASH SYNTAX**
You can also specify the gems with the following syntax
```ruby
gem "pry", :group => "development"
```

### Bundler
Bundler is a way to handle code dependencies. Imagine you're writing an amazing app. This app, being built on the shoulders of giants (gems), requires OTHER code to work. One change to a dependency can completely break your app. We can remedy this by enforcing that we use a specific version of the gem. But this, too, has its problems. How do you enforce this? Are you going to trust that everyone has the right version?

Bundler handles all of this for you. It provides you with a Gemfile where you can keep your requirements in one place. The Gemfile creates a single place for gems to be required and versions to be specified.
- Need the Sinatra gem for your project? Add `gem 'sinatra'` to your Gemfile.
- Need the Sinatra gem, but at version 1.4.5? Add `gem 'sinatra', '1.4.5'` to your Gemfile.
- Need the Sinatra gem at a version higher than 1.4, but less than 1.5? Add `gem 'sinatra', '~> 1.4.0'`

With this, you can make sure everyone working on your app is using the right version.

Getting started with Bundler is super easy. To create a `Gemfile`, type `bundle init` in your terminal. If a gemfile already exists it will prompt an error:
```ruby
// bundle init                                          
Gemfile already exists at /home/brunoboehm-26262/code/labs
/using-bundler-v-000/Gemfile 
```

When you run `bundle` it will also create the `Gemfile.lock` file

There's only one file Bundler requires you have (Gemfile), the other files are conventional for a typical Ruby application, but not required by the use of Bundler for gem management.
- Gemfile - This file is required by Bundler and contains a source, and a list of file requirements. That's all.
- config/environment.rb - The environment file is where we'll be loading all of our app's dependencies, from gems, data, other classes... to database connections. In other files you don't have to say `require`.
- bin/run.rb - This file will start our application. This file will require the environment file we created earlier to provide our app with access to our gems.

**CONFIG ENVIRONMENT**
This means we'd want the gems to be loaded in our app before our own code. If we loaded our code first, we'd get uninitialized constant errors or undefined variable or method errors. Load order matters. We can specify load information in config/environment.rb to configure our load path (or load order) so that nothing breaks.
```ruby
require 'bundler/setup'
Bundler.require
# this line requires/loads the gems into memory, not just on the computer
# you can also be more specific and add Bundler.require(:default, :development)
```
In the example above, we're first requiring 'bundler/setup'. If we don't do this, our app won't know to use bundler to install our gems. Without that line, our Gemfile becomes pointless.

**BIN/RUN.RB**
This is where the action is. This is where our app logic goes, and where we make our millions. To take advantage of all of the work we did in the `environment` file, let's require it here (and environment will require all other necessary files).
```ruby
# !/usr/bin/env ruby
require_relative '../config/environment'

# whatever method should be called next to run the program
MusicLibraryController.new.call
```

`require` and `require_relative` methods might look similiar they do different things. Both load a file based on the filename passed in as a parameter and return true if the file was found and loaded successfully and they will raise a LoadError if it returns false. However...
- `require` takes an **absolute path** for the filename, so the file must either be in the directory from which the application is being run or in one of the directories in your shell's PATH variable (which often includes the directory containing the gems you've installed).
- `require_relative` takes a **relative path** that is relative to the file in which the require statement is called (so it's relative to the file being run, not to the directory from which the code is being called).


## Scraping, Open URI & Nokogiri
Web scraping is the act of parsing a web page's HTML and pulling, or "scraping" pertinent data from that HTML. One of the most common use-cases for web scraping involves you, the programmer, scraping data that you will then use to instantiate your own Ruby objects.

Scraping can be difficult to accomplish––in order to get the data we want, we need to closely examine the HTML and identify exactly which elements contain the information we're interested in. It requires a high degree of precision.

So, if scraping is so tricky, why do we use it? Well, not all of the data we might be interested in using to program is available to use through APIs.

### Open URI
Open-URI is a module in Ruby that allows us to programmatically make HTTP requests. It gives us a bunch of useful methods to make different types of requests, but for this guide, we're interested in only one: open. This method takes one argument, a URL, and will return to us the HTML content of that URL.
```ruby
html = open('http://www.google.com')
```
This stores the HTML of Google into a variable called html. (More specifically, it actually stores the HTML in a temporary file that we can then call read on to get the raw HTML.). 
Note that 'open-uri' is part of the ruby standard library, we don't need to add it as a gem, only to require it.

### NOKOGIRI
Nokogiri is a Ruby gem that helps us to parse HTML and collect data from it. Essentially, Nokogiri allows us to treat a huge string of HTML as if it were a bunch of nested nodes. Nokogiri offers you, the programmer, a series of methods that you can use to extract the desired information from these nested nodes. Nokogiri makes the level of precision required to extract the necessary data much easier to attain.

Let's set up a small CLI program.
Let's start by creating a directory `mkdir Nogokiri` and inside create the following architecture, creating files with `touch file_name`.
Note the Gemfile is created by typing `bundle init` so that it gets filled with preformated info.
```ruby
Nokogiri/
    bin/
        run.rb
    config/
        environment.rb
    Gemfile
    
# In the Gemfile
    source "https://rubygems.org"
    gem 'nokogiri'

# In environment.rb
    require 'open-uri'
    # OpenUri is part of the Ruby standard library, that's why we only need to require it 
    
    require 'bundler/setup'
    Bundler.require(:default)
    #no need to require 'nokogiri' because it is in the default
    # if we had a /lib/ folder we could require all by `require_all 'lib'` if you have the require_all gem
    
# In run.rb
    require_relative '../config/environment'

    html = open("https://flatironschool.com/")
    doc = Nokogiri::HTML(html)
    
    selected = doc.css(".site-hero__headline").text
    
    puts selected
```
Nokogiri's `.css` method can be called on the doc variable that we set equal to that giant string of HTML that Nokogiri retrieved for us. The .css method takes in an argument of the CSS selector you want to retrieve. 

### Iterating over scraped elements
```ruby
require 'nokogiri'
require 'open-uri'
 
html = open("http://flatironschool.com/team")
doc = Nokogiri::HTML(html)
 
instructors = doc.css("#instructors .team-holder .person-box")
instructors.each do |instructor| 
  puts "Flatiron School <3 " + instructor.css("h2").text
end
```
When we use Nokogiri methods, we get a return value of XML elements, collected into an array. Even though the Nokogiri gem returns a Nokogiri::XML::Element (which looks like an array in ruby), we can use Ruby methods, such as .each and .collect, to iterate over it. The main thing to understand, however, is that Nokogiri collects these objects into hierarchical data structures. So, we could iterate over an array of Nokogiri objects, use enumerators, grab the values of attributes that act as hash keys, etc.

#### Single responsibility principle SRP
Each method, piece of code shouldn't be doing more than one thing!

#### Explaining .tap
```ruby
# Inside of the Animal class, with attr_accessors and initialize method

def self.new_from_wikipedia(url)
    animal = Animal.new
    
    properties = AnimalScraper.wikipedia(url)
    
    properties.each do |k, v|
        animal.send("#{k}=", v) #mass assignment
    end
    
    animal
end

# We can refactor using .tap that returns the Animal object that you tapped, and also eliminates the local variable
def self.new_from_wikipedia(url)
    Animal.new.tap do |animal|
        AnimalScraper.wikipedia(url).each do |k, v|
            animal.send("#{k}=", v)
        end
    end
end
```
And of course we have in another file the `AnimalScraper` class
```ruby
class AnimalScraper
    def wikipedia(url)
        doc = Nokogiri::HTML(open(url))
        animal = {}
        
        animal[:name] = doc.search("h1#firstHeading").text
        animal[:family] = doc.search("span.family").text
    end
end
```

#### Example with Flatiron School website
Let's first define a Course class

```ruby
class Course
  attr_accessor :title, :schedule, :description
  @@all = []
 
  def initialize
    @@all << self
  end
 
  def self.all
    @@all
  end
 
  def self.reset_all
    @@all.clear
  end
end
```

And now let's create our Scraper class
```ruby
require 'nokogiri'
require 'open-uri'
# require 'pry'
 
require_relative './course.rb'
 
class Scraper
 
  def get_page
    Nokogiri::HTML(open("http://learn-co-curriculum.github.io/site-for-scraping/courses"))
    # binding.pry
    # we can do scraper = Scraper.new.get_page
  end
 
  def get_courses
    self.get_page.css(".post")
    # we can do scraper.get_courses ==> scraper.get_page.css(".post")
  end
 
  def make_courses
    # scraper.make_courses ==> scraper.get_courses.each...
    self.get_courses.each do |post|
      course = Course.new
      course.title = post.css("h2").text
      course.schedule = post.css(".date").text
      course.description = post.css("p").text
    end
  end
 
  def print_courses
    self.make_courses
    Course.all.each do |course|
      if course.title
        puts "Title: #{course.title}"
        puts "  Schedule: #{course.schedule}"
        puts "  Description: #{course.description}"
      end
    end
  end
 
end
 
Scraper.new.print_courses
```

#### Scraping images
An image tag in HTML is considered to have a source attribute. In the following example `<img src="http://www.example.com/pic.jpg">`
the source attribute would be "http://www.example.com/pic.jpg". You can use the `.attribute` method on a Nokogiri element to grab the value of that attribute.
```ruby
html = File.read('./fixtures/kickstarter.html')
kickstarter = Nokogiri::HTML(html)
image = kickstarter.css(".project-card").first.css("div.project-thumbnail a img").attribute("src").value
```
Example in the full exercise
```ruby
require 'nokogiri'
require 'pry'

def create_project_hash
  html = File.read('./fixtures/kickstarter.html')
  kickstarter = Nokogiri::HTML(html)

  projects = {}

  kickstarter.css(".project-card").each do |project|
    title = project.css("h2.bbcard_name strong a").text
    projects[title.to_sym] = {
      image_link: project.css("div.project-thumbnail a img").attribute("src").value,
      description: project.css("p.bbcard_blurb").text,
      location: project.css("span.location-name").text,
      percent_funded: project.css("ul.project-stats li.first.funded strong").text.gsub("%", "").to_i 
    }
  end

  projects
end
```
Note we're converting the title into a symbol using the `.to_sym` method. Remember that symbols make better hash keys than strings.


