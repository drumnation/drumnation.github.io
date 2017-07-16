---
layout: post
title: "How Sails' MVC framework for Node.js compares to Ruby on Rails"
date: 2017-05-08 11:36:06 -0400
---
# What is Sails?

<img class="center" src="/images/sailsVrails/07-sails-overview.jpg">

# And Rails?

<img class="center" src="/images/sailsVrails/08-rails.png">

# Install

I'm going to give you a quick visualization of some of the syntactic
differences between the two frameworks so you can see how
similiar sails seems to be compared to rails.  This means it could be
a much smoother transition to becoming full stack Javascript
coming from Rails and ActiveRecord than one might have thought.

### Sails
<pre><code>$ npm install sails -g</code></pre>

### Rails
<pre><code>$ gem install rails</code></pre>

# Rails = Generators <br> Sails = Blueprints

### Sails
<pre><code>$ sails new test-project</code></pre>

### Rails
<pre><code>$ rails new blog</code></pre>

# Start Server

### Sails

<pre><code>$ sails lift</code></pre>

Loads the app, runs its bootstrap, then starts listening for HTTP requests and WebSocket connections

### Rails

<pre><code>$ bin/rails server</code></pre>

# Creating a controller

To get each MVC saying "Hello", you need to create at minimum a controller and a view.

### Sails

<pre><code>$ sails generate controller welcome index</code></pre>

Creates a controller named "Welcome" with an action of "index"

### Rails

<pre><code>$ bin/rails generate controller Welcome index</code></pre>

Creates a controller named "Welcome" with an action of "index"

# Configuring routes

To get these MVCs saying "Hello", you need to create at minimum a controller and a view.

### Sails
``` javascript
'GET /welcome/index': 'WelcomeController.index'
```

Sails allows you to manually bind routes to controller actions using the config/routes.js file.

Resourceful Routing If the URL is not specified in config/routes.js, the default route for a URL is: /:controller/:action/:id where :controller, :action, and the :id request parameter are derived from the URL.

<pre><code># Backbone Conventions
	GET   :	/:controller			=> findAll()
	GET   :	/:controller/read/:id		=> find(id)
	POST  :	/:controller/create		=> create()
	POST  :	/:controller/create/:id		=> create(id)
	PUT   :	/:controller/update/:id		=> update(id)
	DELETE:	/:controller/destroy/:id	=> destroy(id)

	# You can also explicitly state the action
	GET   :	/:controller/find		=> findAll()
	GET   :	/:controller/find/:id		=> find(id)
	POST  :	/:controller/create		=> create(id)
	PUT   :	/:controller/update/:id		=> update(id)
	DELETE:	/:controller/destroy/:id	=> destroy(id)</code></pre>

If :action is not specified, Sails will redirect to the appropriate action. Out of the box, Sails supports RESTful resourceful route conventions, as used in Backbone.js.

### Rails

```ruby
get 'welcome/index', to: 'welcome#index'
```

Rails allows you to manually bind routes to controller actions using the config/routes.rb file.

Rails allows you to define resources that generate appropriate routes.

``` ruby
Rails.application.routes.draw do
  get 'welcome/index'

  resources :articles

  root 'welcome#index'
end
```

Which produces these default routes:

<pre><code>$ bin/rails routes
      Prefix Verb   URI Pattern                  Controller#Action
    articles GET    /articles(.:format)          articles#index
             POST   /articles(.:format)          articles#create
 new_article GET    /articles/new(.:format)      articles#new
edit_article GET    /articles/:id/edit(.:format) articles#edit
     article GET    /articles/:id(.:format)      articles#show
             PATCH  /articles/:id(.:format)      articles#update
             PUT    /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
        root GET    /                            welcome#index
        </code></pre>

# Configuring Controller Actions

### Sails

``` javascript
module.exports = {
  hi: function (req, res) {
    return res.send('Hi there!');
  },
  bye: function (req, res) {
    return res.redirect('http://www.sayonara.com');
  }
};
```

A controller file defines a Javascript dictionary (aka "plain object") whose keys are action names, and whose values are the corresponding action methods.

### Rails

``` ruby
class ArticlesController < ApplicationController

def new
  @article = Article.new
end

def edit
  @article = Article.find(params[:id])
end

def create
  @article = Article.new(article_params)

  if @article.save
    redirect_to @article
  else
    render 'new'
  end
end
```

# Configuring Views

### Sails

<img src="/images/sailsVrails/js_view.png">

By default, Sails is configured to use EJS (Embedded Javascript) as its view engine. The syntax for EJS is highly conventional- if you've worked with php, asp, erb, gsp, jsp, etc., you'll immediately know what you're doing.

### Rails

<img src="/images/sailsVrails/ruby_view.png">

Rails views consist of html and embedded Ruby.

# Models and ORMS

### Sails

Sails comes installed with a powerful ORM/ODM called Waterline, a datastore-agnostic tool that dramatically simplifies interaction with one or more databases.

In schemaful databases like Postgres, Oracle, and MySQL, models are represented by tables. In MongoDB, they're represented by Mongo "collections".

### Rails

Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic.

Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database.

# Connecting a Database

### Sails
``` javascript
// in config/connections.js
// ...
{
  adapter: 'sails-mysql',
  host: 'localhost',
  port: 3306,
  user: 'root',
  password: 'g3tInCr4zee&stUfF'
}
// ...
```

A connection represents a particular database configuration. This configuration object includes an adapter to use, as well as information like the host, port, username, password, and so forth.

### Rails

Active Record is the M in MVC - the model - which is the layer of the system responsible for representing business data and logic.

Active Record facilitates the creation and use of business objects whose data requires persistent storage to a database.

# File and Directory Structure

### Sails

### All Folders
<img class="center" src="/images/sailsVrails/01-sails-files.png">

### API Folder Expanded
<img class="center" src="/images/sailsVrails/02-sails-api.png">

### Assets Folder Expanded
<img class="center" src="/images/sailsVrails/03-sails-assets.png">

### Tasks Folder Expanded
<img class="center" src="/images/sailsVrails/04-sails-tasks.png">

### Views Folder Expanded
<img class="center" src="/images/sailsVrails/05-sails-views.png">

### Config Folder Expanded
<img class="center" src="/images/sailsVrails/06-sails-config.png">

# Rails

### All Folders
<img class="center" src="/images/sailsVrails/09-rails-all.png">

### App Folder Expanded
<img class="center" src="/images/sailsVrails/10-rails-app.png">

### Bin Folder Expanded
<img class="center" src="/images/sailsVrails/11-rails-bin.png">

### Config Folder Expanded
<img class="center" src="/images/sailsVrails/12-rails-config.png">

### DB Folder Expanded
<img class="center" src="/images/sailsVrails/13-rails-db.png">

### Lib Folder Expanded
<img class="center" src="/images/sailsVrails/14-rails-lib.png">

### Public Folder Expanded
<img class="center" src="/images/sailsVrails/15-rails-public.png">

### Test Folder Expanded
<img class="center" src="/images/sailsVrails/16-rails-test.png">

# Verdict

They seem similar enough to me that I'm going to build a project
in node.js.  I wish we had used this on our client side app for my last project
because the JS MVC pattern we tried to make ourselves ended up
being really confusing.

Give it a try and let me know how it goes!
