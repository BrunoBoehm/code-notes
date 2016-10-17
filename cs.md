# Computer Science

## From 0 and 1
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



## 
