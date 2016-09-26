---
layout: post
title:  "Ember-CLI Addon"
date:   2015-01-24 20:51:32
image: "../../../../../../../images/addon.jpeg"
categories: ember
---

Ember-CLI is coming along to be a great tool to manage your Ember projects. It takes care of all the scaffolding for you so you can get right
to work.

If you are unfamiliar with Ember-CLI their [website](http://ember-cli.com/) website has a good tutorial, and links to examples
to get you started.

## Getting Ember-CLI and Starting the Addon
We're going to create an addon that has a component that has a property and renders the property in the template.

First item of business is making sure you have Ember-CLI, the recommended course of action is to have it installed globally.

From your command line run:
{% highlight javascript %}
npm install -g ember-cli
{% endhighlight %}

After you have Ember-CLI run:
{% highlight javascript %}
ember addon my-addon
{% endhighlight %}

This will create both an addon and an app directory, the logic for our component will live inside addon/components, but the template
lives in app/templates/components. Another confusing problem that I didn't understand at first is inside our app/components directory
we will import our component from our addon directory and immediately export. This is entirely for namespacing.


## Create the Component
In Ember components must have a dash in the name, so our component will be called "my-component".
Create the .js file inside addon/components/ and make it look like this:

{% highlight javascript %}
import Ember from 'ember';

export default Ember.Component.extend({
  myComponentProperty: function() {
    return "I'm a component!";
  }.property()
});
{% endhighlight %}


And inside app/templates/components/my-component.hbs simply write:
{% highlight javascript %}
{{myComponentProperty}}
{% endhighlight %}

Now inside app/templates/components/my-component.js it's time to do the import/export dance like so:
{% highlight javascript %}
import MyComponent from 'adtest/components/my-component';

export default MyComponent;
{% endhighlight %}

That's the extent of making an Ember-CLI addon. Now all we have to do is npm install our addon and it is available inside our app.


## Changing the Component Inside Our App
If you need to change your component inside your app, all that we need to do is import inside our app's component directory.
So if we wanted to change myComponentProperty, in app/components/my-component.js:
{% highlight javascript %}
import MyComponent from 'adtest/components/my-component';

MyComponent.extend({
  myComponentProperty: function() {
    return "I'm an edited component!";
  }.property()
});

export default MyComponent;
{% endhighlight %}

And our component will be changed!


## Index.js
A lot of the power for Ember-CLI addons comes from the index.js file, there are a lot of different hooks to add data to the parent app.
Here are some of the ones I've found useful,

### contentFor
Use contentFor if you want to insert tags into the head, body or footer of your index.html page.
{% highlight javascript %}
module.exports = {
  name: 'my-addon',
  contentFor: function(type, config) {
    var content = []
    if ( type == 'head' ) {
      content.push(//insert your content for the head here);
    }
    if ( type == 'body' ) {
      content.push(//insert your content for the body here);
    }
    if ( type == 'footer' ) {
      content.push(//insert your content for the footer here);
    }
    return content.join('\n');
  }
}
{% endhighlight %}

### treeForPublic
treeForPublic is for adding public files, such as images, to the parent app.
Create an assets directory, inside the public directory and add an image directory inside that.

{% highlight javascript %}
var path = require('path');
module.exports = {
  name: 'my-addon',
  treeForPublic: function(tree) {
    var publicDir = path.join(this.project.root, 'public');
    var publicFiles = this.pickFiles(this.treeGenerator(publicDir), {
      srcDir: '/',
      destDir: '/'
    });
    if (tree) {
      return this.mergeTrees([tree, publicFiles], {overwrite: true});
    }
    else {
      return publicFiles;
    }
  }
}
{% endhighlight %}

### Injecting Dependencies
If you want to inject some dependencies into the parent app, it's very simple, but the parent app has to have the dependencies bower
installed. A good way to help this is through a blueprint.
Create a directory called blueprints, then make one named after our addon, "my-addon", and create and index.js file inside there that looks
like this:
{% highlight javascript %}
module.exports = {
  normalizeEntityName: function() {},

  afterInstall: function() {
    return this.addBowerPackageToProject('dependency');
  }
}
{% endhighlight %}
Then you have to run `ember g my-addon` and the dependency will be install and added the app's bower.json.

To have the dependency included in the parent's app tree, inside the index.js:
{% highlight javascript %}
module.exports = {
  name: 'my-addon',
  included: function(app) {
    app.import('path/to/dependency');
  }
}
{% endhighlight %}

### Config
One of my favorite additions is the config hook, it allows you to insert your own configuration into the parent app's ENV.
What you define in the addon will be what is added the the parent app, it can act as a default, for you to override.

{% highlight javascript %}
module.exports = {
  name: 'my-addon',
  config: function() {
    var ENV = {
      customConfigObject: {
        'configuration': 'default_config'
      }
    }
    return ENV;
  }
}
{% endhighlight %}


##Finishing Up

With that you now have the tools to create your own Ember-CLI addon. The final step is for you to publish it.
Up the version inside package.json and then run `npm init` and `npm publish`.
With that your addon is complete and out there for the world to use!
