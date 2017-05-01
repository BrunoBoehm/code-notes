# NodeJS

Make sure you have nodeJS up and running
```js
node -v
```

For the main app file, your can chose app.js or main.js or index.js and put inside of it
```js
console.log("Hello World!");
console.error("Ooops there was an error");
console.dir({name: "Andrew", age: 22});
```

Run your file with `node app.js`. By typing just `node` your can also open a javascript terminal.

## Connecting to an API
Let's make a `GET` request to the treehouse API and check the response statusCode

```js
// Require https module
const https = require('https');
const username = "chalkers";

// Function to print message to console
function printMessage(username, badgeCount, points) {
  const message = `${username} has ${badgeCount} total badge(s) and ${points} points in JavaScript`;
  console.log(message);
}

// Connect to the API URL (https://teamtreehouse.com/username.json)
// Callback is an arrow function
const request = https.get(`https://teamtreehouse.com/${username}.json`, response => {

                          console.log(response.statusCode);

                          });
```

We're receiving `data` packets in streams, called "buffer" in node. We have to convert them to string with `.toString()`.
Whenever we listen for a `data` event in node.js there is an `end` event, that tells when the streams of data have ended.
We will concatenate the body of the response from all of the received data streams, and we still need to **parse** that giant string `console.log(typeof body)` - turning a string into an data structure / object is called parsing.

```js
const jsonString = '{"name": "Andrew", "languages": ["javascript", "swift", "java", "ruby", "html", "css", "qbasic"]}';

var jsonObject = JSON.parse(jsonString)
```

Here is the final code
```js
//Require https module
const https = require('https');

//Function to print message to console
function printMessage(username, badgeCount, points) {
  const message = `${username} has ${badgeCount} total badge(s) and ${points} points in JavaScript`;
  console.log(message);
}

function getProfile(username) {
  // Connect to the API URL (https://teamtreehouse.com/username.json)
  const request = https.get(`https://teamtreehouse.com/${username}.json`, response => {
                          let body = "";
                          // Read the data
                          response.on('data', data => {
                            body += data.toString();
                          });

                          response.on('end', () => {
                            // Parse the data
                            const profile = JSON.parse(body);
                            // Print the data
                            printMessage(username, profile.badges.length, profile.points.JavaScript);
                          });
                      });

}

const users = process.argv.slice(2);
users.forEach(getProfile);
```

Note that `users.forEach(getProfile);` is similar to
```js
users.forEach( username => {
  getProfile(username)
});
```

Just like the is the `window` global object for javascript in the browser, in node.js we have `process`.
Let's explore it and the `argv` property with `console.dir(process.argv)`. This will give us an array with the path to our node.js files, to our app file, and then the arguments we passed in the console.
This enables us to use our app from the console with `node app.js chalkers alena mcfly`.

### Handling errors
#### Emitted events
On our response object we get an `error` event property. We also get an error message and a stack trace.
We can use the following code to print out the error message to the console.
This is an asynchronous error, that will fail at run time when node tries to access a URL but cannot access it since it doesn't exist.

```js
request.on('error', error => {
  console.error(`Problem with request: ${error.message}`)
});
```

#### Exceptions
If you pass a wrong argument to a node function it will throw an error immediately.
In that case we need to use a `try` and `catch` block like

```js
try {
  ...
} catch(error) {
  console.error(error.message);
}
```

Here is what our function now looks like
```js
function getProfile(username) {
  try {
    // Connect to the API URL (https://teamtreehouse.com/username.json)
    const request = https.get(`https://teamtreehouse.com/${username}.json`, response => {
                            let body = "";
                            // Read the data
                            response.on('data', data => {
                              body += data.toString();
                            });

                            response.on('end', () => {
                              // Parse the data
                              const profile = JSON.parse(body);
                              // Print the data
                              printMessage(username, profile.badges.length, profile.points.JavaScript);
                            });

                          });
    request.on('error', error => console.error(`Problem with request: ${error.message}`));
  } catch (error) {
    console.error(error.message);
  }
}
```

We can also have an error if we query a username that doesn't exist. We can use try and catch again to handle the error.

```js
response.on('end', () => {
  try {
    // Parse the data
    const profile = JSON.parse(body);
    // Print the data
    printMessage(username, profile.badges.length, profile.points.JavaScript);
  } catch (error) {
    printError(error);
  }
});
```

To print all error messages we can abstract out our duplicate code into a printError function like
```js
function printError(error){
  console.error(error.message);
}
```

If the status code is `200` we want to do the parsing, otherwise throw a custom error by creating a new error object.
We can include the `http` module to translate our error codes like 404 into a readable description.

```js
// check if 200 status
if (response.statusCode === 200) {
  let body = "";
  // Read the data
  response.on('data', data => {
    body += data.toString();
  });

  response.on('end', () => {
    try {
      // Parse the data
      const profile = JSON.parse(body);
      // Print the data
      printMessage(username, profile.badges.length, profile.points.JavaScript);
    } catch (error) {
      printError(error);
    }
  });

// otherwise throw a custom error  
} else {
    const message = `There was an error getting the profile for ${username} (${http.STATUS_CODES[response.statusCode]})`;
    const statusCodeError = new Error(message);
    printError(statusCodeError);
}
});
```
