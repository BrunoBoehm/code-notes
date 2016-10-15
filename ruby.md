# Procedural Ruby

## Ruby operators
```ruby
#combined operators
x = 10
x += 5 # Assigns a value of 15 to variable x (the same as x = x + 5)

y = 20
y -= 10  # Assigns a value of 10 to variable y (the same as y = y - 10)

x = 10;
y = 5;

x /= y; # Assigns a value of 2 to variable x (the same as x = x / y)

#parallel assignment
a, b, c = 10, 20, 30

1 == 2
1.eql? 2
10 != 3
2 < 4
5 > 4
3 <= 4
8 >= 4
9 <=> 9
```

## Variable assignment

```ruby
first_number = 7
second_number = 14
 
sum = first_number + second_number
 
puts sum
```

## String interpolation
Interpolation will only work on Strings wrapped in double quotes "". Single quotes: '' do not support string interpolation
```ruby
current_president = "Barack Obama"
puts "In 2016, the president was #{current_president}."
```

We can also use this way
```ruby
answer = "Flamboyance"
puts "A group of flamingos is called a " + answer + "."
```

## Defining methods
Methods define a new thing that your program can do. Variables are a mechanism to teach your Ruby program about data; methods teach your Ruby program about a new routine or behavior it can use. Variables are like nouns, methods are like verbs.

A common syntax convention for Ruby methods is to preface them with a `#`. For example, if a method is named 'greeting', rubyists will often refer to it as #greeting. But remember that when you write it in your code, it should be greeting and not #greeting.

```ruby
def greeting # Method Signature
  puts "Hello World" # Method Body
end # Method Closing

greeting
``
Make a new file called greeting.rb with this code in it. Save your file and run it with `$ ruby greeting.rb` 

### Method arguments
Methods can also accept arguments, so that we can call `greeting(Marie)

```ruby
    #methodname #argument
def greeting(name)
  puts "Hello, #{name}!"
end

def greeting_programmer(name, language)
  puts "Hello, #{name}. We heard you are a great #{language} programmer."
end
```

The bareword, in this case name, that we use as the argument's name in the method signature becomes a local variable within the method. With the code above, when we say: greeting("Sophie"), the value of the argument name is "Sophie". During the particular runtime invoked by greeting("Sophie"), any reference to name will have the value of "Sophie", allowing the method to behave as intended.

*In the above examples, we're calling methods with parentheses, e.g., greeting('Sophie'). But you can also omit the parentheses: greeting 'Sophie'. When a method takes an argument, omitting the parentheses is generally considered bad style, as it's a bit more difficult to understand what's going on. However, when you want to call a method without any arguments like `say_hi` omitting the parentheses helps to clear things up.*

For example these are the same things
```ruby
describe "MyRubyThing" do
  it "runs" do
    # test here
  end
end

describe("MyRubyThing") do
  it("runs") do
    # test here
  end
end
```

### Default argument
By defining our method with default arguments, we make it possible to call the method with optional arguments, i.e. with or without arguments.

```ruby
#             assigning a default value
def greeting(name = "Ruby programmer")
  puts "Hello, #{name}"
end

# we now can run in the irb console
greeting
# > Hello, Ruby programmer!
 
greeting("Sophie")
# > Hello, Sophie!
```

## Return values
Return values are how different parts of your program communicate with one another. The return value of one method might be operated on by a subsequent method.

By default, Ruby doesn't display any output. The methods puts and print are a great way to explicitly tell the program to display specific information.

The `puts` (short for "out*put s*tring") and `print` commands are both used to display in the console the results of evaluating Ruby code. The primary difference between them is that `puts` adds a new line after executing, and `print` does not.

```ruby
3.times { print "Hello!" }
# > Hello!Hello!Hello!
 
3.times { puts "Hello!" }
# > Hello!
# > Hello!
# > Hello!
```
Ruby uses the `$stdout` global variable to communicate with your computers standard output stream. `puts` and `print` actually use the $stdout variable to send the information to the output stream on your computer which communicates with your operating system and outputs that information to the console.

A return value is the data returned to the program by the execution of a method, the assignment of a variable, actually... Everything in Ruby has a return value!


```ruby
def restaurant
  restaurant_name = "Guy's American Kitchen & Bar"
  cuisine = "american"
  motto = "Welcome to Flavor Town!"
end
# The return value of the restaurant method is "Welcome to Flavor Town!" because that was the last statement evaluated.

def print_name
  puts "Guy Fieri"
end
# puts returns nil

def return_name
  "Guy Fieri"
end
# this would return "Guy Fieri"
```
*The return value of a method in Ruby is generally the value of the last line of the method.*
There is one other way to return a value from a method and that is to use the `return` keyword.

## Debugging with Pry
You've already been introduced to REPLs through using IRB (Interactive Ruby). REPL stands for Read, Evaluate, Print, Loop. It is an interactive programming environment that takes a user's input, evaluates it and returns the result to the user.
Pry is another Ruby REPL with some added functionality. Once you install the Pry library you can use the following line `binding.pry` anywhere in your code.

*Binding is a built-in ruby class whose objects can encapsulate the context of your current scope (variables, methods etc.), and retain them for use outside of that context.*

Calling binding.pry is essentially 'prying' into the current binding or context of the code, from outside your file. So when you place the line binding.pry in your code, that line will get interpreted at runtime (as your program is executed). When the interpreter hits that line, your program will actually freeze and your terminal will turn into a REPL that exists right in the middle of your program, wherever you added the binding.pry line.



## Method scope
Methods in ruby create their own scope. "Scope" refers to the areas of your program in which certain data is available to you. Any local variable created outside of a method will be unavailable inside of a method. In addition, local variables created inside of a method (i.e. in between the `def` and `end` keywords of a method) 'fall out of scope' once you're outside the method.

A variable defined inside a method can't leave that method. It is not available to your program outside of the method. A variable defined outside of a method can only be made available to the code inside the method if you pass that variable in to the method as an argument.

```ruby
evil_monster = "Bowser"
 
def princess_peaches_castle
  puts "#{evil_monster} is trying to kidnap Princess Peach!"
end


princess_peaches_castle
# > NameError: undefined local variable or method `evil_monster' for main:Object
```
The variable defined outside is not accessible

We need to pass the outside variable as an argument
```ruby
evil_monster = "Bowser"
 
def princess_peaches_castle(evil_monster)
  puts "#{evil_monster} is trying to kidnap Princess Peach!"
end
 

princess_peaches_castle(evil_monster)
# > "Bowser is trying to kidnap Princess Peach!"
```

Variables defined inside of variables are not available to the outside
```ruby
def practicing_method_scope
  im_trapped_in_the_method = "You can't access me outside this method!"
end

#and now calling it outside of the method
im_trapped_in_the_method
#=> NameError: undefined local variable or method `im_trapped_in_the_method' for main:Object
```

## Conditionals
### IF...ELSE...END
```ruby
name = gets.chomp
if name == "Danny"
  puts "Welcome Danny"
elsif name == "Ian"
  puts "Yeah"
else
  puts "coucou"
end
```
Elsif enables to add more conditions.

### Ternary operator
In the above statement, the code before the `?` ("question mark") is evaluated as a boolean expression. If it returns true, the code on the left side of the `:` ("colon") will run, otherwise the code on the right will run: `conditional ? action_if_true : action_if_false`
```ruby
age = 1
 
age < 2 ? "baby" : "not a baby"
```

### statement modifiers
Ruby has a useful feature called a statement modifier that allows you to put a conditional at the end of a statement. We can also use `unless` in a statement modifier as well.
```ruby
this_year = Time.now.year
puts "Hey, it's 2015!" if this_year == "2015"
puts "Hey, it's not 2015!" unless this_year == "2015"
```

### Case statements
Useful to refactor when multiple `elsif`

```ruby
#this code smells!
name = "Alice"
 
if name == "Alice"
  puts "Hello, Alice!"
elsif name == "The White Rabbit"
  puts "Don't be late, White Rabbit"
elsif name == "The Mad Hatter"
  puts "Welcome to the tea party, Mad Hatter"
elsif name == "The Queen of Hearts"
  puts "Please don't chop off my head!"
else
  puts "Whoooo are you?"
end 
# note we can finish with an elsif, without else


#let's refactor
case name 
  when "Alice"
    puts "Hello, Alice!"
  when "The White Rabbit"
    puts "Don't be late, White Rabbit"
  when "The Mad Hatter"
    puts "Welcome to the tea party, Mad Hatter"
  when "The Queen of Hearts"
    puts "Please don't chop off my head!"
  else 
    puts "Whoooo are you?"
end
```
Under the hood, case statements actually evaluate their when conditionals by implicitly using the "case equality operator"; the case equality operator is otherwise represented by `===` ("threequals") sign.

```ruby
grade = 95
 
case grade
  when 90..100 then "A" 
  when 80..90 then "B"
  when 70..80 then "C"
  when 60..70 then "D"
  when 0..60 then "F"
end
```

### Loops & iteration

```ruby
#the easiest loop is
loop do
  puts "I have found the Time Machine!"
end

# we can do only a certain number of times
10.times do 
    puts "Hi! Welcome to my very repetitive program"
end
```

To exit the loop we can use the `break`keyword
```ruby
loop do
  puts "You'll see this exactly once."
  break # Exit the Loop
end
 
puts "And the Loop is Done"
```
Or we can use a counter 

```ruby
counter = 0 # Start our counter at 0, we have never run the loop
loop do # Start our loop
  # increment our counter by 1 and set it equal to the sum of its current value, plus 1. 
  counter = counter + 1  #also written as counter += 1
 
  # Do Something
  puts "Iteration #{counter} of the loop"
 
  if counter >= 10 # If our counter is 10 or more
    break # Stop the loop
  end
end
```

### While and Until
The `while` construct is a little different from the loop construct that we looked at earlier. The `while` construct will keep executing a block as long as a specific condition is `true`. Don't forget the counter or you'll end up with an infinite loop.
```ruby
counter = 0
while counter < 20
  puts "The current number is less than 20."
  counter += 1
end
```

You can have fun whith hot dogs
```ruby
num_of_hotdogs_eaten = 0
# => 0 (return value)
 
while num_of_hotdogs_eaten < 7
  num_of_hotdogs_eaten += 1
  puts "You have now eaten #{num_of_hotdogs_eaten} hot dog(s)."
end
# => nil (return value)
 
puts "You ate a total of #{num_of_hotdogs_eaten} hot dogs!"
# => nil (return value)
 
# > "You have now eaten 1 hot dog(s)."
# > "You have now eaten 2 hot dog(s)."
# > "You have now eaten 3 hot dog(s)."
# > "You have now eaten 4 hot dog(s)."
# > "You have now eaten 5 hot dog(s)."
# > "You have now eaten 6 hot dog(s)."
# > "You have now eaten 7 hot dog(s)."
 
# > "You ate a total of 7 hot dogs!"

# or without any counter
input = ""
while input != "Mommmm!!"
  puts "Stop hitting yourself!"
  input = gets.chomp
end
```
`until` is simply the inverse of a `while` loop. An until keyword will keep executing a block until a specific condition is `true`. The block of code following until will execute while the condition is `false.

```ruby
counter = 0
until counter == 20 #it will break out when counter == 20 becomes true
#instead of: while counter < 20
  puts "The current number is less than 20."
  counter += 1
end
```
Note that the return value of a `while loop is always nil. If we want our method to return something else, we have to tell it to do so.

### Each
The most important thing to remember about `each` is that it does not change the return value. It implicitly returns the original array.
```ruby
basket = ["apple 1","apple 2","apple 3","apple 4","apple 5","apple 6","apple 7","apple 8","apple 9","apple 10"]
 
# Step 1,2,3,4,5
basket.each do |apple|
    puts "Taking out #{apple}"
end
```
The `each_with_index` method can also be used
```ruby
hash = Hash.new
%w(cat dog wombat).each_with_index { |item, index|
  hash[item] = index
}
hash   #=> {"cat"=>0, "dog"=>1, "wombat"=>2}
```

## Arrays
An array is like a basket––it is a container for a collection of data.
An array is like a _list_ but in code form. Arrays can contain any data types in any combination––strings, integers, other arrays, hashes, etc.
All about the [arrays](https://docs.ruby-lang.org/en/2.0.0/Array.html)

```ruby
# you can replace
student1 = "Harry Potter"
student2 = "Ron Weasley"
student3 = "Hermione Granger"
student4 = "Draco Malfoy"

# by using 
students = ["Harry Potter", "Ron Weasley", "Hermione Granger", "Draco Malfoy"]
```
### Creating an Array
Literal constructor
```ruby
my_array = []
puppies = ["bulldog", "terrier", "poodle"]
words = %w[rats live on no evil star]
```
Class Constructor
```ruby
my_array = Array.new
Array.new(3)       #=> [nil, nil, nil]
Array.new(3, true) #=> [true, true, true]

Array.new(4) { Hash.new } #=> [{}, {}, {}, {}]

# quick way to build up multi-dimensional arrays
empty_table = Array.new(3) { Array.new(3) }
#=> [[nil, nil, nil], [nil, nil, nil], [nil, nil, nil]]
```

### Adding to an array
Shovel method
```ruby
famous_cats = ["lil' bub", "grumpy cat", "Maru"]
 
famous_cats << "nala cat"
 
puts famous_cats.inspect
# > ["lil' bub", "grumpy cat", "Maru", "nala cat"]
```
Note: The `.inspect` method returns a string containing a human-readable representation of an object. In this case, the list of the strings held in the array.

The `.push` (inverse of .pop) method to append at the end
```ruby
 famous_cats.push("nala cat")
 
puts famous_cats.inspect
# > ["lil' bub", "grumpy cat", "Maru", "nala cat"]
```

The unshift (inverse of shift) method to add to the front
```ruby
 famous_cats.unshift("nala cat")
 
puts famous_cats.inspect
# > ["nala cat", "lil' bub", "grumpy cat", "Maru"]
```
You can also add at a specific position
```ruby
arr.insert(3, 'apple')  #=> [0, 1, 2, 'apple', 3, 4, 5, 6]

#and add multiple values at once
arr.insert(3, 'orange', 'pear', 'grapefruit')
#=> [0, 1, 2, "orange", "pear", "grapefruit", "apple", 3, 4, 5, 6]
```

### Removing to an Array
The `.pop` method (inverse of .push) removes the last item and returns the removed element.
```ruby
maru_cat = famous_cats.pop
 
puts famous_cats.inspect 
# > ["lil' bub", "grumpy cat"]
puts maru_cat
# > Maru
```

Calling `.shift` on an array will remove the first item from the front of the array. The .shift method will also supply the removed element as a return
```ruby
famous_cats = ["lil' bub", "grumpy cat", "Maru"]
lil_bub = famous_cats.shift
 
puts famous_cats.inspect
# > ["grumpy cat", "Maru"]
puts lil_bub
# > lil' bub
```

To delete an element at a particular index in a destructive way:
```ruby
arr =  [1, 2, 3, 4, 5, 6]
arr.delete_at(2) #=> 4
arr #=> [2, 3, 5]
```

To delete a particular element anywhere in an array, use delete:
```ruby
arr = [1, 2, 2, 3]
arr.delete(2) #=> [1, 3]
``

### Accessing the elements of an array
Index starts at 0, so just use `famous_cats[2]`, or using the negative index `famous_cats[-1]` (last element is -1 because before 0 which is the beginning)

If you type an index that doesn't exist it will return `nil`

```ruby
arr = [1, 2, 3, 4, 5, 6]
arr[2, 3] #=> [3, 4, 5]
arr[1..4] #=> [2, 3, 4, 5]
arr.at(0) #=> 1
```

To get the first n elements of an array use `.take(n)`
```ruby
arr.take(3) #=> [1, 2, 3]

# and to take all elements after n
arr.drop(3) #=> [4, 5, 6]
```

You can also use a few helper methods
* famous_cats.first
* famous_cats.last

You can also get the *first* index number of an element by using `.index()`
```ruby
famous_cats.index("Maru")
#  └── nil
```

### Informations about an array
```ruby
browsers = ['Chrome', 'Firefox', 'Safari', 'Opera', 'IE']
browsers.length #=> 5
browsers.count #=> 5
browsers.empty? #=> false
browsers.include?('Konqueror') #=> false
```
### Working with arrays
For strings, this means alphabetically, for numerical values, this means from smallest number to highest number. The​ .sort ​method works by implicitly comparing elements with the "spaceship" operator​ <=> ​(because it looks like a flying saucer, swoosh!) and moving them accordingly.​

```ruby
famous_cats = ["lil' bub", "grumpy cat", "maru"]
famous_cats.sort
#=> ["grumpy cat", "lil' bub", "maru"]
```

Because sort returns a new array, we generally store it into another variable. So we would do sorted_cats = famous_cats.sort. If you don't care about the unsorted version of the array you can call `sort!` (destructive). This will sort the existing array without requiring you to save the return into a new variable. It's a ruby convention that a method with the ! will do the operation in place. It will modify the receiver of the method (AKA the thing to the left of the dot).

To reverse an array
```ruby
famous_wizards = ["Dumbledore", "Gandalf", "Merlin"]
famous_wizards.reverse
#=> ["Merlin", "Gandalf", "Dumbledore"]
```

Note: Be aware that all of the methods we have covered in this reading are case sensitive. For example, reverse not Reverse.

### Converting data types
The `.split` method will convert a string into an array.
```ruby
"hippo,giraffe,monkey,horse".split(",")
  => ["hippo", "giraffe", "monkey", "horse"]
```

To convert a range to an array we use the `.to_a`
```ruby
(1..10).to_a
  => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
The `.join` method, when called on a array, will convert it into a string.
```ruby
["a", "b", "c"].join
  => "abc"

["a", "b", "c"].join(" :-) ")
  => "a :-) b :-) c"  
```

### Collect and Map
Remember the `.each` method does not change the return value. It implicitly returns the original array. 

```ruby
toppings = ["pickles", "mushrooms", "bacon"]
 
def hamburger(toppings)
  toppings.each do |topping|
    puts "I love #{topping} on my burgers!"
  end
end

#I love pickles on my burgers!
#I love mushrooms on my burgers!
#I love bacon on my burgers!
#=> ["pickles", "mushrooms", "bacon"]
```
Since `map` and `collect` are the same thing, this can be expressed exactly the same way with collect, like the following.
```ruby
toppings = ["pickles", "mushrooms", "bacon"]
 
def hamburger(toppings)
  toppings.map do |topping|
    puts "I love #{topping} on my burgers!"
  end
end

# or the same thing with collect instead
def hamburger(toppings)
  toppings.collect do |topping|
    puts "I love #{topping} on my burgers!"
  end
end

# > I love pickles on my burgers!
# > I love mushrooms on my burgers!
# > I love bacon on my burgers!
# => [nil, nil, nil]
```

Note: we are using puts, which always has a nil return value, so if we remove it it will return what we want.

```ruby
def burger(toppings)
  toppings.collect do |topping|
    "I love #{topping} on my burgers"
  end
end

# => ["I love pickles on my burgers", "I love mushrooms on my burgers", "I love bacon on my burgers"]
```

Another example of `.collect` is 
```ruby
def reverse_each_word(sentence)
  sentence.split.collect {|word| word.reverse}.join(" ")
end
```

To understand under the hood how the `.collect` method works:
```ruby
def hello(array)
  i = 0
  collection = []
  while i < array.length
    collection << yield(array[i])
    i += 1
  end
  collection
end

hello(["Tim", "Tom", "Jim"]) { |name| "Hi, #{name}" }

# => ["Hi, Tim", "Hi, Tom", "Hi, Jim"]
```

### Boolean enumerators
`.all?` enumerator: the block passed to it must return `true` for every iteration for the entire `#all`? expression or method to return `true`.

```ruby
all_odd = [1,3].all? do |number|
  number.odd? # Will evaluate to true for 1, true for 3
end 
#=> true

all_odd 
#=> true
```

`.none?` is the opposite, where we are interested in none of the elements in a collection producing a true expression within the block passed to `#none?`.

```ruby
[1,3].none?{|i| i.even?} #=> true
```

Use `.any?` to ensure that at least one element in a collection will create a `true` expression within the block passed.

```ruby
[1,2,100].any?{|i| i > 99} #=> true
```

`#include?` will return true if the given object exists in the element. If it doesn't find a match, it will return `false`.

```ruby
the_numbers = [4,8,15,16,23,42]
the_numbers.include?(42)   #=> true
the_numbers.include?(6)   #=> false
```

### Select, detect, reject
When you evoke `#select on a collection, the return value will be a new array containing all the elements of the collection that cause the block passed to #select to return `true`.

```ruby
[1,2,3,4,5].select do |number|
  number.even?
end 
#=> [2,4]

[1,2,3].select{|i| i.is_a?(String)} 
#=> []
```

`#detect` will only return the first element that makes the block true. Note that `.detect` and `.find` are two names for the same method.

```ruby
[1,2,3].detect{|i| i.odd?} 
#=> 1

[1,2,3,4].detect{|i| i.is_a?(String)} 
#=> nil
```

`#reject` will return an array with the elements for which the block is false.

```ruby
[1,2].reject{|i| i.even?} 
#=> [1]
```

## Blocks
Blocks are part of what make the Ruby language special, powerful, and loved.

```ruby
primary_colors = ["Red", "Yellow", "Blue"]
primary_colors.each do |color| # do begins a block
  # the lines between the do/end are the block's body
  puts "Primary Color #{color} is #{color.length} letters long."
end # end terminates the block
``
The iterator `#each` yields each element one at a time to every iteration via a variable declared with the opening of the block.

A block is a chunk of code between braces, { } or between do/end keywords that you can pass to a method almost exactly like you can pass an argument to a method. 

When invoking an iterator like #each or #map, the variable name inside the pipes acts as an argument that is being passed into the block. 
The iterator will pass, or yield, each element of the collection on which it is called to the block. 
Each element, as it gets passed into the block, will be equal to the variable name inside the pipes.

Under the hood, these methods rely on the `yield` keyword. When used inside the body of a method, it will allow you to call that method with a block and pass, or "yield", to that block. It says "stop executing the code in this method and instead execute the code in the block. Then, return to the code in the method."

```ruby
def yielding
  puts "the program is executing the code inside the method"
  yield
  puts "now we are back in the method"
end

# To call this method with a block, we use the following syntax
yielding {puts "the method has yielded to the block!"}

# > "the program is executing the code inside the method"
# > "the method has yielded to the block!"
# > "now we are back in the method"
```
You can yield values/parameters to a block (they will get in the |i| "pipe" placeholder of the block).
```ruby
def yielding_with_arguments(num)
  puts "the program is executing the code inside the method"
  yield(num)
  puts "now we are back in the method"
end

# for instance, with argument 2 and a block, where |i| is the placeholder for the yielded value
yielding_with_arguments(2) {|i| puts i * 2}

# => "the program is executing the code inside the method"
# => 4
# => "now we are back in the method"
```

You can use `.block_given?` to test whether a block is passed
```ruby
def hello_t(array)
  if block_given?
    i = 0
    while i < array.length
      yield(array[i])
      i = i + 1
    end
    array
  else
    puts "Hey! No block was given!"
  end
end

# we are using a while loop to iterate over an array and yielding each member of the array in turn to a block.

hello_t(["Tim", "Tom", "Jim"]) do |name|
    if name.start_with?("T")
        puts "Hi, #{name}"
    end
end

# > Hi, Tim
# > Hi, Tom
# => ["Tim", "Tom", "Jim"]
```
More about [blocks, proces, lambdas](http://www.reactive.io/tips/2008/12/21/understanding-ruby-blocks-procs-and-lambdas)




## Helpers

```ruby
"Hello World".upcase
#=> "HELLO WORLD"
"Hello World".reverse
#=> "dlroW olleH" 
"Hello World".downcase
#=> "hello world" 
"Hello World".capitalize
#=> "Hello world" 
"Hello World".swapcase
#=> "hELLO wORLD" 
name.start_with?("T")
```

To pause a program for some time you can use [sleep](http://stackoverflow.com/questions/1329967/tell-ruby-program-to-wait-some-amount-of-time)

```ruby
sleep 3
# sleep(num_secs)
sleep(4.minutes)
# or, even longer...
sleep(2.hours); sleep(3.days) # etc., etc.
#Or shorter
sleep(0.5) #Half a second
```

