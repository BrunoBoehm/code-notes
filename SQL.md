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

