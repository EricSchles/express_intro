#Intro to Express.js

Express is a web framework for the middleware of your node application.  It's responsible for passing data from the backend to the front end and vice versa.  For those of you coming from ruby express is similar to [sinatra](http://www.sinatrarb.com/); for those of you coming from python it is similar to [flask](http://flask.pocoo.org/).

##Prerequisites

Writing applications with Express.js means understanding the core of JavaScript first and the node run time first.  Please do check out [this introduction to javascript](http://www.syncano.com/getting-know-javascript-intro/) and [this introduction to node](https://github.com/EricSchles/intro_node) if you are unfamiliar.

##Installation

Express.js is extremely easy to install.  All one needs is to have node already installed.  Since node.js comes with npm, the node package manager, it is extremely easy to install express once you already have node.

To install open a terminal and type the following:

sudo npm install express 
sudo npm install hbs #not required
sudo npm install body-parser #not required

While not required in general, hbs will make our lives much easier, so please do install this as well.

#####Note: npm only installs packages to a directory and any of its subdirectories.  Therefore if you try to run express from some other disassociated directory, it won't work.

##Verifying installation

Once you have express installed, the next thing to do is ensure your installation is correct.

Do this by opening a terminal and type:

`node`

This opens up the node REPL, which allows you to type in small pieces of code (typically one liners) to verify your code is correct.  This is great for testing small pieces of code and allows your code to be more bug free.

Once you've done the above, type:

`var express = require("express");`

If this returns undefined it means you have installed express correctly and are ready to move on!

##Hello World

Now that you know Express.js is installed correctly, enter the following into a file called `app.js` (this is by convention, you can call it whatever you like).  

```
var express = require("express");
var app = express(); //starts up your app

app.get("/",function(req,res){
	res.send("Hello world");
});

app.get("/Hi", function(req,res){
	res.send("Hi");
});

app.listen(5000);
console.log("Server started on http://localhost:5000");
```

You can run this by going to the directory you saved this file in and typing out the following:

`node app.js`

This will start the web server.  Then you can head over to http://localhost:5000 in your favorite web browser and the words Hello world should be displayed to you.

So let's break down what happened here:

First we started a new `app` object, this is initialized by called `express();`

The app object has various methods associated with it.  Check out the [full list here](http://expressjs.com/api.html).  Each method is set with respect to the server.  For example above, `app.get()` means when a get request is sent to the server, do the following action.  Notice there are two parameters, "/", and a function.  The first parameter is called the route and tells the server when to call the action.  This is relative to the root domain of the website.  So "/" refers to localhost:5000/.  Everything after localhost:5000 is what is server will use to make calls for different data or actions.  So the "/Hi" route will be called when we type in localhost:5000/Hi to the browser.  

The next thing to understand is the second parameter, a callback.  This function determines what will be sent back to the browser from the server.  In this case both routes just send back a string.  This is great for testing your server and making sure your routes work.  This won't always be the case, so it's important to test at this stage with a dummy route.

The final piece of code that should be new is `app.listen(5000);`.  This code tells the server to run and to run on port 5000.  

##Comparing express with node

Technically everything you can do with express you can do with node, however compare the following pieces of code, one written with express and the other with node.  Note: they do the same thing.

Written with Node.js alone:
```

var http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(5000);

console.log("Server running on http://localhost:5000");
```

Written with Express.js and Node.js:
```
var express = require("express");
var app = express(); //starts up your app

app.get("/",function(req,res){
	res.send("Hello world");
});

app.listen(5000);
console.log("Server started on http://localhost:5000");
```

Notice how much less code you need to write with the code written using express.  The routing information isn't even clear in the Node.js code.  Also it requires 3 lines to do the same thing in the Node.js code.  Clearly, the express code is superior in terms of readability and functionality.

##A Real example

Now that we understand how easy it is to get up and running.  Let's start working with our front end and making it dynamic.  For that we'll need hbs.  

####Verifying installation

```
var express = require("express");
var app = express();
var hbs = require("hbs");

app.set("view engine", "html");
app.engine("html", hbs.__express);

app.get("/", function(req,res){
	res.render("index");
});

app.listen(5000);

console.log("Server started on http://localhost:5000")
```

This piece of code isn't dynamic.  I include it mostly to go over the basics of hbs.  Notice there is bit more set up required:

`app.set("view engine", "html");` - This setups up what hbs will render, in our case html
`app.engine("html", hbs.__express);` - this tells the hbs we'll be making use of html and express.

The second thing to notice is res.render.  This assumes that there exists a document called index.html in a folder called views.  So the file structure of the app should now be:

```
root_dir/
 app.js
 views/
	index.html
```

In general, res.render([file name]) and looks for views/[file name].html

####A Real example:

app.js
```
var express = require("express");
var app = express();
var hbs = require("hbs");
var bodyParser = require("body-parser");

app.set("view engine", "html");
app.engine("html", hbs.__express);
app.use(bodyParser.urlencoded());

app.get("/", function(req,res){
	res.render("index",{name:"Eric Schles",greeting:"Hello there"});
});

app.listen(5000);

console.log("Server started on http://localhost:5000")
```

views/index.html:

```
<!doctype html>
<html>
<head></head>
<body>

<p>{{{greeting}}},</p>
<p>{{{name}}}</p>

</body>
</html>
```

Notice that we added a little more boilerplate:  

`app.use(bodyParser.urlencoded());`

This will be used to parse the html code that is passed to the render function in our routing method.

notice also that we pass in json to our index.  The keys above will be used in the html as references and the values in those keys will be displayed.  So above our json has two keys: name and greeting.  In our html we make use of the placeholder notation - {{{ }}} and the values will be rendered dynamically.  

The reason this is powerful is now we can substitute in the json we have here for a database call, which will be rendered on demand.  Of course that won't be for a few more weeks.

 

  