[Heroku 101](https://github.com/ibigfoot/heroku-101) | [Heroku 201](https://github.com/ibigfoot/heroku-201) | [Heroku 301](https://github.com/ibigfoot/heroku-301) | [Heroku 401](https://github.com/ibigfoot/heroku-401)

# Heroku 101

Creating your first Node app on Heroku application is pretty simple, but you will need a few things installed before we get started.


## Prerequisites 
So, for the code in this sample to work you will want to have [NodeJS](https://nodejs.org/en/download/) installed and running, my node installed version is
```
> node -v
v7.10.0
```

Lastly, if you haven't got yourself a [Github](https://github.com) account, you will want to sign up as by the end of the day we are going to be deploying our applications directly from Github!

## Heroku CLI
Next, you will definitely want the [Heroku CLI](https://devcenter.heroku.com/articles/using-the-cli) installed.
Go to the [Getting Started with Java](https://devcenter.heroku.com/articles/getting-started-with-java#set-up) setup page and download the Heroku CLI for your environment. 

A quick version check shows
```
> heroku -v
heroku-cli/6.7.4-97fc5c6 (darwin-x64) node-v7.10.0
```

You will know you have it right when you can run the heroku --help command and see this.

```
> heroku --help
Usage: heroku COMMAND [--app APP] [command-specific-options]

Help topics, type heroku help TOPIC for more details:

 access         # manage user access to apps
 addons         # manage add-ons
 apps           # manage apps
 authorizations # OAuth authorizations
 buildpacks     # manage the buildpacks for an app
 certs          # a topic for the ssl plugin
 ci             # run an application test suite on Heroku
 clients        # OAuth clients on the platform
 config         # manage app config vars
 domains        # manage the domains for an app
 drains         # list all log drains
 features       # manage optional features
 git            # manage local git repository for app
 keys           # manage ssh keys
 labs           # experimental features
 local          # run heroku app locally
 logs           # display recent log output
 maintenance    # manage maintenance mode for an app
 members        # manage organization members
 notifications  # display notifications
 orgs           # manage organizations
 pg             # manage postgresql databases
 pipelines      # manage collections of apps in pipelines
 plugins        # manage plugins
 ps             # manage dynos (dynos, workers)
 redis          # manage heroku redis instances
 regions        # list available regions
 releases       # manage app releases
 run            # run a one-off process inside a Heroku dyno
 sessions       # OAuth sessions
 spaces         # manage heroku private spaces
 status         # status of the Heroku platform
 teams          # manage teams
```

## Create a Node Application
Lets get started with writing some code. Today we are going to build and deploy an [Express](https://expressjs.com/) Web Application onto our Heroku service. 
Express is pretty easy to get started with, includes a bunch of stuff including dependency management, i18n and other wonderful bits right out of the box. If you have a preference for a different web app stack please go ahead with that or feel free to continue with the Heroku Dev Center getting started [article](https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction). 

We are going to document two different mechanisms of getting started with Express here, one that is completely from scratch as well as how we can use Express Generator to scafold a more complete web application. I would recommend the generator option personally, but each to their own on this one. The code samples in these tutorials have all been created using the Express Generator with the Handlebars templating engine.

### Option 1 : Express From Scratch
For the first pass, let's start from scratch with Express...

```
> mkdir heroku-101
> cd heroku-101
> npm init
```
When starting with NPM init, I simply just accept all the defaults, feel free to enter what you like here.
```
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (heroku-101) 
version: (1.0.0) 
description: 
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: 
license: (ISC) 
About to write to /Users/local/demos/heroku/heroku-101/package.json:

{
  "name": "heroku-101",
  "version": "1.0.0",
  "description": "Simple node app",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this ok? (yes) 
```
You now have a node app, lets add our Express web framework
```
> npm install express --save
```

Create a file index.js in the root of the directory and add the following code.. 
```
'use strict'

const express = require('express');
const app = express();

app.get('/', function (req, res) {
  res.send('Hello World!');
});
var PORT = system.env.PORT || 3000

app.listen(PORT, function () {
  console.log('Example app listening on port 3000!');
});
```

Also, open the generated package.json file and add a start script
```
   ...
   "scripts": {
      "start": "node index.js",
      "test": "echo \"Error: no test specified\" && exit 1"
   },
   ...
```

Now from the command line in your project root directory, start your server
```
> npm start
```

And you should see a very, very simple web application at [http://localhost:3000](http://localhost:3000)

### Option 2 : Express Generator

Express provides a generator application that allows us to skip a lot of the setup and build a fully scafolded web application, along with routing and view templating and I find this a lot easier to get started. But first, we need the Express Generator installed on our local environment

```
> npm install express-generator -g
```

Now you will find it much easier to create your first working Express application

```
> express --view=hbs heroku-101

   create : heroku-101
   create : heroku-101/package.json
   create : heroku-101/app.js
   create : heroku-101/public
   create : heroku-101/public/images
   create : heroku-101/public/javascripts
   create : heroku-101/routes
   create : heroku-101/routes/index.js
   create : heroku-101/routes/users.js
   create : heroku-101/public/stylesheets
   create : heroku-101/public/stylesheets/style.css
   create : heroku-101/views
   create : heroku-101/views/index.hbs
   create : heroku-101/views/layout.hbs
   create : heroku-101/views/error.hbs
   create : heroku-101/bin
   create : heroku-101/bin/www

   install dependencies:
     $ cd heroku-101 && npm install

   run the app:
     $ DEBUG=heroku-101:* npm start

tsellers-ltm:heroku tsellers$ 
```
Run the install and then start to get the view of your new application. 

I have created my app using the Handlebars view templating system, there are many options for view templating included with Express. To view, simply run the help command on the generator

```
> express -h

  Usage: express [options] [dir]

  Options:

    -h, --help           output usage information
        --version        output the version number
    -e, --ejs            add ejs engine support
        --pug            add pug engine support
        --hbs            add handlebars engine support
    -H, --hogan          add hogan.js engine support
    -v, --view <engine>  add view <engine> support (ejs|hbs|hjs|jade|pug|twig|vash) (defaults to jade)
    -c, --css <engine>   add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
        --git            add .gitignore
    -f, --force          force on non-empty directory
```

Cool, lets get this guy running in Heroku now! 

## Create an Heroku App

So the first thing we need to do is initialise our Git repository, Heroku makes [heavy use](https://devcenter.heroku.com/articles/git) of the [Git](https://git-scm.com/) version control system for deployment. If you are not familiar with Git and the process for handling code using it I would recommend something like this [cheat sheet](https://www.cloudways.com/blog/git-cheat-sheet/) to get you started. 

Firstly, let's create a git repository out of our codebase. 

```
> git init
> git status
```

This should list all the files that need to be added to your git repository. One thing to note, is there are files in here that don't belong in source control. You might have eclips project files for example, your node_modules directory and perhaps the OS X .DS_Store file. Lets add a .gitignore and make sure our repository remains clean. (Below I use the VI editor, but that is just a personal choice.. you can use any text editor to create the file called .gitignore)

```
> vi .gitignore
```

Then simple add an entry per line for things you want to keep out of source control, like this.

```
node_modules/
npm-debug.log
```

Now we can add files to our git repository and commit. 

```
> git add .
> git commit -am "initial commit"
```

If you are yet to authenticate to the Heroku CLI, lets do that first

```
> heroku login
```

Then, to create your Heroku application the command you need to get started is just one.. 
```
> heroku create 
Creating app... done, ⬢ radiant-springs-88284
https://radiant-springs-88284.herokuapp.com/ | https://git.heroku.com/radiant-springs-88284.git
```

You're app is probably not called radiant-springs, but you get the general idea. 

You should see that Heroku has added a git remote to your local codebase. You will see in mine I have been allocated the application name "boiling-cliffs-26828", you can see this application in your [Heroku Dashboard](https://dashboard.heroku.com). Open this and find your app, it will look pretty empty at the moment as we haven't pushed any code or configure our codebase to run on Heroku. 

## Getting our App to Run - The Procfile

The [Procfile](https://devcenter.heroku.com/articles/procfile) is a specific type of file that tells Heroku what is the command it should run to start the application that has just been loaded. You can declare multiple process types (indeed we will in the 301 section!) but for now let's just declare a simple web process. Create a file named 'Procfile'

```
> touch Procfile
```

And then add this one line to the file

```
web: npm start
```

Here we are directing Heroku to run a NodeJS application with the start script in package.json, super simple stuff.

## Push Your App to Heroku

So we need to commit our new changes

```
> git add .
> git commit -am "add Heroku support"
```

Now the fun part, let's deploy your application to Heroku! 

```
> git push heroku master
... (much build output)
> heroku open

```

And there you have it!! You have built and deployed your Heroku application from scratch. Cool huh?

Now, if for some reason yours isn't working as described and you really aren't too bothered to debug it.. 

try clicking this button 

[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)


