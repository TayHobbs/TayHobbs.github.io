---
layout: post
title:  "Basic Ember App"
date:   2014-08-24 20:51:32
image: "../../../../../../../images/ember-app.jpeg"
categories: interning
---

With the coming of Ember-CLI, I would recommend you use it, rather than rolling your own solution
with Grunt or Gulp like you would need to with this guide.


Writing a basic Ember.js application is simple... After you spend 2 weeks memorizing
all the naming conventions. After banging my head against a wall seemingly forever,
I've finally gotten over the initial hump. I know there are hundreds of Ember tutorials
on the internet, but I'd like to take my shot at it, if only for myself.
So,

### Step 1:

Make sure you include jQuery, Handlebars, and Ember

Easy enough,

### Step 2:

In an app.js file, create the app like so.
{% highlight javascript %}
App = Ember.Application.create({})
{% endhighlight %}
"App" can obviously be anything you want, but it's what Ember will use to reference your
application from here on out, so choose wisely.

### Step 3:

Next we have to create our router, one of the major benefits to Ember is
the router, using this we are able to maintain state easily. Making one is simple enough, but it's where the naming
conventions start to get confusing.
{% highlight js %}
App.Router.map(function(){
});
{% endhighlight %}
That's it, I'm going to make a simple route called "index"
{% highlight js %}
App.Router.map(function(){
  this.resource('index');
});
{% endhighlight %}
Now we have a route at www.example.com/#/index, but we're missing a template to render
it!

### Step 4:

We need somewhere to render our index route, for this we'll need a html file.
In the file we'll make a handlebars script and give it a "data-template-name"
this tells Ember wich route to render to this template.
{% highlight js %}
<script type="text/x-handlebars" data-template-name="index">
<--!content-->
</script>
{% endhighlight %}
That's all it takes! You have an Ember application. Not an exciting one,
but we'll get there.

### Step 5:
I'd like to show how nesting routes works, so I'm going to change our "index" route to
"parent" and nest a "child" route right below it.
{% highlight js %}
App.Router.map(function(){
  this.resource('parent', function(){
    this.resource('child');
  });
});
{% endhighlight %}
This will change our url to be www.example.com/#/parent for the "parent" route and www.example.com/#/parent/child
for the "child" route.

### Step 6:
We probably want our routes to actually show something, rather than just blank pages.
This is extremely easy, simply return an array from our model like so:
{% highlight js %}
App.ChildRoute = Ember.Route.extend({
  model: function() {
    return ['jquery', 'handlebars', 'ember']
  }
});
{% endhighlight %}
Notice that while in the router child is lowercase, in our route we capitalize it and smash it next
to Route, this is how ember knows which route you want to make a model for.
Our model is returning the requirements for Ember, but we need to show them on our child route template,
we're going to loop over the array and show it in a list format.
{% highlight js %}
<script type="text/x-handlebars" data-template-name="child">
  <ul>
  {{#each dependency in model}}
    <li>{{dependency}}</li>
  {{/each}}
  </ul>
</script>
{% endhighlight %}
With that we've got our app returning our dependencies.
Lastly, we need to link to the child route from our parent route. This is fairly simple,
all we have to do is pass the name of our route to a handlebars link-to.

### Step 7:
{% highlight js %}
{% raw %}
<script type="text/x-handlebars" data-template-name="parent">
{{#link-to 'child'}}Show Ember requirements{{/link-to}}
</script>
{% endraw %}
{% endhighlight %}
Our Ember application is now complete. This obviously is a simple example, there are many things
I haven't touched on at all, but this is enough to get anyone started.

#### Now go out there and do something amazing.

###### You can find a working example of this code in this <a href='http://emberjs.jsbin.com/ticoce/1#/parent/'>JSbin</a>.
