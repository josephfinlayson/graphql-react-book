### Chapter 1

Outline:

Chapter 1: Gearing Up for Development - 35 pages 

Description:

In this chapter we'll setup our development environment and get familiar with the tools and technologies we'll need for this book.

Then we'll discuss the the architecture that we'll be using for our application.

Topics covered:

- Installing and configuring Node using NVM
- Introduction to the GraphQL, Node and React, and how they fit together
- Architecture, Technology & Requirements

Skills learned:

- Thinking critically about how to architect a stack for an application

### Chapter introduction

While this is an advanced book on React, we wanted to cover the basics just enough for anyone to get a quick start. With that thought in mind, we won't be spending much time on the basics and would move to the advanced topics in which we'll have a detailed discussion on how to architecture a complex web application. To summarise, we'll be covering the following in this chapter:

- Installing Node using NVM
- Overview of the tech stack that we have chosen for our application
- Application architectural discussion

## Setting Up Node.js

We'll start off with installing Node.js. You may install Node.js using your favourite package manager, but we recommend using NVM due to the benefits it comes with.

###NVM

NVM (also known as Node Version Manager) is a handy little bash script that allows you to install multiple released versions of Node.js on a single machine. It's helpful in cases where one needs to develop and test applications that are dependent on the version of the Node.js.

###Installing NVM

To install or update NVM, you can use the install script using cURL:

`curl https://raw.github.com/creationix/nvm/master/install.sh |bash`

or Wget:

`wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | bash`

The script clones the nvm repository to `~/.nvm` and adds the source line to your profile (`~/.bash_profile`, `~/.zshrc`, `~/.profile`, or `~/.bashrc`)



**NVM Github Repo:** https://github.com/creationix/nvm

**Note:** NVM doesn't support Windows. However, alternatives exist and one of them is `nvm-windows` that can get from https://github.com/coreybutler/nvm-windows



To verify that NVM has been installed, do:

`command -v nvm`

which should output 'nvm' if the installation was successful. Please note that `which nvm` will not work, since `nvm` is a sourced shell function, not an executable binary.

###Installing Node.js

Now that we have NVM setup, we can simply use the following command to install the latest release of Node.js

`nvm install node`

To check what versions you have installed, use the command

`nvm ls`

From the list of installed versions, you may choose the one you want to use by running the following command

`nvm use [version]`

To verify that Node.js has been installed successfully and check the currently selected version, use the following command

`node -v`

As of now, the latest version is X.Y.Z and we'll be using it for the rest of the book.



##Tech Stack Overview

For this book, we have chosen to build Graphbook - a Facebook clone. We'd have users signing up, managing their profile, sharing posts, and having a newsfeed where they'd be able to view posts shared by other people.

In other words, we're going to be managing users' data, so we'd need a database to persist data. 

As per the modern app development approach, the back-end and front-end parts of the application are built separately, and this is what we're going to be following in our Graphbook as well. So we'd need some APIs that would be consumed by a front-end application.

Now that we've evaluated what we'd need to build, we need to pick technologies that we'd be using for each unit. 

For this book, we've picked the following technologies:

- Node.js (server)
- Express.js (back-end/APIs)
- React (front-end)
- PostgreSQL (database)
- GraphQL

### Node.js

Node.js is an open source JavaScript runtime built on Chrome's V8 JavaScript engine, written by Ryan Dahl in 2009. Traditionally, JavaScript was only used on the front-end part of the application and a different language was needed on the server-side. This changed with the development of Node.js. Applications can now have JavaScript running on server-side as well allowing the possibility of a full JavaScript stack in applications.

Node.js also provides a extensive library of various JavaScript modules which simplifies the development of web applications using Node.js greatly.

We've picked Node.js for our application to allow developers to learn full stack development in JavaScript.

### Express.js

Express.js, or simple Express, is a popular open source unopinionated web framework, written in JavaScript and hosted within the Node.js runtime environment. It provides a robust set of features to develop web applications and APIs.

We've picked Express for our application to make use of its rapid API development.

### React

React, developed by the engineers at Facebook, is a powerful library for creating clear interactive UIs. React provides benefits such as one-way data binding and a component based architecture, which makes it easier to build highly complex, performant applications.

The problem with most JavaScript libraries is the inefficient manipulation of DOM which leads to slow rendering and it has become a serious issue with large modern web applications. This is where React’s virtual DOM comes into play. With virtual DOM, rather than changing the DOM directly, we build an abstract version of it. Think of it as a lightweight copy of our DOM. We can make changes to it and apply it to our real DOM tree. However, saving changes works in a way that it evaluates the difference between the two and only re-renders what should be changed. This gives us immense speed and allows us to build interactive UIs.

We've picked React for our application to make use of the powerful real time features it provides.

### PostgreSQL

PostgreSQL is a powerful, open source object-relational database system. It has more than 15 years of active development and a proven architecture that has earned it a strong reputation for reliability, data integrity, and correctness. It is highly scalable both in the sheer quantity of data it can manage and in the number of concurrent users it can accommodate.

It runs on all major operating systems, including Linux, UNIX (AIX, BSD, HP-UX, macOS, Solaris), and Windows. It is fully ACID compliant, has full support for foreign keys, joins, views, triggers, and stored procedures (in multiple languages).

We've picked PostgreSQL for the fact that it's an enterprise class database.

### GraphQL

GraphQL, also developed by the engineers at Facebook, is a query language for APIs and a runtime for fulfilling those queries with your existing data. It provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.

GraphQL APIs are organized in terms of types and fields, not endpoints. From a single endpoint, the full capabilities of data can be accessed. Each level of a GraphQL query corresponds to a particular type, and each type describes a set of available fields. Similar to SQL, this allows GraphQL to provide descriptive error messages before executing a query.

Another important feature of GraphQL is its hierarchical nature. It naturally follows relationships between objects, where a REST service may require multiple round-trips (which may be resource-intensive on mobile networks) or a complex join statement in SQL. This data hierarchy pairs well with graph-structured data stores and ultimately with the hierarchical user interfaces it's used within.

We've picked GraphQL for our application because of its robust features.