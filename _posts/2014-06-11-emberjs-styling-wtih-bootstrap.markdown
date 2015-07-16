---
layout:     post
title:      Styling EmberJS with Bootstrap
date:       2015-07-14 12:00:00
summary:    Let's start a simple EmberJS app
categories: Bootstrap EmberJS EmberCLI
---

Every time I work with EmberJS and start putting together the base structure of the application, I always start looking for guides and tutorials to add a basic styling for it.

I decided to start with Twitter Bootstrap, the most common styling framework out there; however there are many ways to achieve this, and I don't want to write a book about this, so let's follow three different routes to achieve this.

## Setup the base application

Let's create a simple **EmberJS** application with three routes: index, about and contact. It will not connect to any REST service, it will only showcase some of Bootstrap's features.

First we need to get *ember-cli*, as this is the official recommendation by the **EmberJS** team to start an application.

{% highlight bash %}

% npm install -g ember-cli

{% endhighlight %}

My current *ember-cli* version is **1.13.1**; you can easily check this with *ember -v*.

With *ember-cli* installed we can create a new application; we do this with the new command from ember. After that let's step into the directory created by *ember-cli*, and fire up the application with *ember serve*.

{% highlight bash %}

% ember new ember-bootstrap
% cd ember-bootstrap
% ember serve

{% endhighlight %}

Open your favorite browser, and go to [localhost:4200](http://localhost:4200/), and you'll see nice welcome message from your new ember applicagtion.

![EmberJS Welcome page](/images/posts/ember-welcome.png)

With the app ready, it is time to add our new routes. Let's start with about.

{% highlight bash %}

% ember generate route about

{% endhighlight %}

The generator creates the following files:
- /app/routes/about.js
- /app/templates/about.hbs
- /test/unit/routes/about-test.js

It also edits */app/router.js* to include the new route:

{% highlight javascript %}

import Ember from 'ember';
import config from './config/environment';

var Router = Ember.Router.extend({
  location: config.locationType
});

Router.map(function() {
  this.route('about');
});

export default Router;

{% endhighlight %}

For the purpose of this post, we will ignore the generated javascript files, and will focus on the new template. Open up */app/templates/about.hbs* in your favorite editor, replace the {{outlet}} line with the following:

{% highlight html %}

<h1>About</h1>
<p>Ember basic components</p>
<ul class="list-group">
  <li class="list-group-item">Route</li>
  <li class="list-group-item">Model</li>
  <li class="list-group-item">View</li>
  <li class="list-group-item">Template</li>
  <li class="list-group-item">Controller?</li>
</ul>
<div class="panel panel-default">
  <div class="panel-heading">
    <h3 class="panel-title">Ember 2.0 is comming</h3>
  </div>
  <div class="panel-body">
    <p>The next version of ember will continue
    to improve the way we create web applications</p>
  </div>
</div>

{% endhighlight %}

With our *about* route created, let's do the same for *contact*:

{% highlight bash %}

% ember generate route contact

{% endhighlight %}

The generator will create the following files:

- /app/routes/contact.js
- /app/templates/contact.hbs
- /test/unit/routes/contact-test.js

It also edits */app/router.js* to include the new route.

Open up */app/templates/contact.hbs* and replace its contents with:

{% highlight html %}

<h1>Contact</h1>
<form>
  <div class="form-group">
    <label for="inputEmail">Email address</label>
    <input type="email" class="form-control" id="inputEmail"
      placeholder="Email">
  </div>
  <div class="form-group">
    <label for="inputComments">Comments</label>
    <textarea class="form-control" id="inputComments"
      placeholder="Comments"></textarea>
  </div>
  <button type="submit" class="btn btn-default">Submit</button>
</form>

{% endhighlight %}

Wait! What happened with the index route? **EmberJS** already created that route for us when we used *ember-cli* to generate our application. It doesn't generate any files for it, but it is already there; the ember guides site provides more explanation to this: [Routes](http://guides.emberjs.com/v1.12.0/routing/defining-your-routes/).

The only thing that we need to add, is the template for our index route. We could easily add the file directly to the templates folder, but I feel it is more interesting to use the *ember-cli* generator:

{% highlight bash %}

% ember generate template index

{% endhighlight %}

Edit */app/templates/index.hbs* and replace its contents with:

{% highlight html %}

<h1>Welcome to ember-bootstrap</h1>

<div class="row">
  <div class="col-md-6">
    <a href="http://emberjs.com/" target="_blank">EmberJS</a>
  </div>
  <div class="col-md-6">
    <a href="http://getbootstrap.com/" target="_blank">Bootstrap</a>
  </div>
</div>

{% endhighlight %}

Great! We have our three routes with its proper content ready. However, if we got to our browser (make sure you are running *ember serve*), we see two different welcome messages, and no easy way to reach the about and contact routes.

We need to setup a simple navigation bar and remove the original welcome message. To do so, we will edit */app/templates/application.hbs* with the following content:

{% highlight html %}

<nav class="navbar navbar-default">
  <div class="container-fluid">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed"
      data-toggle="collapse" data-target="#menu-collapse">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      {{link-to 'Home' 'index' class="navbar-brand" }}
    </div>

    <div class="collapse navbar-collapse" id="menu-collapse">
      <ul class="nav navbar-nav">
        <li>{{link-to 'About' 'about'}}</li>
        <li>{{link-to 'Contact' 'contact'}}</li>
      </ul>
    </div>
  </div>
</nav>

<div class="container">
  {{outlet}}
</div>

{% endhighlight %}

This will setup a basic navigation that allows to reach the about and contact routes. The styling is quite ugly, but we will setup that in the next section.

![Unstyled EmberJS Welcome page](/images/posts/unstyled-index-page.png)

## Add Boostrap using a CDN

With our application foundation ready, we can proceed to add Bootstrap; the quickest way to achieve this is to reference it with CDN. According to [Bootstrap CDN](http://getbootstrap.com/getting-started/#download-cdn) documentation, we only need to add the following:

{% highlight html %}

<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css">

<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap-theme.min.css">

<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js">
</script>

{% endhighlight %}

Open */app/index.html* and add the previous lines inside the *head* tag between the two content-for tags. Now, go to your browser and refresh your application page.

![Styled EmberJS Welcome page](/images/posts/styled-index-page.png)

We got styling in place! However, if you resize the application small enough to show the mobile menu button (hamburger icon), you'll see that it won't do anything when you click on it.

![Collapsed Menu mobile page](/images/posts/mobile-index-collapsed-menu.png)

The reason for this is that the Bootstrap javascript reference was incorporated in our head section; we need to move it inside the body section, after the application javascript file (ember-bootstrap.js).

{% highlight html %}

<script src="assets/vendor.js"></script>
<script src="assets/ember-bootstrap.js"></script>

<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js">
</script>

{% endhighlight %}

Now we have our app fully functional.

![Collapsed Menu mobile page](/images/posts/mobile-index-collapsed-menu.png)

## Add Bootstrap using bower with simple CSS

Adding Bootstrap with [Bower](http://bower.io/) is much easier. First we need to run the following command to install Bootstrap in our bower dependencies:

{% highlight html %}

% bower install bootstrap --save

{% endhighlight %}

After that, we need to configure our application to import Bootstrap; this is achieved by adding to */ember-cli-build.js* three import lines for:

- /bower_components/bootstrap/dist/css/bootstrap.css
- /bower_components/bootstrap/dist/css/bootstrap-theme.css
- /bower_components/bootstrap/dist/js/bootstrap.js

The lines should be added after the *app* variable is defined.

{% highlight javascript %}

var EmberApp = require('ember-cli/lib/broccoli/ember-app');

module.exports = function(defaults) {
  var app = new EmberApp(defaults, {
    // Add options here
  });

  app.import('bower_components/bootstrap/dist/css/bootstrap.css');
  app.import('bower_components/bootstrap/dist/css/bootstrap-theme.css');
  app.import('bower_components/bootstrap/dist/js/bootstrap.js');

  return app.toTree();
};

{% endhighlight %}

Let's restart our ember application and then refresh our browser.

![About page](/images/posts/about-page.png)

## Add Boostrap Using bower with SASS

Pending post ...
