# Computer Science

## Week 0 - From 0 and 1
### Bits
The bit is a basic unit of information in computing and digital communications. A bit can have only one of two values, and may therefore be physically implemented with a two-state device. These values are most commonly represented as either a 0 or 1.
Bits (bi[nary digi]ts) are 0 and 1. 

Bits are transmitted one at a time in serial transmission, and by a multiple number of bits in parallel transmission. A bitwise operation optionally process bits one at a time. Data transfer rates are usually measured in decimal SI multiples of the unit bit per second (bit/s), such as kbit/s.

Transistors turns on and off based on how we program them.

### Bytes / Octet
The `Byte` is a unit of digital information that most commonly consists of `eight bits.
Historically, the byte was the number of bits used to encode a single character of text in a computer and for this reason it is the smallest addressable unit of memory in many computer architectures.
The de facto standard of eight bits is a convenient power of two permitting the `values 0 through 255 for one byte`.
An octet is a byte, that is eight bits. This unit of digital information is often used in computing and telecommunications when the term byte might be ambiguous, since historically there was no standard definition for the size of the byte.

A byte is symbolically marked as the upper-case B by the IEEE in contrast to the bit, lower-case b. 

### Abstracting on bytes
The patterns of 0 and 1 are interpreted depending on the extension of the file .jpg .png .mov etc.
rgb(255,34,77) uses 3 bytes.

## Algorythms
Process to do something from an input and return and output.
Computationnal thinking to formalize approaches to solve a problem.
You can `pseudocode`: "if it's rainy then I'm going to take my jacket"
* Do, open, look : they are procedures, actions, statements
* if, else, then:  they are conditionals
* you need loops to repeat commands

## Servers
The internet operates based on conversations between the client (more familiarly known as the browser) and the server (the code running the web site you're trying to load). By typing in that URL into your browser, you (the client) are requesting a web page. The server then receives the request, processes it, and sends a response. Your browser receives that response and shows it to you.

**Browsers send requests, and servers send responses.**

Being able to switch out both the server or the client happens because the way browsers and servers talk is controlled by a contract or protocol. Specifically it is a protocol created by Tim Berners-Lee called the `Hyper Text Transfer Protocol` or `HTTP`.

![requests](https://s3.amazonaws.com/learn-verified/requests.png)

### Browsers send requests
Requests are sent to a specific  URL/URI (Uniform Resource Identifier). The URI `http://www.youtube.com/adelevevo` is broken into three parts:
* `http` is the protocol (other exist: SMTP for emails, HTTPS for secure requests, FTP for file transfers)
* `youtube.com` is the domain
* `/adelevevo` is the resource

You need to use a HTTP verb with the URI to specify what action you would like the server to do.
Using a browser, almost all requests are GET requests. This just means "hey server, please `GET` me this resource". There are a few other verbs though. What if we want to send some data from the user to the server? This is done with a `POST` request.

Verb | Description
--- | ---
HEAD | Asks for a response like a GET but without the body
GET | Retrieves a representation of a resource
POST | Submits data to be processed in the body of the request
PUT | Uploads a representation of a resource in the body of the request
DELETE | Deletes a specific resource
TRACE | Echoes back the received request
OPTIONS | Returns the HTTP methods the server supports
CONNECT | Converts the request to a TCP/IP tunnel (generally for SSL)
PATCH | Apply a partial modification of a resource

When the client makes a request, it includes other items besides just the URL in the "headers." The request header contains all the information the server needs in order to fulfill the request: the type of request, the resource (path), the domain as well as some other metadata like what type of browser is making this request. The request header would look something like this.

![Headers](https://s3.amazonaws.com/learn-verified/request-header.png)

### Servers send responses
Once your server receives the request, it will do some processing based on code server side and then send a response, the response header would look something like (header+body):
![Response](https://s3.amazonaws.com/learn-verified/response-headers.png)

Each response has a specific status code
- 100's - informational
- 200's - success
- 300's - redirect
- 400's - error
- 500's - server error



## Week 1 - C language beginnings
The C programming language is a popular and widely used programming language for creating computer programs. Programmers around the world embrace C because it gives maximum control and efficiency to the programmer.
Why it's a great language to start from:
- you will be able to read and write code for a large number of platforms -- everything from microcontrollers to the most advanced scientific systems can be written in C, and many modern operating systems are written in C.
- The jump to the object oriented C++ language becomes much easier. C++ is an extension of C, and it is nearly impossible to learn C++ without learning C first.

Read more about [C Language](http://computer.howstuffworks.com/c2.htm)
C is what is called a compiled language. This means that once you write your C program, you must run it through a C compiler to turn your program into an executable that the computer can run (execute). 

The C program is the human-readable form, while the executable that comes out of the compiler is the machine-readable and executable form (0 and 1).

If you are using a UNIX machine, the C compiler is available for free. It is called either "cc" or "gcc" and is available on the command line. If we write a program into a file and save it as add.c. Compile it with the line gcc add.c -o add and then run it by typing add (or ./add).

```c
#include <stdio.h>

int main(void)
{
    printf("hello, world\n")
}
```

And from the terminal we can type
```
$ make hello
make: 'hello' is up to date.
# this compiles the program
$ ./hello
hello, world
```
The compiler is `clang hello.c` but if we don't specific the name of the program to output (hello.c) we can just type `./a.out`
```
$ clang hello.c
# to compile the code
$ .a/out
# to run the code
hellow, world
```

If you type `clang -o hello hello.c` tells the output to be hello. Make is just a simpler program to compile.
```
$ make hello
cland -ggdb3 -O0 -std=c99 -Wall - Werror   hello.c -lcs50 -lm -o hello
$ ./hello
hello, world
```
Let's now add a variable to the program, by declaring a string
```c
#include <cs50.h>
// to be able to use string
#include <stdio.h>

int main(void)
{
    string s = "Hanna";
    printf("hello, %s\n", s);
}
```
The semicolon terminates the line. %s is the placeholder for the variable. Everytime we make a modification we need to recompile the program.

Note: `./` looks at the current directory. `../` it goes up one directory
```c
#include <cs50.h>
// to be able to use string
#include <stdio.h>

int main(void)
{
    // ask user for input
    printf("Give me an integer");
    int x = GetInt();
    printf("Give me another integer");
    int y = GetInt();
    
    // do the math (%i is integer, %f would be float)
    printf("The sum of %i and %i is %i\n", x, y, x+y)
    // \n moves the cursor to the next line (new line)
}
```
Note: This C program starts with #include <stdio.h>. This line includes the "standard I/O library" into your program. The standard I/O library lets you read input from the keyboard (called "standard in"), write output to the screen (called "standard out"), process text files stored on the disk, and so on. It is an extremely useful library. The printf statement allows you to send output to standard out. For us, standard out is generally the screen (although you can redirect standard out into a text file or another command).

The line int main() declares the main function. Every C program must have a function named main somewhere in the code. At run time, program execution starts at the first line of the main function. In C and for this "main" block of code, the { and } symbols mark the beginning and end of a block of code.

The scanf function allows you to accept input from standard in, which for us is generally the keyboard. The scanf function can do a lot of different things, but it is generally unreliable unless used in the simplest ways. It is unreliable because it does not handle human errors very well. But for simple programs it is good enough and easy-to-use.
```
#include <stdio.h>

int main()
{
    int a, b, c;
    printf("Enter the first value:");
    scanf("%d", &a);
    printf("Enter the second value:");
    scanf("%d", &b);
    c = a + b;
    printf("%d + %d = %d\n", a, b, c);
    return 0;
}
```
You MUST put & in front of the variable used in scanf. The reason why will become clear once you learn about pointers. It is easy to forget the & sign, and when you forget it your program will almost always crash when you run it.

### CLI
As a programmer a CLI is more useful than the GUI. We're going to talk about the UNIX-based system, which includes Linux and MacOS. The CS50 IDE is a cloud-based machine running Ubuntu, one of the many flavors of the Linux OS. Windows is not a UNIX based system.

In the terminal
```
// list content of directory
$ ls

// change directory
$ cd directory_name
// shorthand for current directory is ".", parent directory is ".."
$ ..

// move two levels up
$ ../..

// move to the root directory
$ cd
// or
$ cd ~

// print present working directory to know the name of the current directory
$ pwd

// clear the screen
ctrl l

// make a directory
$ mkdir new_directory

// copy source hello.txt to destination hi.txt
$ cp hello.txt hi.txt

// to copy a directory that has content you use recursive
$ cp -r pset0 pset3

// to delete a file, will ask a confirmation y/n
$ rm file_to_remove

// to delete a file without confirmation
$ rm -f file_to_remove

// to delete a directory (and its content)
$ rm -r folder_to_remove

// to delete a directory recursively and forcively (and its content)
$ rm -rf folder_to_remove

// to move a file from source to destination or rename
$ mv greddy.c greedy.c
```

### Data Types
C is an older language that PHP of Javascript, with specfic data types.

C understands several data types:
- char (single byte) for any single character, only 8 bits (1 byte). Map 0 to 127 to ASCII standard.
- float (32 bits = 4 bytes) with a decimal point, also known as real. floats have imprecision problems we cannot have 200 digits.
- double: is twice as big as a float to be more precise, 64 bits (8 bytes).
- int: this data type is used for variables that store integers. Always take 4 bytes of memory (32 bits). This means the range of values they can store is limited to 32 bits worth of information. Biggest 0 to 4 billion (2^32) if you use an unsigned int (qualifier), otherwise actually -2^32 to +2^32 with regular integers.
- string: very short to very long series of characters. Doesn't exist in C you need the <cs50.h> library.
- bool: capable of holding true or false values. Doesn't exist in C you need the <cs50.h> library.
- later in the course we'll encounter structures (struct) and defined types (typedefs) that afford great flexibility in creating data types.

void is not a data type, just a type. Like a placeholder for "nothing"
- a void type doesn't return anything, like printf
- the paramter of a function can also be void, it means that the function doesn't take parameters

To create a variable you need 
1. a type
2. a name
```
int number;
char letter;

// to create multiple variables at the same time
int height, width;
float sqrt2, sqrt3, pi;
```

To use a variable, once it has been declared you don't need to specify the data type anymore
```
int number;
number = 17;

// you can also initialize by simultaneously declaring and setting
int number = 17;
```

### Operators
In order to manipulate and work with variables and values in C, we have a number of operators. 

#### Arithmetic operators
The value on the right gets assigned to the value on the left.
```c
int y = 10;
int x = y + 1;
x = x * 5;

int m = 13 % 4;
// m is now 1 (the remainder, modulus operator)

x = x * 5;
x*= 5;

// shorthand for incrementing or decrementing
x++;
x--;
```

Every non-0 value is similar as true. 0 is false. We need logical operators.
```
x && y 
// only true if x is true, y is true

x || y
// only false if x is false, y is false

!x
// bang/not is false if x is true


if ((x==y) && (j>k))
    z=1;
else
    q=10;
```

Relational operators
````
x < y
x <= y

x == y
x != y
```

Placeholders
You can print all of the normal C types with printf by using different placeholders:
- int (integer values) uses %d
- float (floating point values) uses %f
- char (single character values) uses %c
- character strings (arrays of characters, discussed later) use %s

The table's values are in increments of 10 degrees. The %f symbol has some formatting applied to it: The value will be printed with six digits preceding the decimal point and two digits following the decimal point.
```
#include <stdio.h>

int main()
{
    float a;
    a = 0;
    while (a <= 100)
    {
        printf("%6.2f degrees F = %6.2f degrees C\n",
            a, (a - 32.0) * 5.0 / 9.0);
        a = a + 10;
    }
    return 0;
}
```

escape sequences
```
\n \r \t \' \" \\ \ 
```

### Functions
```c
#include <stio.h>
#include <cs50.h>

int main(void) {
    printf("Please give me an int: ");
    int x = GetInt();
    
    printf("Please give me another int: ");
    int y = GetInt();    
    
    printf("The sum of %d and %d is %d.\n", x, y, x +y);
}
```


functions to get input
```
GetChar()
GetFloat()
GetDouble()
...
```

#### Conditional statements
Let's do an if...else if...else statement. These are mutually exclusive.
```c
#include <cs50.h>
#include <stdio.h>

int main(void){
    printf("Pleas give me and int:");
    int x = GetInt();
    
    if (x > 0) {
        printf("You picked a positive integer.\n");
    } else if (n == 0) {
        printf("You picked zero!\n");
    } else {
        printf("You picked a negative integer!\n");
    }
}
```

To make it non-mutually exclusive (only the 3rd and 4th are mutually exclusive - "else" binds the nearest "if").
```
if (boolean-expression1) {
    // first branch
}
if (boolean-expression2) {
    // second branch
}
if (boolean-expression3) {
    // third branch
}
else {
    // fourth branch
}
```

You can also use the ternary operator, to write shorter code and simulate and if/else.
```
int x = (expr) ? 5 : 6

// which is the same as
int = x;
if (expr) {
    x = 5;
} else {
    x = 6;
}
```

C's Switches statements are conditional statements that permit enumeration of discrete cases, instead of relying on Boolean expressions.
It's important to `break;` between each case, or you'll "fall through" each case (unless that's what you want).
```
int x = GetInt();
switch (x) {
    case 1:
        printf("One!\n");
        break;
    case 2:
        printf("Two!\n");
        break;
    default:
        printf("Sorry...\n");
        break;
}
```

#### Loops
```
for (initialization; condition; updates) {
    // do this again and again
}

for (int i = 0; i < 50; i++) {
    printf(%i\n);
}

int i
for (i = 0; i < 50; i++) {
    printf(%i\n);
}
```
Curly braces can be left out if you only have one line of code.
```
while (condition) {
    // do this again and again
}

int x=1;
while (x<10)
{
    blah blah blah
    x++; 
    /* x++ is the same as saying x=x+1 */
}

do {
    // do this again and again
}
while (condition)
```









