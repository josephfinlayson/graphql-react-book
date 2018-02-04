### Chapter 2

Outline:

Chapter 2: Creating our first API in express - 20 pages
Description

In this chapter we will start our server in Express, the most widely used Node.JS server side framework. Firstly, we'll answer the question: "Why GraphQL?" We'll cover some of the problems with REST APIs.

Then we'll write set up our simple graphQL server, write the simplest "Hello World" API - and  we’ll learn to use Insomnia, to test our GraphQL APIs.

Topics covered
- Getting up and running with Express
- Creating GraphQL APIs
- Getting familiar with Insomnia for API development

Skills learned
The reader will learn about GraphQL APIs and how they can build them using Express. They will learn about testing APIs using Insomnia

### Chapter introduction


Chapter 1. What is Express?
This chapter covers

Node.js, a JavaScript platform typically used to run JavaScript on servers Express, a framework that sits on top of Node.js’s web server and makes it easier to use.

Before we talk about Express, we need to talk about Node.js.

Javascript, after many years languishing in the shadows, to add tiny decorations to websites, got, in 2009, Node.js a bigger brother - Node.js. It took V8, Google Chrome’s powerful JavaScript engine, out of the browser and enabled it to run on servers. In the browser, developers had no choice but to pick JavaScript. In addition to Ruby, Python, C#, Java, and other languages, developers could now choose JavaScript when developing server-side applications.

JavaScript might not be the perfect language for everyone, but Node.js has real benefits. For one, the V8 JavaScript engine is fast, and Node.js encourages an asynchronous coding style, making for faster code while avoiding multithreaded nightmares. JavaScript also had a bevy of useful libraries because of its popularity. But the biggest benefit of Node.js is the ability to share code between browser and server. Developers don’t have to do any kind of context switch when going from client and server. Now they can use the same code and the same coding paradigms between two JavaScript runtimes: the browser and the server.

Node.js caught on—people thought it was pretty cool. Like browser-based JavaScript, Node.js provides a bevy of low-level features you’d need to build an application. But like browser-based JavaScript, its low-level offerings can be verbose and difficult to use.

Enter Express, a framework that acts as a light layer atop the Node.js web server, making it more pleasant to develop Node.js web applications.

*Comment: Mention Sinatra*

Express is philosophically similar to jQuery. People want to add dynamic content to their web pages, but the vanilla browser APIs can be verbose, confusing, and limited in features. Developers often have to write boilerplate code, and a lot of it. jQuery exists to cut down on this boilerplate code by simplifying the APIs of the browser and adding helpful new features. That’s basically it.

### Traditional RESTful Routing in ExpressJS

Express gives us something called "routing". This matches a certain URL, or URL pattern to a certain handler. Routing is a way to map different requests to specific handlers. Handlers "handle" a request, and return a certain response.

Here is an example of how Express traditionally handles RESTful requests.

```js
  const express = require("express");
  const http = require("http");
  const app = express();

  // The * symbol matches all requests. In this example it adds a content type header
  app.all("*", function(request, response, next) {
    response.writeHead(200, { "Content-Type": "application/json" });
    next();
  });

  // This matches requests that are to the root of the application, i.e. http://example.com
  app.get("/", function(request, response) {
    response.end({"{greeting: \"Welcome home!\" }"});
  });

  // This matches requests that are to the about page of the application, i.e. http://example.com/about
  app.get("/about", function(request, response) {
    response.end("{greeting: \"Welcome about!\" }");
  });

  http.createServer(app).listen(8080);
```

The three calls to app.get are Express's routing system. They could also be app.post, which respond to POST requests, or PUT, or any of the HTTP verbs. The first argument is a path, like /about or /. The second argument is a request handler similar to what we've seen before. To quote the Express documentation:

These routes can get smarter, with things like this:

```js
  const express = require("express");
  const http = require("http");
  const app = express();
  const getCatDetailsForBreed = require('./getCatDetailsForBreed');

  const getDogDetailsForBreed = require('./getDogDetailsForBreed');
  // This matches requests that are to the root of the application, i.e. http://example.com/dog/golden_retriever
  app.get("/dog/:breed", function(request, response) {
    const dog = getDogDetailsForBreed(request.params.breed)
    response.end(dog);
  });

  // This matches requests that are to the about page of the application, i.e. http://example.com/cat/persian
  app.get("/cat/:breed", function(request, response) {
    const cat = getCatDetailsForBreed(request.params.breed)
    response.end(cat);
  });

  http.createServer(app).listen(8080);
```
In this example `:breed` becomes a wildcard parameter which matches whatever the API consumer places into the URL route, and the places the value into `request.params.breed`. This means the consumer can inject arbitrary data into the backend, and for example, get cat and dog breed details from the database.

This is lovely, and works really well if you are a full stack developer. You can create your backend API routes, and then program your frontend to access them and the data that they return. It also works well if you are part of a small team, where those creating and consuming APIs can sit together and work together. However, this becomes a pain when working in large teams where frontend and backend work separately.

### Problems with RESTful web services


#### Problem 1: naming conventions
Firstly, let's look at the role of conventions. In Ruby, for some reason the convention is to name the keys of a JSON object using underscore case. I.e. `dog_breed` - while as we know, in JS the convention to write in camelCase - i.e. `dogBreed`. This is a minor problem, but there's no real good solution to solving this. Either the API consumer or the API producer needs to conform to the naming conventions of the other side. This is the type of thing that leads to *Holy Wars* between different development teams. GraphQL solves this problem by letting the frontend name the data structure in the way they want.

Secondly, RESTful APIs, the structure of requests and responses change, and the naming conventions of each key in a JSON dictionary change reasonably often. If we are working in a large team, the change needs to be coordinated between four groups of people.
  - The database engineers that will change the name of the column in the database.
  - The backend engineers to change how the access that database, as well as change variable naming for that value
  - Frontend engineers will need to adapt how they use the data from the API, as well as their variable naming
  - And lastly, a sysadmin or DevOps engineer will have to ensure all these changes are deployed at the same time!


#### Problem 2: entity structures


Another issue comes from the JSON structure itself. Often API structures resemble the databases or microservices from which they originate. For example you may have the following very simple architecture


*Actually draw image*
```
                  ---> Breed Microservice
FE -> API Gateway                         ---> DB
                  ---> Dog Microservice
```

As a backend developer, this is how this would be implemented. It makes logical sense for the domain model of a backend developer. This is also how REST endpoints are meant to be implemented.

In Roy Fieldings initial description of REST https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm#sec_5_2_1_1:

  >The key abstraction of information in REST is a resource. Any information that can be named can be a resource: a document or image, a temporal service (e.g. “today’s weather in Los Angeles”), a collection of other resources, a non-virtual object (e.g. a person), and so on. In other words, any concept that might be the target of an author’s hypertext reference must fit within the definition of a resource. A resource is a conceptual mapping to a set of entities, not the entity that corresponds to the mapping at any particular point in time.

  *Rewrite in plain english*
  A resource can be a singleton or a collection. For example, “dogs” is a collection resource and “dog” is a singleton resource. We can identify “dogs” collection resource using the URI “/dogs”. We can identify a single “dog” resource using the URI “/dogs/:dogName”.

```js
  const express = require("express");
  const http = require("http");
  const app = express();
  const getCatDetailsForBreed = require('./getCatDetailsForBreed');
  const getDogDetailsForBreed = require('./getDogDetailsForBreed');


  app.get("/dog/:name", function(request, response) {
    const dog = getDogDetailsForName(request.params.breed)
    response.json({name: dog.name, breedType: dog.breedType, age: dog.age, photo: dog.photo});
  });

  // Imagine this API endpoint as a separate microservice.
  app.get("/breed/:name", function(request, response) {
    const breed = getDogDetailsForBreedName(request.params.breed)
    response.json({
      breedType: breed.breedType,
      breedLongevity: breed.breedLongevity
    });
  });

  http.createServer(app).listen(8080);

```

However, as a frontend developer it absolutely sucks for some use cases. For example, imagine we are trying to render a facebook page for a dog

*mockup of page with dog details and breed details*

To populate this page, we need to make two requests, one after the other. First we get the dog details, which contains the breed type, then we need to make a second call to the breed endpoint to get the details of the breed! Having single endpoints for each entity means that clients must make several calls to populate a UI that displays multiple entities. This massively slows down the internet!

We can see that Roy Fielding's REST specification can accomadate this by allowing for collections of resources. However, to define all possible collections of resources according to the presentation would be a lot of work, and require a lot of iteration, and could also lead to overfetching - which we'll explore more in the next subsection

GraphQL solves this by ensuring the client will always be performing POST operations and placing a query in the body. This query contains what information the client is requesting.

The server will respond with a JSON object containing the information the client requests. The amount of requests is limited - only one request was done even though we were requesting information on two different entities, and finally it provides flexibility to the frontend developers to define what data they need.

*Recreate image for dogs*
![Alt text](https://cdn-images-1.medium.com/max/1600/1*f6WxMRJd_1cOf-m8Yo_RZg.png)

#### Problem 2: Overfetching

This approach solves the under or over fetching of data (because the client gets only the data it asked for),

Also endpoints return a fixed data structure, so clients will usually under or over fetch information, resulting in unnecessary data transfer.

Finally, the entity structure is not flexible enough when trying to manipulate several pieces of data at the same time. It’s not uncommon for clients to perform a high number of calls to the API to solve a single problem.
