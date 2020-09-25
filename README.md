# **What is EJS? How To Use EJS to Template Your Node Application?**

## **Embedded JavaScript** is a simple templating language that lets you generate HTML markup with plain JavaScript. No religiousness about how to organize things. No reinvention of iteration and control-flow. It's just plain JavaScript.

### **Features**
    1. Fast compilation and rendering
    2. Simple template tags: <% %>
    3. Custom delimiters (e.g., use [? ?] instead of <% %>)
    4. Sub-template includes
    5. Ships with CLI
    6. Both server JS and browser support
    7. Static caching of intermediate JavaScript
    8. Static caching of templates
    9. Complies with the Express view system


### Installing EJS with NPM
> $ npm install ejs

## **Introduction** 

When creating quick on-the-fly Node applications, an easy and fast way to template our application is sometimes necessary.

EJS is does that job well and is very easy to set up in your nodejs application too. Let’s take a look at how we can create a simple application and use EJS to **include repeatable parts of our site (partials)** and **pass data to our views.**

## **Setting up the Demo App**

We will be making two pages for our application having one page with full width and the other with a sidebar.

## ** File Structure**

Here are the files we’ll need for our application. We’ll do our templating inside of the views folder and the rest is pretty standard Node practices.

    - views
    ----- partials
    ---------- footer.ejs
    ---------- head.ejs
    ---------- header.ejs
    ----- pages
    ---------- index.ejs
    ---------- about.ejs
    - package.json
    - server.js

**package.json** will hold our Node application information and for the dependencies we need (express and EJS). 

**server.js** will hold our Express server setup, configuration. We’ll define our routes to our pages here.

## **Node Setup**

## Let’s go into our package.json file and set up our project there.

> ##                   **package.json**

    {
    "name": "node-ejs",
    "main": "server.js",
    "dependencies": {
        "ejs": "^3.1.5",
        "express": "^4.17.1"
        }
    }

##### Make sure to double check your indentations 

Next we will need Express and EJS and to have them we need to install the dependencies we just defined. Go ahead and run:

> $ npm install

With all of our dependencies installed, let’s configure our application to use EJS and set up our routes for the two pages we need: the index page (full width) and the about page (sidebar). We will do all of this inside our server.js file.

> ###                   **server.js**

    // load the things we need
    var express = require('express');
    var app = express();

    // set the view engine to ejs
    app.set('view engine', 'ejs');

    // use res.render to load up an ejs view file

    // index page
    app.get('/', function(req, res) {
        res.render('pages/index');
    });
    // about page
    app.get('/about', function(req, res) {
        res.render('pages/about');
    });

    app.listen(3000);
    console.log('3000 is the magic port');

Here we define our application and set it to show on port 8080. We also have to set EJS as the view engine for our Express application using app.set('view engine', 'ejs');. Notice how we send a view to the user by using res.render(). It is important to note that **res.render() will look in a views folder** for the view. So we only have to define pages/index since the full path is views/pages/index.


## Start Up our Server

Go ahead and start the server using:

>    node server.js

Now we can see our application in the browser at http://localhost:3000 and http://localhost:3000/about. Our application is set up and we have to define our view files and see how EJS works there.


## Create the EJS Partials

Like a lot of the applications we build, there will be a lot of code that is reused. We’ll call those **partials** and define three files we’ll use across all of our site: head.ejs, header.ejs, and footer.ejs. Let’s make those files now.

> ###                   **views/partials/head.ejs**

    <meta charset="UTF-8">
    <title>EJS Is Fun</title>

    <!-- CSS (load bootstrap from a CDN) -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.5.2/css/bootstrap.min.css">
    <style>
        body { padding-top:50px; }
    </style>

> ###                   **views/partials/header.ejs**

<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="/">EJS Is Fun</a>
  <ul class="navbar-nav mr-auto">
    <li class="nav-item">
      <a class="nav-link" href="/">Home</a>
    </li>
    <li class="nav-item">
      <a class="nav-link" href="/about">About</a>
    </li>
  </ul>
</nav>


> ###                   **views/partials/footer.ejs**

<p class="text-center text-muted"> © Copyright 2020 The Awesome People</p>

## Add the EJS Partials to Views

We have our partials defined now. All we have to do is include them in our views. Let’s go into index.ejs and about.ejs and use the include syntax to add the partials.

## Syntax for including an EJS Partial

> Use <%- include('RELATIVE/PATH/TO/FILE') %> to embed an EJS partial in another file.

    + The hyphen <%- instead of just <% to tell EJS to render raw HTML.
    + The path to the partial is relative to the current file.

> ###                   **views/pages/index.ejs**

<!DOCTYPE html>
<html lang="en">
<head>
    <%- include('../partials/head'); %>
</head>
<body class="container">

<header>
    <%- include('../partials/header'); %>
</header>

<main>
    <div class="jumbotron">
        <h1>This is great</h1>
        <p>Welcome to templating using EJS</p>
    </div>
</main>

<footer>
    <%- include('../partials/footer'); %>
</footer>

</body>
</html>

1. Now we can see our defined view in the browser at http://localhost:3000.

2. For the about page, we also add a bootstrap sidebar to demonstrate how partials can be structured to reuse across different templates and pages.

> ###                   **views/pages/about.ejs**

<!DOCTYPE html>
<html lang="en">
<head>
    <%- include('../partials/head'); %>
</head>
<body class="container">

<header>
    <%- include('../partials/header'); %>
</header>

<main>
<div class="row">
    <div class="col-sm-8">
        <div class="jumbotron">
            <h1>This is great</h1>
            <p>Welcome to templating using EJS</p>
        </div>
    </div>

    <div class="col-sm-4">
        <div class="well">
            <h3>Look I'm A Sidebar!</h3>
        </div>
    </div>

</div>
</main>

<footer>
    <%- include('../partials/footer'); %>
</footer>

</body>
</html>


3. If we visit http://localhost:3000/about, we can see our about page with a sidebar! 


Now we can start using EJS for passing data from our Node application to our views.

## Pass Data to Views and Partials

Let’s define some basic variables and a list to pass to our home page. Go back into your server.js file and add the following inside your app.get('/') route.

> ###                   **server.js**

// index page
app.get('/', function(req, res) {
    var mascots = [
        { name: 'Sammy', organization: "DigitalOcean", birth_year: 2012},
        { name: 'Tux', organization: "Linux", birth_year: 1996},
        { name: 'Moby Dock', organization: "Docker", birth_year: 2013}
    ];
    var tagline = "No programming concept is complete without a cute animal mascot.";

    res.render('pages/index', {
        mascots: mascots,
        tagline: tagline
    });
});

We have created a list called mascots and a simple string called tagline. Let’s go into our index.ejs file and use them.


## **Render a Single Variable in EJS**
To echo a single variable, we just use <%= tagline %>. Let’s add this to our index.ejs file:

> ###                   **views/pages/index.ejs**

...
<h2>Variable</h2>
<p><%= tagline %></p>
...

## **Loop Over Data in EJS**

To loop over our data, we will use .forEach. Let’s add this to our view file:

> ###                   **views/pages/index.ejs**

...
<ul>
    <% mascots.forEach(function(mascot) { %>
        <li>
            <strong><%= mascot.name %></strong>
            representing <%= mascot.organization %>, born <%= mascot.birth_year %>
        </li>
    <% }); %>
</ul>
...

Now we can see in our browser the new information we have added!

## **Pass Data to a Partial in EJS**

The EJS partial has access to all the same data as the parent view. But be careful: If you are referencing a variable in a partial, it needs to be defined in every view that uses the partial or it will throw an error.

You can also define and pass variables to an EJS partial in the include syntax like this:

> ###                   **views/pages/about.ejs**

...
<header>
    <%- include('../partials/header', {variant:'compact'}); %>
</header>
...

But you need to again be careful about assuming a variable has been defined.

If you want to reference a variable in a partial that may not always be defined, and give it a default value, you can do so like this:

> ###                   **views/partials/header.ejs**

...
<em>Variant: <%= typeof variant != 'undefined' ? variant : 'default' %></em>
...

In the line above, the EJS code is rendering the value of variant if it’s defined, and default if not.

## Conclusion
### EJS lets us spin up quick applications when we don’t need anything too complex. By using partials and having the ability to easily pass variables to our views, we can build some great applications quickly.
