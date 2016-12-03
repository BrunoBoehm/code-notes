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

#### Some examples of queries
Taken from this [video from Learn](https://www.youtube.com/watch?v=qfB1MRnzk4g) here are a few queries,

Here is the database schema that we can run from the command line: `sqlite3 ecommerce-database.db < schema.sql` (it will create the file and execute the `sql` statement in the context of the database).
```sql
CREATE TABLE products (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    price INTEGER
);

CREATE TABLE customers (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT
);

CREATE TABLE carts (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    customer_id INTEGER
);

CREATE TABLE line_items (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    cart_id INTEGER,
    product-id INTEGER
);
```

Here's some seed data we can insert `sqlite3 ecommerce-database.db < seed.sql`
```sql
INSERT INTO customers (name) VALUES
("Avi"), ("Steven"), ("Jill");

INSERT INTO carts (customer_id) VALUES
(1), 
(2), 
(1);

INSERT INTO products (name, price) VALUES
("Phone"), 
("Tv"), 
("Radio");

INSERT INTO line_items (cart_id, product_id) VALUES
(1, 1), 
(1, 2), 
(2, 2);
(3, 3);
```

To get the products from a cart (let's say `cart_id = 1`)
```sql
SELECT products.*   # selects all colums
FROM products
JOIN line_items ON line_items.product_id = products.id
WHERE line_items.cart_id = 1
```

To get the total price of a cart and number of articles in each cart
```sql
SELECT cart_id, SUM(products.price) AS total_price, COUNT(*) AS product_count 

FROM products
JOIN line_items ON products.id = line_items.product_id

GROUP BY line_items.cart_id
```

To get the same as above, as well as the name of the person who owns each cart (2 other tables to join to reach the info)
```sql
SELECT customers.name, cart_id, SUM(products.price) AS total_price, COUNT(*) AS product_count

FROM products
JOIN line_items ON products.id = line_items.product_id
INNER JOIN carts ON line_items.cart_id = carts.id
INNER JOIN customers ON carts.customer_id = customers.id

GROUP BY line_items.cart_id
```

To get the most valuable customer
```sql
SELECT customers.name, SUM(products.price) AS total_price, COUNT(*) AS product_count 
FROM products
JOIN line_items ON line_items.product_id = products.id
INNER JOIN carts ON line_items.cart_id = carts.id
INNER JOIN customers ON carts.customer_id = customers.id
GROUP BY carts.customer_id
ORDER BY total_price DESC
LIMIT 1
```
You actually `SELECT` from the "combined" table that's created by `FROM ... JOIN ... INNER JOIN ... INNER JOIN` (will append each database), and then `GROUP` and `ORDER` and `LIMIT` to get only one row of the original "combined" table constructed by the query.

To get the most popular product:
```sql
SELECT products.name, COUNT(*) AS product_count

FROM products
JOIN line_items ON products.id = line_items.product_id
INNER JOIN carts ON line_items.cart_id = carts.id
INNER JOIN customers ON carts.customer_id = customers.id

GROUP BY line_items.product_id
ORDER BY product_count DESC
LIMIT 1
```

Two very useful articles about the JOIN SQL queries
- [visual explanatio of JOIN SQL query](http://blog.flatironschool.com/sql-joins-explained-visually-the-3-ring-binder/)
- [another visual explanation attempt](https://blog.codinghorror.com/a-visual-explanation-of-sql-joins/)

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

Note: when we execute the `SELECT last_insert_rowid() FROM songs` statement using our SQLite3-Ruby gem, we'll get back an array of arrays, something like:
```
DB[:conn].execute("SELECT last_insert_rowid() FROM songs")
# => [[1]]
```
Here, we used the `last_insert_rowid()` SQL query to request one thing: the last inserted row's ID. Our SQLite3-Ruby gem obliged and gave us an array that contains one array that contains one element––the last inserted row ID.

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

**READING FROM THE DATABASE**
Our Ruby program gets most interesting when we add data. To do this, we use a database. When we want our Ruby program to store things we send them off to a database. When we want to retrieve those things, we ask the database to send them back to our program.

Ruby understands objects. The database understands raw data. We don't store Ruby objects in the database, and we don't get Ruby objects back from the database. When we query the database, it is up to us to write the code that takes that data and turns it back into an instance of whatever class.

`.new_from_db`: The first thing we need to do is convert what the database gives us into a Ruby object. We will use this method to create all the Ruby objects in our next two methods.

The first thing to know is that the database, SQLite in our case, will return an array of data for each row. For example, a row for Michael Jackson's "Thriller" (356 seconds long) that has a db id of 1 would look like this: `[1, "Thriller", 356]`.
```ruby
def self.new_from_db(row)
    new_song = self.new
    new_song.id = row[0]
    new_song.name = row[1]
    new_song.length = row[2]
    new_song
end
```
This function will for a given row return a newly created instance.
We can even refactor this using `.tap`
```ruby
def self.new_from_db(row)
    self.new.tap do |song|
        song.id, song.name, song.length = row[0], row[1], row[2]
    end
end
```

`Song.all`: Now we can start writing our methods to retrieve the data. Let's get all the songs in the database. The `SELECT * FROM songs` will return an array of rows from the database that match our query. Then we have to iterate over each row and use the `new_from_db` method to create a new Ruby object for each row.
```ruby
class Song
    def self.all
        sql = <<-SQL
            SELECT *
            FROM songs
        SQL
        DB[:conn].execute(sql).map do |row|
            self.new_from_db(row)
        end
    end    
end
```

Last method we need is `Song.find_by_name(name)`. This one is similar to Song.all with the small exception being that we have to include a name in our SQL statement. 
```ruby
class Song
    def self.find_by_name(name)
        sql = <<-
            SELECT *
            FROM songs
            WHERE name = ?
            LIMIT 1
        SQL
        
        DB[:conn].execute(sql, name).map do |row|
            self.new_from_db(row)
        end.first
    end
end
```
That we can also refactor like
```ruby
class Song
    def self.find_by_name(name)
        sql = <<-
            SELECT *
            FROM songs
            WHERE name = ?
            LIMIT 1
        SQL
        
        stud = DB[:conn].execute(sql, name).flatten
        self.new.tap do |song|
            sond.id, song.name, song.length = stud[0], stud[1], stud[2]
        end
    end
end
```
Because the `DB[:conn].execute(sql, name)` would return an array like [[id, name, length]], we need to flatten it.

**UPDATING DATABASE RECORDS**
It's hard to imagine a database that would stay totally static and never change. A user of your social networking site "friends" another user, creating a new association between them. We need to be able to update, or change, the records that are stored in that database.

First, we need to find the appropriate record. 
Then, we make some changes to it.
Finally, we need to save it once again.

In our Ruby ORM, where attributes of given Ruby objects are stored as an individual row in a database table, we will need to retrieve these attributes, reconstitute them into a Ruby object, make changes to that object using Ruby methods, and then save those (newly updated) attributes back into the database.

Here's the code for the `Song` class
```ruby
class Song
 
attr_accessor :name, :album
attr_reader :id
 
  def initialize(id=nil, name, album)
    @id = id
    @name = name
    @album = album
  end
 
  def save
    sql = <<-SQL
      INSERT INTO songs (name, album) 
      VALUES (?, ?)
    SQL
 
    DB[:conn].execute(sql, self.name, self.album)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
  end
 
  def self.create(name:, album:)
    song = Song.new(name, album)
    song.save
    song
  end
 
  def self.find_by_name(name)
    sql = "SELECT * FROM songs WHERE name = ?"
    result = DB[:conn].execute(sql, name)[0]
    Song.new(result[0], result[1], result[2])
  end
end
```
Now that we've seen how to create a Song instance, save its attributes to the database, retrieve those attributes and use them to re-create a Song instance, let's move on to updating records and objects.

To `update` songs, we need to find the song, make the change we need, then save the record back to the DB, using the `UPDATE` SQL statement.
```ruby
class Song
  ...
  def update
    sql = "UPDATE songs SET name = ?, album = ? WHERE id = ?"
    DB[:conn].execute(sql, self.name, self.album, self.id)
  end
end
```
We need to use the `WHERE` call on something immutable: the `id`. Indeed if we looked for the record using for instance the song's name or album, they could have just been updated in the object, and we wouldn't be able to find them back in the database.

Let's work on refactoring our `.save` method. This method will always INSERT a new row into the database table. But, what happens if we accidentally call `#save` on an object that has already been persisted and has an analogous database row? It would have the effect of creating a new database `row` with the same attributes as an existing row. The only difference would be the `id` number
```ruby
hello = Song.new("Hello", "25")
hello.save
 
DB[:conn].execute("SELECT * FROM songs WHERE name = 'Hello' AND album = '25'")
# => [[1, "Hello", "25"]]
 
# What happens if we save the same song again?
 
hello.save
 
DB[:conn].execute("SELECT * FROM songs WHERE name = 'Hello' AND album = '25'")
# => [[1, "Hello", "25"], [2, "Hello", "25"]]
```

We need our `#save` method to check to see if the object it is being called on has already been persisted. If so, don't `INSERT` a new row into the database, simply `UPDATE` an existing one.

How do we know if an object has been persisted? If it has an `id` that is not `nil`. Remember that an object's id attribute gets set only once it has been `INSERT`ed into the database.
```ruby
def save
  if self.id
    # looks better than !self.id.nil? ;)
    self.update
  else
    sql = <<-SQL
      INSERT INTO songs (name, album) 
      VALUES (?, ?)
    SQL
    DB[:conn].execute(sql, self.name, self.album)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM songs")[0][0]
  end 
end
```

### Preventing duplication
What happens when two Ruby objects get created using the same attributes? For example, lets say we have a `Song` class that produces individual song objects, each of which have a name and album attribute.

Nothing stops us from creating two objects, each of which have the exact same name and album. We'll assume our connection to the database is stored in DB[:conn].
```
hello = Song.new("Hello", "25")
hello_again = Song.new("Hello", "25")

hello.save
hello_again.save
 
DB[:conn].execute("SELECT * FROM songs WHERE name = "Hello")
# => [[1, "Hello", "25"], [2, "Hello", "25"]]
```

We have two records that contain the same information! We should first check to see the object we are trying to save already has an equivalent record in the database, if it does, we should simply update it, otherwise, we can go ahead and save it. 

Before we call `#save` on our hello object, we need to check and see if a record containing this name and album already exists in the database.
```
SELECT * FROM songs
WHERE name = "Hello", album = "25";
```

We need to craft a `#find_or_create_by` method.
```ruby
  def self.find_or_create_by(name:, album:)
    song = DB[:conn].execute("SELECT * FROM songs WHERE name = ? AND album = ?", name, album)
    
    if !song.empty?
      song_data = song[0]
      song = Song.new(song_data[0], song_data[1], song_data[2])
      # re-instantiation of an existing Song object 
    else
      song = self.create(name: name, album: album)
      # create and save a new Song instance
    end
    song
  end 
```
Now, we can use our Song class without worrying about creating duplicate records.
```ruby
Song.find_or_create_by(name: "Hello", album: "25")
Song.find_or_create_by(name: "Hello", album: "25")
 
DB[:conn].execute("SELECT * FROM songs WHERE name = Hello, album = 25")
# => [[1, "Hello", "25"]]
```
### Dynamics ORM
As developers, we understand the need for our Ruby programs to be able to connect with a database. Any complex application is going to need to persist some data. Along with this need, we recognize the need for the connection between our program and our database to be, like all of our code, organized and sensible. 

That is why we use the ORM design pattern in which a Ruby class is mapped to a database table and instances of that class are represented as rows in that table.

We can implement this mapping by using a class to create a database table:
```ruby
class Song
  attr_accessor :name, :album
  attr_reader :id
 
  def initialize(id=nil, name, album)
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
Here, we create our `songs` table, so-named because we are mapping this table to an existing class, `Song`. The column names for the table are taken from the known attr_accessors of the Song class.

This is one way to map our program to our database, but it has some limitations. For one thing, our `#create_table` method is dependent on our knowing exactly what to name our table and columns. So, every class in our program would require us to re-write this `#create_table` method, swapping out different table and column names each time. This is repetitive.

Well, with a dynamic ORM, we can abstract all of our conventional ORM methods into just such flexible, abstract, and shareable methods.

A dynamic ORM allows us to map an existing database table to a class, and write methods that can use nothing more than information regarding a specific database table to:
- Create attr_accessors for a Ruby class.
- Create shareable methods for inserting, updating, selecting, and deleting data from the database table.

This pattern –– first creating the database table and having your program do all the work of writing your ORM methods for you, based on that table –– is exactly how we will develop web applications in Sinatra and Rails.

```ruby
require_relative "../config/environment.rb"
require 'active_support/inflector'

class Song
# get the table name    
  def self.table_name
    self.to_s.downcase.pluralize
    # pluralize comes from /inflector
  end

  def table_name_for_insert
    self.class.table_name
    # .class puts us in the class method scope
  end

# get the columns names
  def self.column_names
    DB[:conn].results_as_hash = true
    # will return a hash instead of array

    sql = "PRAGMA table_info('#{table_name}')"
    # remember: will return as a hash

    table_info = DB[:conn].execute(sql)
    column_names = []
    table_info.each do |row|
      column_names << row["name"]
      # we just need the names from the hash
    end
    column_names.compact
    # will return the array without any nil value
  end

  def col_names_for_insert
    self.class.column_names.delete_if {|col| col == "id"}.join(", ")
  end
  
# define the attr_accessor
  self.column_names.each do |col_name|
    attr_accessor col_name.to_sym
    # .to_sym converts to :symbol
  end

# metaprogram the initialize function
  def initialize(options={})
    options.each do |property, value|
      self.send("#{property}=", value)
      # metaprogramming #send method interpolates each hash key
      # as a method set to the key's value
    end
  end

# insertion to the database
  def values_for_insert
    values = []
    self.class.column_names.each do |col_name|
      values << "'#{send(col_name)}'" unless send(col_name).nil?
    end
    values.join(", ")
  end

  def save
    sql = "INSERT INTO #{table_name_for_insert} (#{col_names_for_insert}) VALUES (#{values_for_insert})"
    DB[:conn].execute(sql)
    @id = DB[:conn].execute("SELECT last_insert_rowid() FROM #{table_name_for_insert}")[0][0]
  end

# finder class method
  def self.find_by_name(name)
    sql = "SELECT * FROM #{self.table_name} WHERE name = '#{name}'"
    DB[:conn].execute(sql)
  end

end
```
Several points to note.

Querying a table for column name is made with the SQL query [PRAGMA](https://www.sqlite.org/pragma.html#pragma_table_info)
```
PRAGMA table_info(<table name>)
```

`DB[:conn].results_as_hash = true`, available to use from the SQLite3-Ruby gem, will return an array of hashes describing the table itself.
```ruby
[{"cid"=>0,
  "name"=>"id",
  "type"=>"INTEGER",
  "notnull"=>0,
  "dflt_value"=>nil,
  "pk"=>1,
  0=>0,
  1=>"id",
  2=>"INTEGER",
  3=>0,
  4=>nil,
  5=>1},
 {"cid"=>1,
  "name"=>"name",
  "type"=>"TEXT",
  "notnull"=>0,
  "dflt_value"=>nil,
  "pk"=>0,
  0=>1,
  1=>"name",
  2=>"TEXT",
  3=>0,
  4=>nil,
  5=>0},
 {"cid"=>2,
  "name"=>"album",
  "type"=>"TEXT",
  "notnull"=>0,
  "dflt_value"=>nil,
  "pk"=>0,
  0=>2,
  1=>"album",
  2=>"TEXT",
  3=>0,
  4=>nil,
  5=>0}]
```
Note that in
```ruby
self.class.column_names.each do |col_name|
  values << "'#{send(col_name)}'" unless send(col_name).nil?
end
```
We push the return value of invoking a method via the #send method, unless that value is nil (as it would be for the id method before a record is saved, for instance).
Notice that we are wrapping the return value in a string. That is because we are trying to craft a string of SQL. Also notice that each individual value will be enclosed in single quotes, ' ', inside that string. That is because the final SQL string will need to look like this:
```ruby
INSERT INTO songs (name, album)
VALUES 'Hello', '25';
```

### Dynamic ORM with inheritance
The great thing about a Dynamic ORM is that it provides us with a way to write an ORM that is almost entirely abstract. In other words, the methods we write that allow a given Ruby class and instances to communicate with a database are not specific to any one class.

This means we can use such methods again and again. We can define them in only one place and simply make them available, via inheritance, to any other class in our program.

## ActiveRecord Mechanics
Up until now, we've built our own ORM. We even worked up to creating a dynamic ORM that could give us lots of functionality via inheritance.

Meet [Active Record](http://guides.rubyonrails.org/active_record_basics.html), the database toolkit for Ruby! Active Record is like the dynamic ORM we built. It maps database tables to Ruby classes. Imagine our dynamic ORM on steroids. 

We don't have to write much code -- as long as we **follow conventions**. And as a result, Active Record gives you enormous functionality (Think of our ORM with a lot more methods).

Active Record is a Ruby gem, meaning we get an entire library of code just by running `gem install activerecord` or by including it in our `Gemfile`.

### Set up a connection
This is how we would connect to a database
```ruby
connection = ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/songs.sqlite"
)
```

### Create a table
Let's create our table in the `db/songs.sqlite`
```ruby
sql = <<-SQL
  CREATE TABLE IF NOT EXISTS songs (
  id INTEGER PRIMARY KEY, 
  title TEXT, 
  length INTEGER
  )
SQL
 
ActiveRecord::Base.connection.execute(sql)
```

### Active record methods
To add Active Record's `Base` methods to your class, inherit from `ActiveRecord::Base`
```ruby
class Student < ActiveRecord::Base
end
```
Note that our `Student` class doesn't have any methods defined for `#name` either. Nor does it make use of Ruby's built-in `attr_accessor` method.
Check out the complete list of [querying methods](http://guides.rubyonrails.org/active_record_querying.html#calculations)

Retrieve a list of all the columns in the table
```ruby
Student.column_names
#=> [:id, :name]
```

Create a new `Student` entry in the database
```ruby
Student.create(name: 'Jon')
# INSERT INTO students (name) VALUES ('Jon')
```

Retrieve a `Student` from the database by `id`
```ruby
Student.find(1)
# SELECT * FROM students WHERE (students.id = 1) LIMIT 1

Student.find([1, 10])
Student.find(1, 10)
# SELECT * FROM clients WHERE (clients.id IN (1, 10))
# will return id = 1 and id = 10
```

Take the first one
```ruby
client = Client.take
# => #<Client id: 1, first_name: "Lifo">
# SELECT * FROM clients LIMIT 1

client = Client.take(2)
# => [
#   #<Client id: 1, first_name: "Lifo">,
#   #<Client id: 220, first_name: "Sara">
# ]
# SELECT * FROM clients LIMIT 2
```

To find the first record ordered by primary key
```ruby
client = Client.first
# => #<Client id: 1, first_name: "Lifo">
# SELECT * FROM clients ORDER BY clients.id ASC LIMIT 1 

client = Client.first(3)
# => [
#   #<Client id: 1, first_name: "Lifo">,
#   #<Client id: 2, first_name: "Fifo">,
#   #<Client id: 3, first_name: "Filo">
# ]
# SELECT * FROM clients ORDER BY clients.id ASC LIMIT 3

client = Client.order(:first_name).first
# => #<Client id: 2, first_name: "Fifo">
# SELECT * FROM clients ORDER BY clients.first_name ASC LIMIT 1
```

Or to take the last element
```ruby
client = Client.last
# => #<Client id: 221, first_name: "Russel">
# SELECT * FROM clients ORDER BY clients.id DESC LIMIT 1

client = Client.last(3)
# => [
#   #<Client id: 219, first_name: "James">,
#   #<Client id: 220, first_name: "Sara">,
#   #<Client id: 221, first_name: "Russel">
# ]
# SELECT * FROM clients ORDER BY id DESC LIMIT 3

client = Client.order(:first_name).last
# => #<Client id: 220, first_name: "Sara">
# SELECT * FROM clients ORDER BY clients.first_name DESC LIMIT 1
```

Order the elements
```ruby
Client.order(:created_at)
# OR
Client.order("created_at")

Client.order(created_at: :desc)
# OR
Client.order(created_at: :asc)
# OR
Client.order("created_at DESC")
# OR
Client.order("created_at ASC")

Client.order(orders_count: :asc, created_at: :desc)
# OR
Client.order(:orders_count, created_at: :desc)
# OR
Client.order("orders_count ASC, created_at DESC")
# OR
Client.order("orders_count ASC", "created_at DESC")

Client.order("orders_count ASC").order("created_at DESC")
# SELECT * FROM clients ORDER BY orders_count ASC, created_at DESC
```

Selecting Specific fields
```ruby
Client.select("viewable_by, locked")
# SELECT viewable_by, locked FROM clients
```
Be careful because this also means you're initializing a model object with only the fields that you've selected.

To only return disting records/single record per unique value in a certain field:
```ruby
Client.select(:name).distinct
# SELECT DISTINCT name FROM clients

query = Client.select(:name).distinct
# => Returns unique names
 
query.distinct(false)
# => Returns all names, even if there are duplicates
```

To put limits and offsets
```ruby
Client.limit(5)
# SELECT * FROM clients LIMIT 5

# will return instead a maximum of 5 clients beginning with the 31st
Client.limit(5).offset(30)
# SELECT * FROM clients LIMIT 5 OFFSET 30
```

Find by any attribute, such as `name`
```ruby
Student.find_by(name: 'Jon')
# SELECT * FROM artists WHERE (name = 'Jon') LIMIT 1

Client.find_by first_name: 'Lifo'
# => #<Client id: 1, first_name: "Lifo">

# is equivalent to
Client.where(first_name: 'Lifo').take
Client.where('first_name' => 'Lifo').take
# SELECT * FROM clients WHERE (first_name = "Lifo") LIMIT 1
```

You can get or set attributes of an instance of `Student` once you've retrieved it
```ruby
student = Student.find_by(name: 'Jon')
student.name
#=> 'Jon'
 
student.name = 'Steve'
 
student.name
#=> 'Steve'
```

And then save those changes to the database
```ruby
student = Student.find_by(name: 'Jon')
student.name = 'Steve'
student.save
```

Retrieving multiple objects in a large database can be very memory-consuming
```ruby
# This is very inefficient when the users table has thousands of rows.
User.all.each do |user|
  NewsMailer.weekly(user).deliver_now
end
```
User.all.each instructs Active Record to fetch the entire table in a single pass, build a model object per row, and then keep the entire array of model objects in memory. Indeed, if we have a large number of records, the entire collection may exceed the amount of memory available.
Rails provides two methods that address this problem by dividing records into memory-friendly batches for processing. The first method, find_each, retrieves a batch of records and then yields each record to the block individually as a model. The second method, find_in_batches, retrieves a batch of records and then yields the entire batch to the block as an array of models.
The find_each and find_in_batches methods are intended for use in the batch processing of a large number of records that wouldn't fit in memory all at once. If you just need to loop over a thousand records the regular find methods are the preferred option.
```ruby
User.find_each do |user|
  NewsMailer.weekly(user).deliver_now
end

# to add conditions
User.where(weekly_subscriber: true).find_each do |user|
  NewsMailer.weekly(user).deliver_now
end

User.find_each(batch_size: 5000) do |user|
  NewsMailer.weekly(user).deliver_now
end

User.find_each(start: 2000, batch_size: 5000) do |user|
  NewsMailer.weekly(user).deliver_now
end

User.find_each(start: 2000, finish: 10000, batch_size: 5000) do |user|
  NewsMailer.weekly(user).deliver_now
end

# Give add_invoices an array of 1000 invoices at a time
Invoice.find_in_batches do |invoices|
  export.add_invoices(invoices)
end
```

With conditions
```ruby
Client.where("orders_count = ? AND locked = ?", params[:orders], false)

Client.where("orders_count = ?", params[:orders])
# is preferable to
Client.where("orders_count = #{params[:orders]}")
```
because of argument safety. Putting the variable directly into the conditions string will pass the variable to the database as-is. This means that it will be an unescaped variable directly from a user who may have malicious intent. If you do this, you put your entire database at risk because once a user finds out they can exploit your database they can do just about anything to it. Never ever put your arguments directly inside the conditions string.

Similar to the (?) replacement style of params, you can also specify keys in your conditions string along with a corresponding keys/values hash. This makes for clearer readability if you have a large number of variable conditions.
```ruby
Client.where("created_at >= :start_date AND created_at <= :end_date",
  {start_date: params[:start_date], end_date: params[:end_date]})
```

To group elements
```ruby
Order.select("date(created_at) as ordered_date, sum(price) as total_price").group("date(created_at)")

# SELECT date(created_at) as ordered_date, sum(price) as total_price
# FROM orders
# GROUP BY date(created_at)
```

To get the total of grouped items
```ruby
Order.group(:status).count
# => { 'awaiting_approval' => 7, 'paid' => 12 }
# SELECT COUNT (*) AS count_all, status AS status
# FROM "orders"
# GROUP BY status
```

### Eager Loading
Eager loading is the mechanism for loading the associated records of the objects returned by Model.find using as few queries as possible.
```ruby
clients = Client.limit(10)
 
clients.each do |client|
  puts client.address.postcode
end
```
This code looks fine at the first sight. But the problem lies within the total number of queries executed. The above code executes 1 (to find 10 clients) + 10 (one per each client to load the address) = 11 queries in total.

Active Record lets you specify in advance all the associations that are going to be loaded. This is possible by specifying the `includes` method of the `Model.find` call. With `includes`, Active Record ensures that all of the specified associations are loaded using the minimum possible number of queries.
```ruby
clients = Client.includes(:address).limit(10)
 
clients.each do |client|
  puts client.address.postcode
end

# SELECT * FROM clients LIMIT 10
# SELECT addresses.* FROM addresses
# WHERE (addresses.client_id IN (1,2,3,4,5,6,7,8,9,10))
```

Active Record lets you eager load any number of associations with a single `Model.find` call by using an array, hash, or a nested hash of array/hash with the `includes` method.
```ruby
Article.includes(:category, :comments)
```
This loads all the articles and the associated category and comments for each article.
```ruby
Category.includes(articles: [{ comments: :guest }, :tags]).find(1)
```
This will find the category with id 1 and eager load all of the associated articles, the associated articles' tags and comments, and every comment's guest association.

### Scopes
Scoping allows you to specify commonly-used queries which can be referenced as method calls on the association objects or models.
```ruby
class Article < ApplicationRecord
  scope :published, -> { where(published: true) }
end

# the same as defining a class method
class Article < ApplicationRecord
  def self.published
    where(published: true)
  end
end

Article.published 
# => [published articles]
```

Using a class method is the preferred way to accept arguments for scopes. These methods will still be accessible on the association objects:
```ruby
class Article < ApplicationRecord
  def self.created_before(time)
    where("created_at < ?", time)
  end
end

# or if you really want a scope
class Article < ApplicationRecord
  scope :created_before, ->(time) { where("created_at < ?", time) }
end

Article.created_before(Time.zone.now)
```

`pluck` can be used to query single or multiple columns from the underlying table of a model. It accepts a list of column names as argument and returns an array of values of the specified columns with the corresponding data type.
```ruby
Client.where(active: true).pluck(:id)
# SELECT id FROM clients WHERE active = 1
# => [1, 2, 3]
 
Client.distinct.pluck(:role)
# SELECT DISTINCT role FROM clients
# => ['admin', 'member', 'guest']
 
Client.pluck(:id, :name)
# SELECT clients.id, clients.name FROM clients
# => [[1, 'David'], [2, 'Jeremy'], [3, 'Jose']]
```
It enables to replace code like
```ruby
Client.select(:id).map { |c| c.id }
# or
Client.select(:id).map(&:id)
# or
Client.select(:id, :name).map { |c| [c.id, c.name] }
```
Unlike `select`, `pluck` directly converts a database result into a Ruby Array, without constructing ActiveRecord objects.

ids can be used to pluck all the IDs for the relation using the table's primary key.
```ruby
Person.ids
# SELECT id FROM people
```

If you simply want to check for the existence of the object there's a method called exists?. This method will query the database using the same query as find, but instead of returning an object or collection of objects it will return either true or false.
```ruby
Client.exists?(1)

Client.exists?(id: [1,2,3])
# or
Client.exists?(name: ['John', 'Sergei'])

#or even without argument
Client.where(first_name: 'Ryan').exists?
Client.exists?

# you can also use any? and many? to check for existence on a model or relation
# via a model
Article.any?
Article.many?
 
# via a named scope
Article.recent.any?
Article.recent.many?
 
# via a relation
Article.where(published: true).any?
Article.where(published: true).many?
 
# via an association
Article.first.categories.any?
Article.first.categories.many?
```

## Rake
Rake is a tool that is available to us in Ruby that allows us to automate certain jobs––anything from execute SQL to `puts`-ing out a friendly message to the terminal.

### Intro to Rake
Rake allows us to define something called "Rake tasks" that execute these jobs. Once we define a task, that task can be executed from the command line.

Every program has some jobs that must be executed now and then. For example, the task of creating a database table, the task of making or maintaining certain files. Rake provides us a standard, conventional way to define and execute such tasks using Ruby: it is the task management tool for Ruby.

Before Rake was invented, we would have to write scripts that accomplish these tasks in `BASH`, or we would have to make potentially confusing and arbitrary decisions about what segment of our Ruby program would be responsible for executing these tasks. Writing scripts in BASH is tough, and BASH just isn't as powerful as Ruby. On the other hand, for each developer to make his or her own decisions about where to define and execute certain common tasks related to databases or file maintenance is confusing.

In fact, the `C` community was the first to implement the pattern of writing all their recurring system maintenance tasks in a separate file. They called this file the `MakeFile` because it was generally used to gather all of the source files and make it into one compiled executable file.

Building a Rake task is easy, since Rake is already available to us as a part of Ruby. All we need to do is create a file in the top level of our directory called Rakefile. We define tasks with task + name of task as a symbol + a block that contains the code we want to execute.
```ruby
task :hello do
  puts "hello from Rake!"
end
```
Now we can type in our terminal `rake hello` and it will output in the terminal
```
hello from Rake!
```

Rake comes with a handy command, `rake -T`, that we can run in the terminal to view a list of available Rake tasks and their descriptions. In order for `rake -T` to work though, we need to give our Rake tasks descriptions.
```ruby
desc 'outputs hello to the terminal'
task :hello do 
  puts "hello from Rake!"
end
```

Now, if we run rake -T in the terminal, we should see the following:
```
rake hello       # outputs hello to the terminal
```

A namespace is really just a way to group or contain something, in this case our Rake tasks. So, we might namespace a series of greeting Rake tasks, like `hello`, above, and `hola` under the greeting heading.
```ruby
namespace :greeting do 
desc 'outputs hello to the terminal'
  task :hello do 
    puts "hello from Rake!"
  end
 
  desc 'outputs hola to the terminal'
  task :hola do 
    puts "hola de Rake!"
  end
end
```

Now to use either of our Rake tasks, we can type either
```
rake greeting:hello
hello from Rake!
 
# OR 
rake greeting:hola
hola de Rake!
```

### Common rake tasks
As we move towards developing Sinatra and Rails web applications, you'll begin to use some common Rake tasks that handle the certain database-related jobs.

#### Rake db:migrate
One common pattern you'll soon become familiar with is the pattern of writing code that creates database tables and then "migrating" that code using a rake task.

Our `Student` class currently has a `#create_table` method, so let's use that method to build out our own `migrate` Rake task. We'll namespace this task under the `db` heading. This namespace will contain a few common database-related tasks. We'll call this task migrate, because it is a convention to say we are "migrating" our database by applying SQL statements that alter that database.

```ruby
desc 'migrate changes to your database' 
namespace :db do 
  task :migrate => :environment do 
    Student.create_table
  end
end
```

`:migrate => :environment` creates a *task dependency*. Since our `Student.create_table` code would require access to the `config/environment.rb` file (which is where the student class and database are loaded), we need to give our task access to this file. We can do so by defining yet another Rake task that we can tell to run before the migrate task is run.
```ruby
# in Rakefile
 
task :environment do
  require_relative './config/environment'
end
```

Or you can use a more inclusive version without any dependency
```ruby
require_relative 'config/environment.rb'

namespace :db do

  desc "Migrate the db"
  task :migrate do
    connection_details = YAML::load(File.open('config/database.yml'))
    ActiveRecord::Base.establish_connection(connection_details)
    ActiveRecord::Migrator.migrate("db/migrate/")
  end

  desc "drop and recreate the db"
  task :reset => [:drop, :migrate]

  desc "drop the db"
  task :drop do
    connection_details = YAML::load(File.open('config/database.yml'))
    File.delete(connection_details.fetch('database')) if File.exist?(connection_details.fetch('database'))
  end
end
```
Note that the yaml file in `config/database.yml` should be required in the `config/enviroment.rb` file with `require 'yaml'`, and the file just contains
```ruby
adapter: 'sqlite3'
database: 'db/tvshows.db'
```

#### Rake db:seed
Another task you will become familiar with is the `seed` task. This task is responsible for "seeding" our database with some dummy data.
```ruby
desc 'seed the database with some dummy data'
namespace :db do
  ...
  task :seed do 
    require_relative './db/seeds.rb'
  end
end
```
The conventional way to seed your database is to have a file in the `db` directory, `db/seeds.rb`, that contains some code to create instances of your class.

```ruby
# in db/seeds.rb

require_relative "../lib/student.rb"
 
Student.create(name: "Melissa", grade: "10th")
Student.create(name: "April", grade: "10th")
Student.create(name: "Luke", grade: "9th")
Student.create(name: "Devon", grade: "11th")
Student.create(name: "Sarah", grade: "10th")
```
Now, if we run `rake db:seed` in our terminal (provided we have already run `rake db:migrate` to create the database table), we will insert five records into the database.

#### Rake console
We'll define a task that starts up the `Pry` console. We'll make this task dependent on our `environment` task so that the Student class and the database connection load first.
```ruby
desc 'drop into the Pry console'
task :console => :environment do
  Pry.start 
end
```
We can now drop into our console typing the terminal command `rake console` which would bring up the following in the terminal
```
[1] pry(main)>
```

### Mechanics of migrations
Migrations are a convenient way for you to alter your database in a structured and organized manner.

You could edit fragments of SQL by hand but you would then be responsible for telling other developers that they need to go and run them. You’d also have to keep track of which changes need to be run against the production machines next time you deploy.

Migrations also allow you to describe these transformations using Ruby. The great thing about this is that it is database independent, you don't have to worry about a specific syntax for talking with the DB. For example, you could use SQLite3 during development, but Postgres in production.

Another way to think of migrations is like version control for your database. By adding a new migration for each change you make to the database, you won't lose any data you don't want to, and you can easily revert changes.

Executed migrations are tracked by ActiveRecord in your database, so they aren't used twice. Using the migrations system to apply the schema changes is easier than keeping track of the changes manually and executing them manually at the appropriate time.

#### ActiveRecord migration methods: up, down, change
Here we're creating a class called CreateArtists which inherits from ActiveRecord's `ActiveRecord::Migration` module.

Within the class we have an up method to define what code to execute when the migration is run, and in the down method we define what code to execute when the migration is rolled back. Think of it like "do" and "undo."

```ruby
# db/migrate/01_create_artists.rb
 
class CreateArtists < ActiveRecord::Migration
  def up
  end
 
  def down
  end
end
```

Another method is available to use besides up and down: change, which is more common for basic migrations. Which is just short for "do this", and then "undo it" on rollback.
```ruby
# db/migrate/01_create_artists.rb
 
class CreateArtists < ActiveRecord::Migration
  def change
  end
end
```

##### Creating a table
Using SQL with ActiveRecord this is how we'd create a table
```ruby
# in config/environment.rb we connect to a database
connection = ActiveRecord::Base.establish_connection(
  :adapter => "sqlite3",
  :database => "db/songs.sqlite"
)

# in artist.rb we create our table using SQL
sql = <<-SQL
  CREATE TABLE IF NOT EXISTS songs (
  id INTEGER PRIMARY KEY,
  title TEXT,
  length INTEGER
  )
SQL

ActiveRecord::Base.connection.execute(sql)
```

Now that we have access to `ActiveRecord::Migration`, we can create tables using only Ruby. 

To create the migration file we can use the following command
```ruby
rake db:create_migration NAME=create_artists
```

Here we've added the `create_table` method, and passed the name of the table we want to create as a symbol. Pretty simple, right? Other methods we can use here are things like `remove_table`, `rename_table`, `remove_column`, `add_column` and others. See this [list](http://edgeguides.rubyonrails.org/active_record_migrations.html#writing-a-migration) for more.
```ruby
# db/migrate/01_create_artists.rb

class CreateArtists < ActiveRecord::Migration
    def change
      create_table :artists do |t|
        t.string :title
        t.integer :length
      end
    end
end    
```
On the left we've given the data type we'd like to cast the column as, and on the right we've given the name we'd like to give the column. The only thing that we're missing is the primary key. `ActiveRecord` will generate that column for us, and for each row added, a key will be autoincremented.

To run the migrations, the simplest way is to use a `raketask` given by the `activerecord gem`. Run `rake -T` to see the list of commands we have. The way in which we get these commands as raketasks is through `require 'sinatra/activerecord/rake'`.
Let's run `rake db:migrate`.

Note our `Artist` class needs to inherit from the `ActiveRecord::Base`. Then we can use the following methods.
```ruby
Artist.column_names
#=> [:id, :name, :genre, :age, :hometown]

a = Artist.new(name: 'Jon')
#=> #<Artist id: nil, name: "Jon", genre: nil, age: nil, hometown: nil>

a.age = 30
#=> 30

a.save
#=> true

a.destroy
=> #<Artist id: 1, name: "Jon", genre: nil, age: nil, hometown: nil>

Artist.create(name: 'Kelly')
#=> #<Artist id: 2, name: "Kelly", genre: nil, age: nil, hometown: nil>

Artist.all
#=> [#<Artist id: 1, name: "Jon", genre: nil, age: 30, hometown: nil>,
 #<Artist id: 2, name: "Kelly", genre: nil, age: nil, hometown: nil>]
 
Artist.find_by(name: 'Jon')
#=> #<Artist id: 1, name: "Jon", genre: nil, age: 30, hometown: nil>
```
The list of [CRUD](http://guides.rubyonrails.org/active_record_basics.html#crud-reading-and-writing-data) methods is longer than that!

##### Manipulating existing tables
Let's add a gender column to our artists table. Remember that ActiveRecord keeps track of what migrations we've already run, so adding it to our 01_create_artists.rb won't work because it won't get executed when we run our migrations again, unless we drop our entire table before rerunning the migration. But that isn't best practices, especially with a production database.

To make this change we're going to need a new migration, which we'll call `02_add_gender_to_artists.rb`. By convention, the `class` name should match the part of the file name after the number.
```ruby
# db/migrate/02_add_gender_to_artists.rb
 
class AddGenderToArtists < ActiveRecord::Migration
  def change
    add_column :artists, :gender, :string
    add_column :artists, :voice, :string
  end
end
```
We basically just told ActiveRecord to add a column to the artists table, call it gender, and it's going to be a string.
Note that `add_column :artists, :gender, :string` is the same as `add_column(:artists, :gender, :string)`

Notice how we incremented the number in the file name there? Imagine for a minute that you deleted your original database and wanted to execute the migrations again. ActiveRecord is going to execute each file, but it has to do so in some order and it happens to do that in alpha-numerical order. If we didn't have the numbers, our add_column migration would have tried to run first ('a' comes before 'c') and our artists table wouldn't have even been created yet!

In reality our two-digit system is very rudimentary. As you'll see later on, frameworks like rails have generators that create migrations with very accurate timestamps so you'll never have that problem.

We can run the migration with rake `db:migrate` and check everything is fine in the console by typing `rake console`. 

In case we need to rollback the migration we can use `rake db:rollback`.

To change the type of a column we can also type:
```ruby
class ChangeDatatypeForGender < ActiveRecord::Migration
  def change
    change_column :artists, :gender, :integer
  end
end
```

To remove an existing column
```ruby
class RemoveGenderFromArtists < ActiveRecord::Migration
  def change
    remove_column :artists, :gender, :integer
  end
end
```

##### ActiveRecord CRUD methods
A few examples of [methods](http://guides.rubyonrails.org/active_record_querying.html)
```ruby
def can_be_instantiated_and_then_saved
  movie = Movie.new
  movie.title = "This is a title."
  movie.save
end

def can_be_created_with_a_hash_of_attributes
  attributes = {
      title: "The Sting",
      release_date: 1973,
      director: "George Roy Hill",
      lead: "Paul Newman",
      in_theaters: false
  }
  movie = Movie.create(attributes)
end

def can_be_created_in_a_block
  movie = Movie.create do |m|
    m.title = "Home Alone"
    m.release_date = 1990
  end
end

def can_get_the_first_item_in_the_database
  Movie.first.title
end

def can_get_the_last_item_in_the_database
  Movie.last.title
end

def can_get_size_of_the_database
  Movie.all.size
end

def can_find_the_first_item_from_the_database_using_id
  Movie.find(1).title
end

def can_find_by_multiple_attributes
  # title == "Title"
  # release_date == 2000, 
  # director == "Me"
  Movie.find_by(title: "Title", release_date: 2000)
end

def can_find_using_where_clause
  # For this test return all movies released after 2002 and ordered by 
  # release date descending
  Movie.where("release_date > 2002").order(release_date: :desc)
end

def can_be_found_updated_and_saved
  # Updtate the title "Awesome Flick" to "Even Awesomer Flick"
  Movie.create(title: "Awesome Flick")
  movie = Movie.find_by(title: "Awesome Flick")
  movie.title = "Even Awesomer Flick"
  movie.save
end

def can_update_using_update_method
  # Update movie title to "Wat, huh?"
  Movie.create(title: "Wat?")
  movie = Movie.find_by(title: "Wat?")
  movie.update(title: "Wat, huh?")
end

def can_update_multiple_items_at_once
  # Change title of all movies to "A Movie"
  5.times do |i|
    Movie.create(title: "Movie_#{i}", release_date: 2000+i)
  end
  Movie.update_all(title: "A Movie")
end

def can_destroy_a_single_item
  Movie.create(title: "That One Where the Guy Kicks Another Guy Once")
  movie = Movie.find_by(title: "That One Where the Guy Kicks Another Guy Once")
  movie.destroy
end

def can_destroy_all_items_at_once
  10.times do |i|
    Movie.create(title: "Movie_#{i}")
  end
  Movie.destroy_all
end
```

### Architecture
ActiveRecord is magic. Well, not really. But it does build out a bunch of methods for you in only a few lines of code. For instance, when it's used properly it will give you access to methods such as `create`, `save`, and `find_by`. Rejoice! Never again will you have to manually build out these methods!

You will only be altering code in six files, the three files in the `models` folder and the three files in the `db/migrations` folder.
```ruby
├── app
│   └── models
│       ├── costume.rb
│       ├── costume_store.rb
│       └── haunted_house.rb
└──db
    └── migrations
        ├── 001_create_costumes.rb
        ├── 002_create_costume_stores.rb
        └── 003_create_haunted_houses.rb
```

ActiveRecord allows you to create a `database` that interacts with your `class` with only a few lines of code. These lines of code go to creating a `model`, which resides in the `app/models` folder, and a `migration`, which resides in the `db/migrations` folder.

The model inherits from `ActiveRecord::Base` while the migration inherits from `ActiveRecord::Migration`. Many migrations these days have a change method, but you might also see migrations with an up and a down method instead. 

#### Migrations
To use ActiveRecord, you have to stick to some specific naming conventions: while the migrations are plural, the models are singular.
The `class` names in the migration files must match their file names. For instance, a class in the migration file called `20141013204115_create_candies.rb` must be named `CreateCandies`.
```ruby
class CreateDogs < ActiveRecord::Migration
  def change
    create_table :dogs do |t|
      t.string :name
      t.string :breed
      t.timestamps
    end
  end
end
```
You might notice that in both the examples above, the numbers at the front of the file name were ignored. These numbers are in the form YYYYMMDDHHMMSS. Later on, these timestamps will become important as Rails uses them to determine which migration should be run and in what order.

`t.timestamps` creates two new columns, created_at and updated_at. These are handy columns to have around as sometimes you want to query based on the time of creation or update-tion instead of querying using attributes or ids.

Data types for migrations

Data type | Examples
------- | ------
boolean | true, false
integer | 2, -13, 485
string | "Halloween", "Boo!", strings betweeen 1-255 characters
datetime | DateTime.now, DateTime.new(2014,10,31)
float | 2.234, 32.2124, -6.342
text | strings between 1 and 2 ^ 32 - 1 characters

After the migration file is ready you can run `rake db:migrate`.

#### Models
Like migrations, models also inherit, but they inherit from `ActiveRecord::Base`. A simple model would look like this:
```ruby
class Dog < ActiveRecord::Base
    ...
end
```
Notice that you immediately get access to `reader` and `writer` methods that cooperated with the database that you never had to actually code: no need to put an `attribute_accesor`. 

This `Dog` model would work as soon as you have a `dogs` table created (like above) and you would be able to call `name`, `name=`, `breed`, `breed=`, and `id` on any new instance of the `Dog` class. You can call `new`, `save` (will run `INSERT` or `UPDATE` SQL commands). 
All that matters is the methods ActiveRecord give you. If you take a class `Song` that doesn't inherit from AR and call `Song.new.methods.size` it will return 105. If you take the Dog class that inherist from AR `Dog.new.methods.size` will return 388. And when you add in the model a `has_many` and `belongs_to` association, it will be even more etc. That's the difference between PORO (plain old ruby object) and an AR object. You inherit from all of the AR modules like `ActiveRecord::AttributeMethods`.

> Don't override an ActiveRecord method!

Let's add the `belongs_to` method to a `Song` class
```ruby
class Song < ActiveRecord::Based
    belongs_to :artist
end
``` 
Now in the terminal lets see the difference in methods, and identify the 7 methods that the `belongs_to` association gives us
```
$ s = Song.new
$ songs_with_association = s.methods
=> [array of all methods with association]
# then we comment out the belongs_to and reload the console
$ reload!
=> true
$ songs_without_association = s.methods
=> [array of all methods without association]
$ songs_with_association - songs_without_association
=> [:belongs_to_counter_cache_after_update, :autosave_associated_records_for_artist, :artist, :build_artist, :create_artist, :create_artist!]
```
The first 2 are low level methods, but the 5 more are: `:artist`, `:build_artist`, `:create_artist`, `:create_artist!`

### Advanced Finder Methods
Active Record makes it easy to ask our database for certain information and datasets by providing a bunch of built-in methods for us.

For example, we can request the sum of all of the values of a particular column with the #sum method:
```ruby
<class name>.sum(:<column_name>)

Song.sum(:duration)
```

We can query our database based on certain conditions using the #where method. Let's say we have a Song class and table and each song has a number_of_stars rating attribute. We could query for songs with more than 3 stars like this:
```ruby
Song.where("number_of_stars > ?", 3)

Song.where("number_of_stars > ?", 3).limit(4)

Song.minimum(:number_of_stars)
Song.minimum("number_of_stars")
```
Let's use a few other methods
```ruby
class Show < ActiveRecord::Base
  def self.highest_rating
    self.maximum(:rating)
  end

  def self.most_popular_show
    self.where("rating = ?", self.highest_rating).first
  end

  def self.lowest_rating
    self.minimum("rating")
  end

  def self.least_popular_show
    self.where("rating = ?", self.lowest_rating).first
    # self.where("rating = ?", lowest_rating).take
    # self.where(rating: self.lowest_rating)[0]
  end

  def self.ratings_sum
    self.sum(:rating)
  end

  def popular_shows
    self.where("rating > ?", 5)
    # self.where("rating > 5")
  end

  def shows_by_alphabetical_order
    self.order(:name)
    # self.all.order("name ASC")
  end
end
```

### ActiveRecord Associations
We already know that we can build our classes such that they associate to one another. We also know that it takes a lot of code to do it. Active Record associations allow us to associate models and their analogous database tables without having to write tons of code.

Active Record makes it easy to implement the following relationships between models:
- belongs_to
- has_one
- has_many
- has_many :through
- has_one :through
- has_and_belongs_to_many

In order to implement these relationships we will need to do two things:
1. Write a migration that creates tables with associations. For example, if a cat belongs to an owner, the cats table should have an owner_id column.
2. Use Active Record macros in the models.

##### Migrations
The different models
A `song` will belong to an artist and belong to a genre.

| id | name | artist_id | genre_id |
| ----- | ----- | ----- | ----- |
| 2 | Shake It Off | 1 | 1 |

These foreign keys, in conjunction with the ActiveRecord association macros will allow us query to get an artist's songs or genres, a song's artist or genre, and a genre's songs and artists entirely through ActiveRecord provided methods on our classes.

> When an object belongs to another object, it ALWAYS has the foreign key

```ruby
class CreateSongs < ActiveRecord::Migration
  def change
    create_table :songs do |t|
      t.string :name 
      t.integer :artist_id
      t.integer :genre_id
    end
  end
end
```
`through` is the one table with the more elements, because `Genre` has-many artists `through` songs, and `Artist` has_many genre `through` songs. 

An `artist` will have many songs and it will have many genres through songs. The table songs is the JOIN table. That means that songs has both an `artist_id` and a `genre_id` to combine those two tables together in a many to many relationship.

| id | name |
| ----- | ----- |
| 1 | Taylor Swift |
Our artists table just needs a name column.
```ruby
class CreateArtists < ActiveRecord::Migration
  def change
    create_table :artists do |t|
      t.string :name
    end
  end
end
```

A `genre` will have many songs and it will have many artists through songs.
| id | name |
| ----- | ----- |
| 1 | pop |
Our genre table just needs a name column.
```ruby
class CreateGenres < ActiveRecord::Migration
  def change
    create_table :genres do |t|
      t.string :name 
    end
  end
end
```
We can now run rake `db:migrate`.

##### AR Macros
Active Record and Routings are the most important things in a Rails App. Spend time plaing around!

A macro is a method that writes code for us (think metaprogramming). By invoking a few methods that come with Active Record, we can implement all of these associations.
We'll be using the following AR macros (or methods):
- [has_many](http://guides.rubyonrails.org/association_basics.html#the-has-many-association)
- [has_many through](http://guides.rubyonrails.org/association_basics.html#the-has-many-through-association)
- [belongs_to](http://guides.rubyonrails.org/association_basics.html#the-belongs-to-association)

```ruby
class Song < ActiveRecord::Base
  belongs_to :artist
  belongs_to :genre
end

class Artist < ActiveRecord::Base
  has_many :songs
  has_many :genres, through: :songs
end

class Genre < ActiveRecord::Base
  has_many :songs
  has_many :artists, through: :songs
end
```

**Here the model that `has_many` is considered the parent. The model that `belongs_to` is considered the child. If you tell the child that it belongs to the parent, the parent won't know about that relationship (while not persisted in DB). If you tell the parent that a certain child object has been added to its collection, both the parent and the child will know about the association.**
```
rake console

[1]pry(main)> hello = Song.new(name: "Hello")
=> #<Song:0x007fc75a8de3d8 id: nil, name: "Hello", artist_id: nil, genre_id: nil>

[2] pry(main)> adele = Artist.new(name: "Adele")
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">

[3] pry(main)> hello.artist = adele
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
[4] pry(main)> hello.artist
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">
[5] pry(main)> hello.artist.name
=> "Adele"

# in memory (as long as not persisted [.persisted? => false]) our parent doesn't know about his child
[6] pry(main)> adele.songs
=> []

---

[7] pry(main)> rolling_in_the_deep = Song.new(name: "Rolling in the Deep")
=> #<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>

# let's add the song to the collection of songs that adele has
[8] pry(main)> adele.songs << rolling_in_the_deep
=> [ #<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>]
# artist_id will be nil as long as not saved in the DB, but still adele.songs returns the good array

# adding the song to the parent, the child knows about the relationship
[9] pry(main)> rolling_in_the_deep.artist
=> #<Artist:0x007fc75b8d9490 id: nil, name: "Adele">

---

[10] pry(main)> pop = Genre.create(name: "pop")
=> #<Genre:0x007fa34338d270 id: 1, name: "pop">

# otherwise the parent wouldn't know about the child
[11] pry(main)> pop.songs << rolling_in_the_deep
=> [#<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>]

[12] pry(main)> pop.songs
=> [#<Song:0x007fc75bb4d1e0 id: nil, name: "Rolling in the Deep", artist_id: nil, genre_id: nil>]

# the child knows about its parent
[13] pry(main)> rolling_in_the_deep.genre
=> #<Genre:0x007fa34338d270 id: 1, name: "pop">

[14] pry(main)> pop.artists
=> [#<Artist:0x007fa342e34dc8 id: 1, name: "Adele">]
```

Note the methods ActiveRecord gives with `macros` and relations

Methods added by `belongs_to`.
```ruby
class Book < ApplicationRecord
    belongs_to :author
end

@book = Book.create(name: "The Old Man and the Sea")

@book.author
@book.author = @author
@book.build_author(author_number: 123, author_name: "John Doe")  
# for initializing a `has_one` or `belongs_to` 
# instead of @book.build() that's used for `has_many` or `has_and_belongs_to_many`
@book.create_author(author_name: "John Doe")
@book.create_author!(author_name: "John Doe")
```

Methods added by `has_one`
```ruby
class Member < ApplicationRecord
  has_one :profile
end

@member.profile
@member.profile = @profile
@member.build_profile(terms: "premium")
@member.create_profile(terms: "premium")
@member.create_profile!(terms: "premium")
```

Methods added by `has_many`
```ruby
class Author < ApplicationRecord
  has_many :books
end

@author.books
@author.books << (object, ...)
@author.books = (objects)
@author.book_ids
@author.book_ids=(ids)

@author.books.build(attributes = {}, ...)
@author.books.create(attributes = {})
@author.books.create!(attributes = {})

@author.books.delete(object, ...)
@author.books.destroy(object, ...)
@author.books.clear

@author.books.empty?
@author.books.size
@author.books.find(...)
@author.books.where(...)
@author.books.exists?(...)
```

Let's recap
```
=== Singular associations (one-to-one)
                                  |            |  belongs_to  |
generated methods                 | belongs_to | :polymorphic | has_one
----------------------------------+------------+--------------+---------
other(force_reload=false)         |     X      |      X       |    X
other=(other)                     |     X      |      X       |    X
build_other(attributes={})        |     X      |              |    X
create_other(attributes={})       |     X      |              |    X
create_other!(attributes={})      |     X      |              |    X

=== Collection associations (one-to-many / many-to-many)
                                  |       |          | has_many
generated methods                 | habtm | has_many | :through
----------------------------------+-------+----------+----------
others(force_reload=false)        |   X   |    X     |    X
others=(other,other,...)          |   X   |    X     |    X
other_ids                         |   X   |    X     |    X
other_ids=(id,id,...)             |   X   |    X     |    X
others<<                          |   X   |    X     |    X
others.push                       |   X   |    X     |    X
others.concat                     |   X   |    X     |    X
others.build(attributes={})       |   X   |    X     |    X
others.create(attributes={})      |   X   |    X     |    X
others.create!(attributes={})     |   X   |    X     |    X
others.size                       |   X   |    X     |    X
others.length                     |   X   |    X     |    X
others.count                      |   X   |    X     |    X
others.sum(*args)                 |   X   |    X     |    X
others.empty?                     |   X   |    X     |    X
others.clear                      |   X   |    X     |    X
others.delete(other,other,...)    |   X   |    X     |    X
others.delete_all                 |   X   |    X     |    X
others.destroy(other,other,...)   |   X   |    X     |    X
others.destroy_all                |   X   |    X     |    X
others.find(*args)                |   X   |    X     |    X
others.exists?                    |   X   |    X     |    X
others.distinct                   |   X   |    X     |    X
others.reset                      |   X   |    X     |    X


=== Singular associations (one-to-one)
                                          |            |  belongs_to  |
        generated methods                 | belongs_to | :polymorphic | has_one
                                          |   :book
------------------------------------------+------------+--------------+---------
@author.books(force_reload=false)         |     X      |      X       |    X
@author.books=(other)                     |     X      |      X       |    X
@author.build_books(attributes={})        |     X      |              |    X
@author.create_books(attributes={})       |     X      |              |    X
@author.create_books!(attributes={})      |     X      |              |    X

=== Collection associations (one-to-many / many-to-many)
                                  |       |          | has_many
generated methods                 | habtm | has_many | :through
----------------------------------+-------+----------+----------
others(force_reload=false)        |   X   |    X     |    X
others=(other,other,...)          |   X   |    X     |    X
other_ids                         |   X   |    X     |    X
other_ids=(id,id,...)             |   X   |    X     |    X
others<<                          |   X   |    X     |    X
others.push                       |   X   |    X     |    X
others.concat                     |   X   |    X     |    X
others.build(attributes={})       |   X   |    X     |    X
others.create(attributes={})      |   X   |    X     |    X
others.create!(attributes={})     |   X   |    X     |    X
others.size                       |   X   |    X     |    X
others.length                     |   X   |    X     |    X
others.count                      |   X   |    X     |    X
others.sum(*args)                 |   X   |    X     |    X
others.empty?                     |   X   |    X     |    X
others.clear                      |   X   |    X     |    X
others.delete(other,other,...)    |   X   |    X     |    X
others.delete_all                 |   X   |    X     |    X
others.destroy(other,other,...)   |   X   |    X     |    X
others.destroy_all                |   X   |    X     |    X
others.find(*args)                |   X   |    X     |    X
others.exists?                    |   X   |    X     |    X
others.distinct                   |   X   |    X     |    X
others.reset                      |   X   |    X     |    X
```

Note also the existence of the method `#find_or_create_by(params)`, that finds the first record with the given attributes, or creates a record with the attributes if one is not found. It runs first a SELECT, and if there are no results an INSERT is attempted.

When designing the form `name=""` tags (that create the params based on the `value="""` tags) make sure to structure the `params` hash so that it will be interpreted nicely by the controller.

A Song has a `name` as defined in its database
If a Song `belongs_to` an :artist => then the song can have an `artist_id`.
If a Song `has_many` :genres => then the song can have an `genre_ids`.
// Thus it makes sense to structure part of the params like
`{"song"=>{"name"=>"Video Games", "artist_id"=>"94", "genre_ids"=>["3", "4"]}, ...}`
And this can be achieved through the `name=""` tags (provided you put the relevant values in them) - case of an edit form (values already populated):
```ruby
<input type="text"      name="song[name]"           value="<%= @song.name %>"   >
<input type="checkbox"  name="song[artist_id]"      value="<%= artist.id %>"    >
<input type="checkbox"  name="song[genre_ids][]"    value="<%= genre.id %>"     >
```

Then if we need to create a new `Artist` instance from scratch, this is out of the scope of an individual song (that can only go as far as `artist_id` and `genre_ids` because this describe the relationship, not the `Artist` object instance itself).

We need to create another part in the `params` hash dedicated to creating the `Artist` instance, containing all informations necessary to initialization.
`{..., "artist"=>{"name"=>"Lana Del Rey", "birthyear"=>"1985"}, ...}`.
And of course the corresponding `form` inputs (in the case of an edit form):
```ruby
<input type="text"      name="artist[name]"         value="<%= @artist.name %>"   >
<input type="checkbox"  name="artist[birthyear]"    value="<%= @artist.birthyear %>"    >
```

And in the controller
```ruby
# Create a song
# params[:artist] = {name: ""}
# params[:song] = {name: "", artist_id: "", genre_ids: []}
post '/songs' do
    @song = Song.create(params[:song])
    @song.artist = Artist.find_or_create_by(params[:artist]) unless params[:artist][:name].empty?
    @song.save
    flash[:message] = "Successfully created song."
    redirect("/songs/#{@song.slug}")
end

# and if we wanted to create the artist (that has many songs, and to whom the song belongs_to)
# { "artist"=>{ "song_ids"=>[] }, "song"=>{ "name"=>"" }, ...}
post '/artist' do 
    @artist = Artist.create(params[:artist])
    @artist.songs << Song.create(name: params["song"]["name"]) unless params["song"]["name"].empty?
    @artist.save  # optional: no need to save it seems since called on AR collection
    redirect to "artists/#{@artist.id}"
end

----------
# Update a song
post '/songs/:slug' do
    # => {"song"=>{"name"=>"Video Games", "artist_id"=>"94", "genre_ids"=>["3", "4"]}, "artist"=>{"name"=>""} ...
    # find by slug is a custom module method
    song = Song.find_by_slug(params[:slug])
    song.update(params[:song])
    song.artist = Artist.find_or_create_by(name: params[:artist][:name]) unless params[:artist][:name].empty?
    song.save
    flash[:message] = "Successfully updated song."
    redirect("/songs/#{song.slug}")
end

# Update an artist
# { "song"=>{ "name"=>"", "artist_id"=>"" }, "artist"=>{ "name"=>"" }, ...}
post '/artist/:id' do 
    @artist = Artist.find(params[:id])
    @artist.update(params[:artist])
    if !params["song"]["name"].empty?
      @artist.songs << Song.create(params["song"])
    end
    redirect to "artists/#{@artist.id}"
end
```  

> As soon as you have something else than relational info (`artist_id`, `genre_ids`, `song_id`...), that is `name`, `birthdate`, you need to create another part to the `params` hash. This will enable you to pass methods like `.create(params[:artist])` using mass assignment instead of regular parameter per parameter assignment.

Mass assignment basically will call the method that has the name of each key (`:artist_id` will be called on the instantiated object as `.artist_id=` - provided there's the column name in the table). Indeed you can associate a post to an author by saying (with p instantiated as a Post instance) `p.author_id=` (and passing the value of the relevant integer), but it's better to use high-level assigners (because an id column can change!): use `p.author=` instead.

### belongs_to: Create & New/Build
There's a subtle difference between `create_association` (similar to create: instantiation and persistence in the DB) and `build_association` (similar to new: instantiation only).
```ruby
class Artist < ActiveRecord::Base
    has_many :songs
end

class Song < ActiveRecord::Base
    belongs_to :artist
end

-----

# on the Song side (child-side)

## building the artist and immediately assigning it to the song
s = Song.new(name: 'Thriller')
s.artist = Artist.new(name: 'MJ')
s.save # Will run 2 INSERTs

## building the artist off the song
s = Song.new(name: 'Runaway')
s.build_artist(name: 'Kanye')
s.save # Will run 2 INSERTs

```

### has_many
2 possibilities to create an has_many association
```ruby
# using build
    # {"figure"=>{"name"=>"Jon", "title_ids"=>["1"], "landmark_ids"=>["1","4"]}, "title"=>{"name"=>"Master Jedi"}, "landmark"=>{"name"=>"Waza Stadium"}}
    figure = Figure.new(params[:figure])
    figure.titles.build(params[:title]) unless params[:title][:name].empty?
    figure.landmarks.build(params[:landmark]) unless params[:landmark][:name].empty?
    figure.save

# using <<
    # {"figure"=>{"name"=>"Jon", "title_ids"=>["1"], "landmark_ids"=>["1","4"]}, "title"=>{"name"=>"Master Jedi"}, "landmark"=>{"name"=>"Waza Stadium"}}
    figure = Figure.create(params[:figure])
    figure.titles << Title.new(params[:title]) unless params[:title][:name].empty?
    figure.landmarks << Landmark.new(params[:landmark]) unless params[:landmark][:name].empty?
```

Let's understand what happens
```ruby
mj = Artist.new(name: "Michael Jackson")
mj.save
[commits to the database 1 INSERT]

mj.songs.build(name: "Beat It")
# instantiated in memory with foreign key
# mj (always the object on the left) is the parent

mj.persisted?
=> true

mj.songs
# will return the object in memory of the songs mj has

mj.save
# persistence in DB: only INSERT 1 for the song, with the right foreign key

---
queen = Artist.new(name: "Queen")
=> #<Artist id: nil, name: "Queen">

queen.persisted?
=> false

queen.songs.build(name: "Bohemian Rhapsody")
=> #<Song id: nil, name: "Bohemian Rhapsody", artist_id: nil>    
# will instantiate a new song in memory, virtually associated even if not with the artist_id

queen.songs
=> #<ActiveRecord::Associations::CollectionProxy [#<Song id: nil, name: "Bohemian Rhapsody", artist_id: nil>]> 
# will return the right collection with the "Bohemian Rhapsody" in memory

queen.save
D, [2016-11-23T12:33:47.074774 #277] DEBUG -- :    (0.2ms)  begin transaction    D, [2016-11-23T12:33:47.081454 #277] DEBUG -- :   SQL (0.5ms)  INSERT INTO "artists" ("name") VALUES (?)  [["name", "Queen"]]      
D, [2016-11-23T12:33:47.082611 #277] DEBUG -- :   SQL (0.2ms)  INSERT INTO "songs" ("name", "artist_id") VALUES (?, ?)  [["name", "Bohemian Rhapsody"], ["artist_id", 11]]                                     
D, [2016-11-23T12:33:47.087615 #277] DEBUG -- :    (4.5ms)  commit transaction
=> true      
```

`queen.songs.build()` will associate the objects in memory. When you save the parent (queen) it will save the child (song), thus running 2 INSERTS:
- persistence in DB: INSERT 1 for the artist (need a value for the primary key for the foreign key of the 2nd insert)
- persistence in DB: INSERT 2 for the song, with the right foreign key

```ruby
# continues previous example

queen.songs << Song.new(name: "Don't stop me now")
D, [2016-11-23T12:36:01.773348 #277] DEBUG -- :    (1.3ms)  begin transaction
D, [2016-11-23T12:36:01.777504 #277] DEBUG -- :   SQL (1.4ms)  INSERT INTO "songs" ("name", "artist_id") VALUES (?, ?)  [["name", "Don't stop me now"], ["artist_id", 11]]                              
D, [2016-11-23T12:36:01.787775 #277] DEBUG -- :    (8.5ms)  commit transaction
=> #<ActiveRecord::Associations::CollectionProxy [#<Song id: 14, name: "Bohemian Rhapsody", figure_id: 11>, #<Landmark id: 15, name: "Don't stop me now", figure_id: 11>]>

queens.song.create(name: "We will rock you")
# INSERTs in the db and returns the saved object, use this one if you don't need all of the collection in memory
# mostly you use the build one and save it, never the create one
```

You are saving the object by pushing as long as the parent is saved (queen). More on this [here](http://stackoverflow.com/questions/11043096/rails-push-into-array-saves-object). Pushing is closer to creating than building (that needs `.save`). it will also always return the collection, which can be useful.


The object on the left is the parent, and on the right is the child. When you save the parent the child gets automatically saved. But if you only save the child the parent won't get saved! 
This is very useful on the belongs_to side (child of the relationship). If you build off the `song` (child of the relationship) doing `s = Song.new.build_artist()` and save `s` you save the parent that instantiated the artist off of itself (the artist is built out of the song: song inside of the method is the parent, artist is the child, so when you save the song the child gets saved).
```ruby
# on the belongs_to :artist side (build_artist off song)

> a = Song.new.build_artist
=> #<Artist id: nil, name: nil, created_at: nil, updated_at: nil, artist_id: nil>

> a
=> #<Artist id: nil, name: nil, created_at: nil, updated_at: nil, artist_id: nil>

> a.save
[will run only 1 INSERT for the artist]
# only the child of the 'Song.new.build_artist' gets saved, on the right side (artist), the parent doesn't get saved. There's no need to have an artist_id (to create an artist) so the song won't get saved.

---
# on the belongs_to :artist side (build_artist off 1 song)

> s = Song.new
=> #<Song id: nil, name: nil, created_at: nil, updated_at:nil, artist_id: nil>
> a = s.build_artist
=> #<Artist id: nil, name: nil, created_at: nil, updated_at:nil, artist_id: nil>
> s.save
# we save the parent, off of which the artist was born (child)
[ will INSERT twice, the artist (to get the primary key) first and the the song with the right foreign key from INSERT 1]
---

# on the has_many :songs side (artist.songs.build)  build from 1 artist's songs collection
a = Artist.new
=> #<Artist id: nil, name: nil, created_at: nil, updated_at:nil, artist_id: nil>

s = a.songs.build
=> #<Song id: nil, name: nil, created_at: nil, updated_at:nil, artist_id: nil>

s.save
# we save the song, which is on the right, so is the child, and it will have to force the saving of the parent (artist) to save the child
```

Anyway playing with the console and the SQL logger. You want to play in the smallest sandbox as possible.
AR is one of the most important parts of the whole stach, and once it clicks it wont't never unclick!


### Aliasing AR associations
3 models
- posts (title, author_id, content)
- users (name)
- comments (post_id, user_id, content)

We want a domain model that will allow us to express queries like
- post has many comments
- post has many commenters (users) through comments
- post belongs to an author (user)
- comment belongs to a post
- comment belongs to a commentor (user)
- user has many authored_posts
- user has many comments
- user has many commented_posts throught comments

Users, commenters, authors are what we call *aliases*. We'll be able to do thinks like
- create a new post for an author
- query the author of a post
- find all the posts a user has commented on

The comments table is kind of like a join table between posts and users.
Take a look at the diagram at thte beginning of the video below.
<iframe width="560" height="315" src="https://www.youtube.com/embed/WVBWlnUghOI?rel=0&amp;showinfo=0" frameborder="0" allowfullscreen></iframe>

Let's start with the model that's the most decoupled/has the less dependencies (foreign key). Then we'll generate the post model because it only has one dependency.

```ruby
class User < ActiveRecord::Base
    has_many :posts
end

class Post < ActiveRecord::Base
    belongs_to :user
end

---

> h = user.create(name: "Hemingway")
[commits to DB]
> sea = post.create(title: "Old man and the sea")
[commits to DB with author_id: nil]
> sea.user = h
=> ActiveModel::MissingAttributeError: can't write unknown attribute 'user_id'
```

The right way using an aliased association would be 
```ruby
class User < ActiveRecord::Base
    has_many :authored_posts,   
            class_name: "Post", 
            foreign_key: "author_id"
end

class Post < ActiveRecord::Base
    belongs_to :author, class_name: "User"
end
```
We need a foreign key for the `has_many` because otherwise it assumes that the `foreign_key` in the other model is named after this class (and would look for `user_id`). The more you alias things the more you need to tell AR about the details.

It enables us to do things like
```ruby
h = User.first
p.author = h
p.save

p.author
=> #<User id:1 ...>
# we're querying for an author but receiving a User! 
# We're not introducing a new class that inherits from User, we're using an alias: it's the same as p.user

h.authored_posts
=> Post load... SELECT "posts".* FROM "posts WHERE "posts"."author_id" = ? [["author_id", 1 ]]
```

Note: for posts we could also have used a foreign key
```ruby
class Post < ActiveRecord::Base
    belongs_to:user, foreign_key: "author_id"
end
```

Let's now do the `Comment` model
```ruby
class Comment < ActiveRecord::Base
    ...
    belongs_to :commentor, 
            class_name: "User", 
            foreign_key: "user_id"
    belongs_to :post        
end

class User < ActiveRecord::Base
    ...
    has_many :comments
    # SELECT * FROM posts WHERE user_id = X
    
    has_many :commented_posts, through: :comments, source: :post
    # i'm looking for the only 2 possibilities :post or :commentor in the Comment model
end

class Post < ActiveRecord::Base
    ...
    has_many :comments
    has_many :commentors, through: :comments, source: :commentor
    # it's looking into the (through) Comment model and has to choose (source) from :commentor and :post
end
```

## AR Object life cycle
An object is born at initialization, and then has a lifecycle.
You cannot overid initialize.
ActiveRecord gives you callbacks, hooks to connect to moments of the object lifecycle (Create, Update, Destroyed).

The validations are made when the object is persisted, if it is not valid it will populate the return value (AR error model) of the  `.error` method with `@messages`. You can call all methods listed in `.errors.local_methods` like `.errors.full_message.to_sentence`, `.valid?`.

You can do custom validations
```ruby
class Artist < ActiveRecord::Base
    has_many :songs
    validates :name, presence: true
    validates :name, length: { minimum: 5 }
    validate :good_song
    
    def good_song
        if self.name !="Taylor Swift"
            errors.add(:good_song, "must be made by  TayTay")
        end
    end
end
```

> Only 3 things you can do in programming: instantiate objects, set variables, define or call method

You can also create actions to fire after a lifecycle event.
```
after_create :email_people
# different from after_save

def email_people
    puts" I've emailed someone"
end    

