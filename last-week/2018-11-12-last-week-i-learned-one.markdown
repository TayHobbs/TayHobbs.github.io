---
layout: post
title:  "Last Week I Learned"
date:   2018-11-12 19:00:00
image: "../../../../../../../images/last-week-i-learned.jpg"
categories: Last Week I Learned
---

Starting this week, I would like to start blogging weekly about something that I've learned, programming related. Admittedly, this is far from an original
idea. Today I Learned is a popular post concept, but I think that this will challenge me to look for areas to stretch myself. It will force me to looke for more things
to learn and expose myself to, as well as to blog more frequently. I chose weekly over daily because given my history with blogging, daily is simply
too much to reasonably be expected.

These will be shorter than a more "typical" blog post most of the time. The idea is to have it be just a short snippet about something I learned. Some may be larger than
others, depending on what kind of week I had. To start us off, here is what I learned last week:


#Last Week I Learned: How to Tie a Django Model To Itself With a Many-to-Many


Last week at my place of work I was presented with a challenge: A model that can contain many of itself in an asymmetrical way. Django definitely supports the
ability to tie a model to itself with M2M and Foreign Keys, but it defaults to being symmetrical. What this means, as per the Django documentation:
<a href="https://docs.djangoproject.com/en/dev/ref/models/fields/#django.db.models.ManyToManyField.symmetrical">"...if I am your friend, then you are my friend."</a>

This is a reasonable assumption when a model, such as Person, is tying to itself to represent a "friends" relationship:

{% highlight python %}
class Person(models.Model):
    friends = models.ManyToManyField('self')
{% endhighlight %}


However, this is not desirable when the relationship should better represent a parent/child relationship: if you are my parent, I am not yours, but rather your child.

{% highlight python %}
class Person(models.Model):
    children = models.ManyToManyField('self', symmetrical=False, related_name='parents')
{% endhighlight %}

Now, both parents and children relationships can be added to the model, and they will not be symmetrical.
