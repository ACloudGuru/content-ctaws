# Transitioning the API Code

## Get a Copy of the Code
The Plant Shop code can be found in GitHub at https://github.com/ACloudGuru/ctaws-plant-shop. Feel free to make your own fork of the project!

You can work with the code on your own machine, or directly via GitHub (if you make your own fork).

Get a copy of the source code on your machine like this:

```
git clone https://github.com/ACloudGuru/ctaws-plant-shop.git
```

The main source code file for the API is `api/server.js`. We will need to edit this source code to bo ready for Lambda.

If you want to see an example of how it should look at the end, a Lambda-ready version of the code can be found in the project at `api/lambda/index.js`.

## Modify the Code
The main change is to remove the built-in express web server and run the code in an event handler instead. Lambda has no need of a built-in web server, as it will handle all web request functionality. Instead, Lambda will pass an "event" to our code, and our code needs to be set up to process the event.

We can set up an event handler with `exports.handler`. The final version could look something like this:

```
var mysql = require('mysql');

exports.handler = (event, context, callback) => {

    var connection = mysql.createConnection({
        host: process.env.DB_HOST,
        user: process.env.DB_USER,
        password: process.env.DB_PASS,
        database: "plantshop",
    });

    connection.query('select * from items', function (error, results, fields) {
        if (error) {
            connection.destroy();
            throw error;
        } else {
            // connected!
            console.log("Query result:");
            console.log(results);
            callback(error, {items: results});
            connection.end(function (err) { callback(err, results);});
        }
    });
};
```
