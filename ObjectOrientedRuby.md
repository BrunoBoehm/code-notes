# Object Oriented Ruby
It's natural to wonder, "how can a string of ones and zeroes be referred to as an 'object'?". A code object representing a water pipe (instead of a smoking pipe) might contain values for `length`, `diameter`, `material`, and `manufacturer`. The bundling of these individual pieces of information together begins to form a larger whole, it also bundles customized functions that can be performed on that data (methods).

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
We're used to defining methods already with the def keyword. If we place this method definition within the body of a class, that method becomes a specific behavior of instances of that class, not a generic procedure we can just call whenever we want.

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

The `bro_greeting` variable is what's known as a **local variable**, so named because it can only be accessed in a specific, local environment.
A local variable that is defined inside one method, for example, cannot be accessed by another method. In order to get around this limitation, we can use **instance variables** inside our Ruby classes.

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
  end
 
  def name
    @this_dogs_name
  end
end
 
lassie = Dog.new
lassie.name = "Lassie"
 
puts lassie.name
```

### Object attributes
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

We can initialize several attributes
```ruby
def initialize(first_name, last_name)
    @first_name = first_name
    @last_name = last_name
  end
```

### Object accessors
In Ruby a macro is like a method, just some code, that instead of returning a Ruby datatype returns more Ruby code! The implementation of macros is considered metaprogramming––the writing of programs that operate on other programs. We'll implement macros to abstract away the manual, explicit definition of setter and getter methods in a Ruby class.

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

We can even make it shorter
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



