# Object Oriented Ruby
It's natural to wonder, "how can a string of ones and zeroes be referred to as an 'object'?". A code object representing a water pipe (instead of a smoking pipe) might contain values for `length`, `diameter`, `material`, and `manufacturer`. The bundling of these individual pieces of information together begins to form a larger whole, it also bundles customized functions that can be performed on that data (methods).

Object oriented programming, or OOP, is an extremely useful programming paradigm in which we can organize our code according to how real-world objects might interact with one another. We can wrap properties/data and behavior up in classes, and then create instances, or individual "members", of those classes that can interact with one another.

Imagine a phone call between 2 people. Sure, the PEOPLE are real, but what about the phone call? If we think about the phone call through OOP, we can model it too! A phone call has a caller and a receiver, a duration, and even a cost_per_minute. In the real world, it's not a real thing, but in OOP IT IS!

## Classes and instances
Let's say we are building a dog walking app. Our app's users might be dog walkers and dog owners and they can use the app to manage the dog walks. Such an app would need to store information about a potentially large number of dogs.

### Class instances
Think of a class like the blueprint that defines how to build an object. The Dog class is different from an individual dog just as the blueprints that show how to build a house are not the actual house.

A Ruby class both contains the instructions for creating new objects and has the ability to create those objects. Calling `.new` on the Dog class is like getting a brand new `dog` object from an assembly line which produces a series of similar dog objects based on the same `Dog` template.

Class names begin with capital letters because they are stored in Ruby constants. If your class name contains two words, the name should be CamelCased

```ruby
class Dog
  # some code to describe a dog
end

fido = Dog.new
fido #=> #<Dog:0x007fc52c2d7d20>
fido.object_id #=> 70173135795280
fido.methods
```
On the Dog class, we call the .new method and that will *instantiate* a new dog. Classes act as a factory for our objects, capable of instantiating new instances. Instantiate means bringing a new object to life, a new individual, like a particular dog, like Snoopy or Lassie or Rover. An `instance` is a single occurrence of an object. Instances refer to the individual objects produced from the class.

Notice that every time you make an instance of a class, Ruby tells you that the return value is something that looks like `#<Dog:0x007fc52c2cc588>`. This syntax is called Ruby Object Notation and it's just the default way that Ruby communicates to you that you are dealing with an object or instance of a particular class.

We send the fido instance a message object_id by separating the receiving object, fido and the message, object_id by a dot (.). When we send an object a message through `dot notation`, we are evoking the corresponding method on the object. We are `calling` the object_id method on fido.

### Instance Methods
We're used to defining methods already with the def keyword. If we place this method definition within the body of a class, that method becomes a specific behavior of instances of that class, not a generic procedure we can just call whenever we want. It represents an objectfs ability to have logic.

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
We call the methods defined within the object's class Instance Methods because they are methods that belong to any instance of the class.

### Instance variables
When we build objects through our own classes we know we can add behavior to the objects through instance methods. But how do we give our objects data?

```ruby
bro_greeting = "Sup, bro?"
```

The `bro_greeting` variable is what's known as a **local variable**, so named because it can only be accessed in a specific, local environment: they are scoped to the method.
A local variable that is defined inside one method, for example, cannot be accessed by another method. In order to get around this limitation, we can use **instance variables** inside our Ruby classes. Instance variable belongs to the instance itself / the scope is to the instance itself.

Note: $variable are global variables, but you shouldn't have to use them, they are for the interpreter itself.

*An instance variable is a variable that is accessible in any instance method in a particular instance of a class.*

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
Calling `lassie = Dog.new` and `lassie.name = "Lassie"` with `lassie.name` would not work and return `name: undefined local variable or method this_dogs_name``.
That is because this_dogs_name is a local variable. A local variable has a local scope. That means that it cannot be accessed outside of the method in which it is defined.

We define an instance variable by prefacing the variable name with an @ symbol.

Instance variables are bound to an instance of a class. Instance variables hold information about an instance, usually an attribute of that instance. Think of instance variables as the containers for instance-specific information. 

Instance variables can be called on throughout the class, without needing to be passed into other methods as arguments (as would be the case with local variables).

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
Let's define a setter method. A setter method is defined with an `=`, equals sign, appended to the name of the method. The = is followed by the `(argument_name)`.

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

Our Person class' `#name` method is referred to as a "getter" or reader method. It returns information stored in an instance variable. In order to make a person's name attribute writable, we need to define a "setter" method.

We can initialize several attributes. Initializing is making sure the object can do something at the moment it is instantiated. `#initialize` is a hook / a callback / a lifecycle event, because it is a method automatically fired by another method (everytime you call `#new` on the Baby class).
```ruby
def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end
```

### Object accessors
In Ruby a macro is like a method, just some code, that instead of returning a Ruby datatype returns more Ruby code! The implementation of macros is considered metaprogrammingb: the writing of programs that operate on other programs. We'll implement macros to abstract away the manual, explicit definition of setter and getter methods in a Ruby class.

In object-oriented Ruby, there is a strong convention to have a setter and a getter method that pertain to the same attribute. For example, a .name getter and a .name= setter on our Person class. Because this pattern is so common, we find ourself making these manual setter and getter definitions again and again.

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
  attre_reader :dna #should be initialized since cannot be written
end
```

## Object Lifecycle
We already know that any Ruby class can produce new instances of itself, via the `<Class Name>.new` method, whether or not that class has an `#initialize` method. However, if we want each instance of our class to be created with certain attributes, we must define an #initialize method. An #initialize method is a method that is called automatically whenever `#new` is used.

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

Now we can call `.new` like this
```ruby
lassie = Dog.new("Collie")
 
lassie.breed #=> "Collie"
```

### Class constants
First, whenever we see ::, it indicates "name spacing" (eg `Book::GENRES`). In other words, it tells us that the thing on the right side of the colons is a thing defined within the context, or name space, of the thing on the left side of the colons. In this case, this is telling us that there is a constant, GENRES (we know it's a constant because it's written in all caps) that is defined within our Book class: it's a Class Constant.

Class Constants are available to all instances of a particular class. Whereas instance variables, i.e. title, author, etc., are individual to each instance of a class, class constants are shared among all instances. They all have access to the same data, and if that data should change for some reason, all instances will know about that change.

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
A domain model is a representation of real-world concepts in software. The concept of domain modeling is key in object orientation. In object orientation, we think of our classes as templates for objects. The instances of our classes are thought of as objects. For example, a Person class produces people objects that have attributes and behaviors, as described and enacted by instance methods.

An `AutoPlant` class that produces individual cars and has instance and class methods that handle things like take_inventory or paint_cars. Such a program not only represents a single object or concept but an entire environment––that of the auto factory. Consequently, this program could be understood as a domain model.

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
When we create a class, each new instance of a class is considered to be an object. Every object is aware of itself and we can define methods in which we tell objects to operate on themselves. We do so using the `self` keyword, inside the body of an instance method, to refer to the very same object the method is being called on. Every object is, quite literally, self aware.

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

The beauty of object-oriented programming is that we can encapsulate, or wrap up, attributes and behaviors into one object.

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

## Class variables and Methods
The `Album` class can have its own variables and methods. We call these class variables and class methods. Let's say you wanted to keep a counter for how many albums you had in your music collection. The current code in our Album class has no way to keep such a count. Looks like we will have to write some code to accommodate this new feature of our program.

When it comes to adding new features or functionalities to our code, we start out by asking a question: whose responsibility is it to enact this behavior or functionality? It isn't the job of the individual albums, but the job of the `Album` class to keep a count of all of instances it produces.

A class variable looks like this: `@@variable_name`.

A class variable is accessible to the entire class––it has class scope. A class method is a method that is called on the class itself, not on the instances of that class. 

Class variables store information regarding the class as a whole and class methods enact behaviors that belong to the whole class, not just to individual instances of that class.

```ruby
class Album
  @@album_count = 0 
 
  def initialize
    @@album_count += 1
  end
 
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

Here, the `self` keyword refers to the entire class itself, not to an instance of the class. In this case, we are inside the class only, not inside an instance method of that class. So, we are in the class scope, not the instance scope.

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

Private methods also signal to other developers that this method is depended on by other methods in your program. It signals that they should beware of removing such a method for fear of breaking other parts of the program that they may not realize rely on it.

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
### Remembering Objects
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
`self.all` defines a class method that simply exposes the value in the class variable `@@all`. This is a class reader, very similar to an instance reader method that reads out of an instance-level property such as `@name`.

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
  #self within a class method is the class itself.
  
end
 
Person.new("Ada Lovelace")
Person.all.detect{|p| p.name == "Ada Lovelace"} # Literal implementation, no abstraction or encapsulation
 
Person.find_by_name("Ada Lovelace") # Abstract implementation with logic entirely encapsulated.
```

We call class methods like `Person.find_by_name` 'finders'. Finder class methods are responsible for finding instances based on properties or conditions.

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

In an ideal world every time we got CSV data we'd just want the Person class to be responsible for parsing it. Could we build something like `Person.new_from_csv`? Of course!

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

We don't want to put that functionality in the `initialize` method. Initialize should be closed to modification. It should only handle the most required and common cases of initializing an object. Anything we add to initialize should be permanent and never modified. If we need more functionality when making an instance, instead of modifying initialize, we can extend it by wrapping it within a custom constructor.

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



