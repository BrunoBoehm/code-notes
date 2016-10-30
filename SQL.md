# SQL

SQL (Structured Query Language) is a language for managing data in a database: it's only used for one thing: talking to databases. 
Even though SQL has just one purpose, it is used by many different database systems such as MySQL, PostgreSQL, or the system we'll be using in this course: SQLite.

Every database system has its own strengths and weaknesses, and as you learn more about them you should evaluate them thoughtfully when deciding which to use for what purpose.

Most of the sites you interact with every day, and consequently most of the sites you'll work on or build, need databases to persist data. We need a way to take our Ruby objects, store them in a database and retrieve them at the appropriate time.

## SQLite3
Test that you have it installed
```
which sqlite3

> /usr/bin/sqlite3
```

### Creating a database
Now create a DB called test_sqlite.db and open it in the sqlite prompt.
```
sqlite3 test_sqlite.db

>
SQLite version 3.7.12 2013-03-19 12:42:02
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite>

# you can now type
sqlite> create table test_table(id);
sqlite> .quit
```

All SQL statements that you write in your terminal, inside the sqlite prompt, `sqlite3>`, must be terminated with a semi-colon `;`. If you hit enter without adding a semi-colon to the end of your line, you will be trapped! Don't worry though, just add that ; on the new line and hit enter again.

### SQL databases basics
Relational Databases like SQLite store data in a structure we refer to as a table. You can think of a table in a database a lot like you would a spreadsheet.

For instance, if you had a table called "People" you could imagine a structure like this
person_name | age | email_address
--- | --- | ---
bob | 29 | bob@gmail.com
avi | 28 | avi@flatiron.com
bobba | 39 | bobba@gmail.com

2 conventions/ best practices
- we will always use lowercase letters when referring to columns in our database
- when we have multiple words in a column name, we link them together using underscores rather than spaces ("snake_case")

Once you create a database (which we did with the `sqlite3 database_name.db` command), we create a table:
```sql
sqlite3> CREATE TABLE table_name;
Error: near ";": syntax error
```

We get the error because before we're able to store any actual data in a table, we'll need to define the columns in the table as well as the specific type of data each column will store.

```sql
sqlite3> CREATE TABLE cats (
				id INTEGER PRIMARY KEY,
				name TEXT,
				age INTEGER
			;)
```
Capitalization is arbitrary, but it is a convention to help separate the SQL commands from the names we make up for our tables and columns.

Every table we create, regardless of the other column names and data types, should be defined with an id `INTEGER PRIMARY KEY` column + data type. Our SQLite database tables must be indexed by a number. We want each row in our table to have a number, which we'll call "id", just like in an Excel spreadsheet. 

Numbering our table rows makes our data that much easier to access, update, and organize. SQLite comes with a data type designation called "Primary Key". Primary keys are unique and auto-incrementing, meaning they start at 1 and each new row automatically gets assigned the next numeric value.

You need help, type in `sqlite3> .help`
```sql
sqlite> .help
.backup ?DB? FILE      Backup DB (default "main") to FILE
.bail ON|OFF           Stop after hitting an error.  Default OFF
.databases             List names and files of attached databases
.dump ?TABLE? ...      Dump the database in an SQL text format
                         If TABLE specified, only dump tables matching
                         LIKE pattern TABLE.
.echo ON|OFF           Turn command echo on or off
.exit                  Exit this program
.explain ?ON|OFF?      Turn output mode suitable for EXPLAIN on or off.
                         With no args, it turns EXPLAIN on.
.header(s) ON|OFF      Turn display of headers on or off
.help                  Show this message
.import FILE TABLE     Import data from FILE into TABLE
.indices ?TABLE?       Show names of all indices
                         If TABLE specified, only show indices for tables
                         matching LIKE pattern TABLE.
.log FILE|off          Turn logging on or off.  FILE can be stderr/stdout
.mode MODE ?TABLE?     Set output mode where MODE is one of:
                         csv      Comma-separated values
                         column   Left-aligned columns.  (See .width)
                         html     HTML <table> code
                         insert   SQL insert statements for TABLE
                         line     One value per line
                         list     Values delimited by .separator TEXT
                         tabs     Tab-separated values
                         tcl      TCL list elements
.nullvalue TEXT      Print TEXT in place of NULL values
.output FILENAME       Send output to FILENAME
.output stdout         Send output to the screen
.prompt MAIN CONTINUE  Replace the standard prompts
.quit                  Exit this program
.read FILENAME         Execute SQL in FILENAME
.restore ?DB? FILE     Restore content of DB (default "main") from FILE
.schema ?TABLE?        Show the CREATE statements
                         If TABLE specified, only show tables matching
                         LIKE pattern TABLE.
.separator TEXT      Change separator used by output mode and .import
.show                  Show the current values for various settings
.stats ON|OFF          Turn stats on or off
.tables ?TABLE?        List names of tables
                         If TABLE specified, only list tables matching
                         LIKE pattern TABLE.
.timeout MS            Try opening locked tables for MS milliseconds
.vfsname ?AUX?         Print the name of the VFS stack
.width NUM1 NUM2 ...   Set column widths for "column" mode
.timer ON|OFF          Turn the CPU timer measurement on or off
```

To list all tables in the database
```
sqlite> .tables
cats
```
We can look at the structure or "schema" of our database with the `.schema` command
```
sqlite> .schema
CREATE TABLE cats(
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER
);
```
### Altering the database
We decide we want to add or remove a column. We can do so with the `ALTER TABLE` statement.

```
sqlite> ALTER TABLE cats ADD COLUMN breed TEXT;
```

Let's check out the `.schema`
```sql
sqlite> .schema
CREATE TABLE cats(
  id INTEGER PRIMARY KEY,
  name TEXT,
  age INTEGER,
  breed TEXT
);
```

Notice that the ALTER statement isn't here, but instead SQLite has updated our original CREATE statement. The schema reflects the current structure of the database, which is reflected as the CREATE statement necessary to create that structure.

### Delete a table
Deleting a table is very simple `sqlite> DROP TABLE cats;`
And that's it! You can exit out of the sqlite prompt with the .quit command.

### Writing in text editor
Up until now, we've been executing our SQL commands directly in the terminal. It is likely, however, that you will find yourself writing SQL in a file and executing that file in the context of your database: we'll create files in our text editor that have the .sql extension. These files will contain valid SQL code. Then, we can execute these files against our database in the command line. 

SQL is a programming language like any other, so we can write SQL in our text editor and execute it. This allows us to keep better track of our SQL code, including the SQL statements that create tables and query data from those tables.

We create a database and quit it
```
sqlite3 pet_database.db
...
sqlite> .quit
```
In the text editor we create a file called `01_create_cats_table.sql` with the following
```sql
CREATE TABLE cats (
  id INTEGER PRIMARY KEY,
    name TEXT, 
    age INTEGER
);
```

And not in the command line run it
```
sqlite3 pets_database.db < 01_create_cats_table.sql
```
Let's now see if it worked
```
sqlite3 pets_database.db
sqlite> .schema

CREATE TABLE cats (
  id INTEGER PRIMARY KEY,
    name TEXT, 
    age INTEGER
);

sqlite> .quit
```

To do any other operation on the database we'll create another `.sql` file in the text editor and execute them, for instance `02_add_column_to_cats.sql`

### Data types
We define columns in a CREATE statement by including a name and a datatype to let SQLite know the kind of data we will be storing there. The practice of explicitly declaring a type is known as "typing".

Why is it important that we use typing in our database? Simply put, typing allows us to exercise some level of control over our data. Typing not only informs our database of the kind of data we plan to store in a column but it also restricts it. In other words, because databases are designed to store large amounts of data, they are very concerned with storing, accessing, and acting upon that data as efficiently and normally as possible.

Typing gives us the ability to perform all kinds of operations with predictable results. For instance, the ability to perform Math operations like SUM - i.e. summing integers - doesn't just depend on everything being an integer of some sort but would also expect it.

SQLite is a good starting point to learn about datatypes because it only has four basic categories of datatypes; they are:
```
TEXT
INTEGER
REAL
BLOB
```

TEXT: Any alphanumeric characters which we want to represent as plain text. The body of this paragraph is text. Your name is text. Your email address is a piece of text.

INTEGER: Anything we want to represent as a whole number. If it's a number and contains no letter or special characters or decimal points then we should store it as an integer. If we use it to perform math or create a comparison between two different rows in our database, then we definitely want to store it as an integer. If it's just a number, it's generally not a bad idea to store it as an integer.

REAL: Anything that's a plain old decimal like 1.3 or 2.25. SQLite will store decimals up to 15 characters long.

BLOB: It is generally used for holding binary data

To increase its compatibility with other database engines (E.G. mySQL or PostgreSQL), SQLite allows the programmer to use other common datatypes outside of the four mentioned above. This is why we are referring to TEXT INTEGER REAL BLOB as datatype "categories". All other common datatypes are lumped into one of the four existing datatypes recognized by SQLite.

### SQL Inserting, Updating, and Selecting
As a wrap up
```
INSERT INTO cats (name, age, breed) VALUES ('Maru', 3, 'Scottish Fold');

SELECT id, name, age, breed FROM cats;

SELECT * FROM cats WHERE age < 2;

SELECT * FROM cats ORDER BY age DESC;

SELECT * FROM cats ORDER BY age DESC LIMIT 1;

SELECT name FROM cats WHERE age BETWEEN 1 AND 3;

SELECT * FROM cats WHERE name IS NULL;

SELECT COUNT(owner_id) FROM cats WHERE owner_id = 1;

SELECT breed, owner_id, COUNT(breed) FROM cats GROUP BY breed, owner_id;

UPDATE cats SET name = "Hana" WHERE name = "Hannah";

DELETE FROM cats WHERE id = 3;
```

#### INSERTING
Let's start inserting some cats into the pets_database.db cats table, we can place this in a new file `01_insert_cats_into_cats_table.sql`
```sql
INSERT INTO cats (name, age, breed) VALUES ('Maru', 3, 'Scottish Fold');
```
Note that we didn't specify the "id" column name or value. Since we created the cats table with an "id" column whose type is `INTEGER PRIMARY KEY`, we don't have to specify the id column values when we insert data. Primary Key columns are auto-incrementing.

```bash
sqlite3 pets_database.db < 01_insert_cats_into_cats_table.sql
```

#### SELECTING
We also want to read the data with the `SELECT` statement. We specify the names of the columns we want to SELECT and then tell SQL the table we want to select them FROM.

If we want to get the names of all the dogs and cats, we can no longer run a query with just the column name: `SELECT name FROM cats,dogs;` would return an error.
```
SELECT [names of columns we want to select] FROM [table we're reading from];
SELECT name FROM cats;
SELECT cats.name FROM cats;
SELECT cats.name, dogs.name FROM cats, dogs;

SELECT name, age FROM cats;

SELECT id, name, age, breed FROM cats;
```
Which should give back
```
1 | Maru     | 3 | Scottish Fold
2 | Hannah   | 1 | Tabby
3 | Lil\' Bub | 5 | American Shorthair
```
A faster way to get data from every column in our table is to use a special selector, known commonly as the 'wildcard', `*` selector.
```
SELECT * FROM cats;
``` 
If you only want to select unique values, you can use the DISTINCT keyword.
```
SELECT DISTINCT name FROM cats;
```
We can format the output
```
.header on       # output the name of each column
.mode column     # now we are in column mode, enabling us to run the next two .width commands
.width auto      # adjusts and normalizes column width
# or
.width NUM1, NUM2 # customize column width
```

#### WHERE (select on condition)
What happens if we want to retrieve all the baby cats who are younger than two years old? When a user signs into your app, you'll need to access your database and select the user that matches the credentials an individual is using to log in.

Here's an example of a boilerplate `SELECT` statement using a `WHERE` clause.
```
SELECT * FROM [table name] WHERE [column name] = [some value];
SELECT * FROM cats WHERE age < 2;

sqlite> SELECT * FROM cats WHERE name = "Maru";
```
will return
```
Maru|3|Scottish Fold
```

#### UPDATE
The `UPDATE` statement uses a `WHERE` clause to grab the row you want to update. It identifies the table name you are looking in and resets the data in a particular column to a new value.

```
UPDATE [table name] SET [column name] = [new value] WHERE [column name] = [value];

sqlite> UPDATE cats SET name = "Hana" WHERE name = "Hannah";

UPDATE cats SET owner_id = 1 WHERE name = "Maru";
```

#### DELETING
We can select the row to delete using the Primary Key column.
```
DELETE FROM [table name] WHERE [column name] = [value];

sqlite> DELETE FROM cats WHERE id = 3;
```

### Basic SQL Queries
The term "query" refers to any SQL statement that retrieves data from your database. `SELECT`is already a query. Queries can help us manipulate, view and analyze data.

#### Order By
This modifier allows us to order the table rows returned by a certain `SELECT` statement. The default is to order in ascending order. If we want to specify though, we can use `ASC` for "ascending" or `DESC` for "descending."

```
SELECT column_name FROM table_name ORDER BY column_name ASC|DESC;

sqlite> SELECT * FROM cats ORDER BY age;

sqlite> SELECT * FROM cats ORDER BY age DESC;
```

#### Limit
`LIMIT` is used to determine the number of records you want to return from a dataset.

```
SELECT * FROM cats ORDER BY age DESC LIMIT 1;
```

#### Between
Let's say we urgently need to select all of the cats whose age is between 1 and 3. To create such a query, we can use `BETWEEN`.

```
SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2;

SELECT name FROM cats WHERE age BETWEEN 1 AND 3;
```

#### Null
We can add data with missing values using the `NULL` keyword.
```
INSERT INTO cats (name, age, breed) VALUES (NULL, NULL, "Tabby");

SELECT * FROM cats WHERE name IS NULL;
```

#### Count
SQL aggregate functions are SQL statements that retrieve minimum and maximum values from a column, sum values in a column, get the average of a column's values, or count a number of records that meet certain conditions. `COUNT` will count the number of records that meet certain condition.
```
SELECT COUNT([column name]) FROM [table name] WHERE [column name] = [value]

SELECT COUNT(owner_id) FROM cats WHERE owner_id = 1;
```

#### Group by
GROUP BY is a great function for aggregating results into different
segments — you can even use it on multiple columns!

```
SELECT breed, COUNT(breed) FROM cats GROUP BY breed;

SELECT breed, owner_id, COUNT(breed) FROM cats GROUP BY breed, owner_id;
```

### SQL Aggregate functions
Imagine writing 
- an application for a restaurant owner to track her customers and transactions
- an app that an e-commerce company uses to store users, transactions and shopping behaviors. 
- a social networking application whose administrators want to keep track of the number of times a user logs on and identify who their most frequent users are. 
- a program to discover who clients biggest spenders are or what they make on average over a busy weekend. 
- a feature to know who their most frequent buyers are and how much they spend on average on a given item, and so on.

Storing or persisting information in an application or program is about more than just keeping track of static data, we want to operate on or analyze the data we store.

We can do so using the aggregate functions that SQL makes available to us. With these functions we can `COUNT`, `SUM` and average `AVG` column data, request `MIN` and `MAX` values, and more. SQL also makes available to use keywords that allow us to group aggregated data by various categories and narrow our search criteria based on various conditions.

```
SELECT AVG(column_name) FROM table_name;
SELECT AVG(net_worth) FROM cats;

AVG(net_worth) 
---------------
800850.0 

SELECT AVG(net_worth) AS average_net_worth FROM cats;
average_net_worth   
--------------------
800850.0 

SELECT SUM(column_name) FROM table_name;
SELECT SUM(net_worth) FROM cats;

SELECT MIN(column_name) FROM table_name;
SELECT MIN(net_worth) FROM cats;

SELECT MAX(column_name) FROM table_name;

SELECT COUNT(column_name) FROM table_name;
SELECT COUNT(name) FROM cats;
```
We can use the `COUNT()` function to calculate the total number of rows in a table that are not NULL. `NULL` means empty.
```
SELECT COUNT(*) FROM cats WHERE net_worth > 1000000;
```
`*` means everything. Sometimes it's called the "wildcard." This COUNT(*) will count the rows where at least one column has data in it.

## Relational Databases
It's hard to imagine an application that saves data but doesn't relate it. For example––a Facebook user is associated to other users via "friendships", an Amazon user has a shopping cart full of items, a blog's author has many posts and posts might in turn have many tags. All of these examples require different datasets to be related or associated to one another.

A relational database, simply put, is **a database structured to recognize relations among stored items of information**. A relational database will allow us to store representations of our Ruby objects and preserve the relationships between those objects when we store them.

In a relational database, every row has a number, called a `primary key`. Relationships between tables can be established by using a `foreign key` column, that uses that primary key of another table to refer to a member of that table. Primary keys are numbers, and as opposed to names for instance, are always unique!

Edgar Codd invented the concept of the relational database, in other words, he came up with the idea that storing data in tables, indexed by primary key and related by foreign keys would normalize that data.

Continuing the posts and authors examples, you could say that an author has many posts. The reciprocal of this would be that a post belongs to an author. Let's say the Post "10 Ways to Pet Your Cat" is written by "Joe Burgess", and Joe's id is 5. We just need to add a new column to our Posts table with the id of the Author that it's related to. Let's call this column author_id. This `author_id` column is called a "foreign key".

Why didn't we do the reverse? Why didn't we add a list of Post IDs to a single Author row? The answer is pretty straight forward. Is there an array data type? Is there really any way to store multiple items in a single column? Nope! So we just set up the relation in one direction. 

A user can write many posts, but each post was written by, and therefore belongs to, only one user. The thing that "has many" is considered to be the parent. The thing that "belongs to" we'll call the child. The child table gets the foreign key column, the value of which is the primary key of that data's/row's parent.

To associate one table to another, give one table a column called "foreign key" with a type of `INTEGER` and insert the primary key of another table row into that column.

Bonus: practice [SQL queries](https://sqlbolt.com/lesson/select_queries_review)

## SQL JOINS
A SQL JOIN clause is a way to combine rows from two or more tables, based on a common column between them. The great thing about relational databases is that they are just that––relational. 

Relational databases allow us not only to store data that is interconnected, but to retrieve that data in ways that reflect that interconnectivity.

We know how to write a SELECT statement that gets us all of the cats with a particular owner_id. For example:
```
SELECT * FROM cats WHERE owner_id = 2;
```
This would return us the appropriate list of cats. But what if we wanted to query both the Cats and the Owners tables and return information about both cats and owners? This is where JOIN statements come in.

type | declaration
------ | ------
INNER JOIN | Returns all rows when there is at least one match in BOTH tables
LEFT JOIN | Returns all rows from the left table, and the matched rows from the right table
RIGHT JOIN | Returns all rows from the right table, and the matched rows from the left table
FULL JOIN | Returns all rows when there is a match in ONE of the tables

### Inner JOIN
```
SELECT column_name(s)
FROM first_table
INNER JOIN second_table
ON first_table.column_name=second_table.column_name;
# ON ... is the JOIN condition that needs to be fulfilled

SELECT Cats.name, Cats.breed, Owners.name
FROM Cats 
INNER JOIN Owners
ON Cats.owner_id = Owners.id;

# this will output
name             breed            name
---------------  ---------------  ----------
Maru             Scottish Fold    mugumogu  
Hana             Tabby            mugumogu  
Nona             Tortoiseshell    Sophie  

# we can even improve the last column's name
SELECT Cats.name, Cats.breed, Owners.name AS "owner_name"
FROM Cats 
INNER JOIN Owners
ON Cats.owner_id = Owners.id;

# this will output
name             breed            owner_name
---------------  ---------------  ----------
Maru             Scottish Fold    mugumogu  
Hana             Tabby            mugumogu  
Nona             Tortoiseshell    Sophie  
```

### Left Outer JOIN
```
# boilerplate
SELECT column_name(s)
FROM first_table
LEFT JOIN second_table
ON first_table.column_name=second_table.column_name;

# example 
SELECT Cats.name, Cats.breed, Owners.name 
FROM Cats 
LEFT OUTER JOIN Owners 
ON Cats.owner_id = Owners.id;

# would return...
name             breed            name      
---------------  ---------------  ----------
Maru             Scottish Fold    mugumogu  
Hana             Tabby            mugumogu  
Nona             Tortoiseshell    Sophie 
Lil' Bub         perma-kitten                
```

### Complex/Outer JOINS
Inner join example
>Imagine you want to get a list of all the students with an "A" in the class. We only want those students in the class with top grades, ignoring the other students in the class.

Outer/complex join example
>Imagine you want to get a list of all the students with an "A" in the class. We only want those students in the class with top grades, ignoring the other students in the class.

It is referred to as "complex" simply because SQL is conducting an inner join in addition to gathering a little more information from one or more tables.



## ORMs and Active Record
Object Relational Mapping (ORM) is the technique of accessing a relational database using an object-oriented programming language.

Object Relational Mapping is a way for our Ruby programs to manage database data by "mapping" database tables to classes and instances of classes to rows in those tables.

```ruby
database_connection = SQLite3::Database.new('db/my_database.db')
database_connection.execute("Some SQL statement")
```

An ORM is really just a concept. It is a design pattern, a conventional way for us to organize our programs when we want those programs to connect to a database. The convention is this:
**When "mapping" our program to a database, we equate classes with database tables and instances of those classes with table rows.**

You may also see this referred to as "wrapping" a database, because we are writing Ruby code that "wraps" or handles SQL.

Let's say we have a program that helps a veterinary office keep track of the pets it treats and those pets' owners. Such a program would have an Owners class and Cats class.
```ruby
database_connection = SQLite3::Database.new('db/pets.db')
database_connection.execute("CREATE TABLE IF NOT EXISTS cats(
    id INTEGER PRIMARY KEY,
    name TEXT,
    breed TEXT,
    age INTEGER)
    ")
database_connection.execute("CREATE TABLE IF NOT EXISTS owners(
    id INTEGER PRIMARY KEY, 
    name TEXT)
    ")

database_connection.execute("INSERT INTO cats (name, breed, age) 
    VALUES ('Maru', 'scottish fold', 3)
    ")
database_connection.execute("INSERT INTO cats (name, breed, age) 
    VALUES ('Hana', 'tortoiseshell', 1)
    ")    
```
Notice that in the lines of code above, there is a lot of repetition. In fact, the only difference between the two lines in which we insert data into the database are the actual values. The repetition would also occur for other SQL statements we might want to execute against our database. Any `SELECT` queries, for example, would repeat the call to the `database_connection.execute` method and differ only in the specifics of what data we are selecting from which table.

Instead of repeating the same, or similar, code any time we want to perform common actions against our database, we can write a series of methods to abstract that behavior. We can write a `.save` method on our `Cats` class that handles the common action of `INSERT`ing data into the database.

```ruby
class Cat
 
  @@all = []
 
  def initialize(name, breed, age)
    @name = name
    @breed = breed
    @age = age
    @@all << self
  end
 
  def self.all
    @@all
  end
 
  def self.save(name, breed, age, database_connection)
    database_connection.execute(
        "INSERT INTO cats (name, breed, age) VALUES (?, ?, ?)", name, breed, age
        )
  end
end

# Now we can create some cats and save them
database_connection = SQLite3::Database.new('db/pets.db')

Cat.new("Maru", "scottish fold", 3)
Cat.new("Hana", "tortoiseshell", 1)

Cat.all.each do |cat|
    Cat.save(cat.name, cat.breed, cat.age, database_connection)
end
```
We follow the convention: classes are mapped to or equated with tables and instances of a class are equated to table rows. If we have a `Cat` class, we have a cats **table**. `Cat` instances get stored as **rows** in the cats table.

Just like the .save method we previewed above, we will learn to build a series of common, conventional methods that our programs can rely on again and again to communicate with our database.

### Mapping Ruby Classes to Database Tables
Let's say we are building a music player app that allows users to store their music and browse their songs by song.

```ruby
class Song
  attr_accessor :name, :album
 
  def initialize(name, album)
    @name = name
    @album = album
  end
end
```

 In order to "map" this Song class to a songs database table, we need to create our database, then we need to create our songs table. 
 
In building an ORM, **it is conventional to pluralize the name of the class to create the name of the table**. Therefore, the `Song` class equals the `songs` table.
 
**CREATING THE DB**
Before we can create a songs table we need to create our music database. This is not the responsibility of the `Song` class since it will be mapped to a *table* inside of the database, but not to the database as a whole. It is the responsibility of our program as a whole to create and establish the database.

We will do our setup in the `config` directory that contains the `environment.rb` file.
```ruby
# config/environment.rb
require 'sqlite3'
require_relative '../lib/song.rb'
 
DB = {:conn => SQLite3::Database.new("db/music.db")} 
```
We set up a constant: `DB`, that is equal to a hash that contains our connection to the database. In our lib/song.rb file, we can therefore access the DB constant and the database connection it holds like this `DB[:conn]`.

**CREATING THE TABLE**
According to the ORM convention in which a class is mapped to or equated with a database table, we need to create a songs table. We will accomplish this by writing a class method in our `Song` class that creates this table.

To "map" our class to a database table, we will create a table with the **same name as our class** and give that **table column names that match the `attr_accessor`s of our class**.
 
```ruby
class Song
  attr_accessor :name, :album, :id
 
  def initialize(name, album, id=nil)
    @id = id
    @name = name
    @album = album
  end
 
  def self.create_table
    sql =  <<-SQL 
      CREATE TABLE IF NOT EXISTS songs (
        id INTEGER PRIMARY KEY, 
        name TEXT, 
        album TEXT
        )
        SQL
    DB[:conn].execute(sql) 
  end
end
```
We therefore set the default value of the id argument that the `#initialize` method takes equal to nil, so that we can create new song instances that do not have an id value. We'll leave that up to the database to handle later on. Only the database itself, through the magic of SQL, can ensure that the id of each record is unique.

Why is the `.create_table` method a class method? Well, it is not the responsibility of an individual song to create the table it will eventually be saved into. It is the job of the class as a whole to create the table that it is mapped to.
Top-Tip: For strings that will take up multiple lines in your text editor, use a heredoc to create a string that runs on to multiple lines. To create a heredoc, we use: `<<- + name of language contained in our multiline statement + the string, on multiple lines + name of language.`

**MAPPING CLASS INSTANCES TO TABLE ROWS**
If individual instances of a class are "mapped" to rows in a table, does that mean that the instances themselves, these individual Ruby objects, are saved into the database? Actually, **we are not saving Ruby objects in our database. We are going to take the individual attributes of a given instance**, in this case a song's name and album, and save those attributes that describe an individual song to the database as one, single row.

```ruby
hello = Song.new("Hello", "25")
 
hello.name 
# => "Hello"
 
hello.album
# => "25"

INSERT INTO songs (name, album) VALUES ("Hello", "25")
```
We can see that the operation of saving the attributes of a particular song into a database table is common enough. Let's abstract this functionality into an instance method, `#save`.

```ruby
class Song
...
  def save
    sql = <<-SQL
      INSERT INTO songs (name, album) 
      VALUES (?, ?)
    SQL
 
    DB[:conn].execute(sql, self.name, self.album)
  end
end
```
Above, we used the heredoc to craft our multi-line SQL statement. How are we going to pass in, or interpolate, the name and album of a given song into our heredoc? We use something called bound parameters. 

Instead of interpolating variables into a string of SQL, we are using the ? characters as placeholders. Then, the special magic provided to us by the SQLite3-Ruby gem's `#execute` method will take the values we pass in as an argument and apply them as the values of the question marks.

So, our #save method inserts a record into our database that has the name and album values of the song instance we are trying to save. We are not saving the Ruby object itself. We are creating a new row in our songs table that has the values that characterize that song instance. Notice that **we didn't insert an ID number into the table** with the above statement. Remember that the `INTEGER PRIMARY KEY` datatype will assign and auto-increment the id attribute of each record that gets saved.

**CREATING INSTANCES VS TABLE ROWS**
The moment in which we create a new Song instance with the #new method is different than the moment in which we save a representation of that song to our database.

The #new method creates a new instance of the song class, a new Ruby object. The #save method takes the attributes that characterize a given song and saves them in a new row of the songs table in our database.

At what point in time should we actually save a new record?
We don't want to force our objects to be saved every time they are created, or make the creation of an object dependent upon/always coupled with saving a record to the database. So, we'll keep our #initialize and #save methods separate.
```ruby
Song.create_table
hello = Song.new("Hello", "25")
ninety_nine_problems = Song.new("99 Problems", "The Black Album")
 
hello.save
ninety_nine_problems.save
```

**PRIMARY KEY ID**
The database table's row has a column for Name, Album and also ID. As each record gets inserted into the database, it is given an ID number automatically.
In this way, our `hello` instance is stored in the database with the name and album that we gave it, plus an ID number that the database assigns to it.

We want our `hello` instance to completely reflect the database row it is associated with so that we can retrieve it from the table later on with ease. So, once the new row with hello's data is inserted into the table, let's grab the ID of that newly inserted row and assign it to be the value of hello's id attribute.

```ruby
...
  def save
    sql = <<- SQL
      INSERT INTO songs (name, album) 
      VALUES (?, ?)
    SQL
 
    DB[:conn].execute(sql, self.name, self.album)
 
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
  end
```
This approach still leaves a little to be desired, however. Here, we have to first create the new song and then save it, every time we want to create and save a song. This is repetitive and tedious. Any time we see the same code being used again and again, we think about abstracting that code into a method...

**THE CREATE METHOD**
This method will wrap the code we used above to create a new Song instance and save it. We use `keyword arguments` to pass a name and album into our #create method. 
```ruby
class Song
...
  def self.create(name:, album:)
    song = Song.new(name, album)
    song.save
    song
  end
end
```
Notice that at the end of the method, we are returning the song instance that we instantiated. The return value of #create should always be the object that we created.
```ruby
song = Song.create(name: "Hello", album: "25")
# => #<Song:0x007f94f2c28ee8 @id=1, @name="Hello", @album="25">
 
song.name
# => "Hello"
 
song.album
# => "25"
```


