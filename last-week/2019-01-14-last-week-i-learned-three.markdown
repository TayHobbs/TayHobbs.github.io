---
layout: post
title:  "Last Week I Learned: Python Dictionary setdefault"
date:   2019-01-14 19:00:00
image: "../../../../../../../images/lwil3.jpg"
categories: Last Week I Learned
---

Python provides many tools to use when working with different collections. The dictionary is obviously one of the basic types to use. Most developers are familiar
with using the `dict.get()` method to do safe key lookups on a dictionary when the key may not exist. I recently ran into a similar situation where I wanted to
set a key if it didn't exist, but I didn't want to override it if it already did. I initially setup my code to look something like:
```
if not data.get(key):
  data[key] = ''
```

This works well, but I discovered another dictionary method called `setdefault()`. It allows you to set a value on a key in a dictionary, but only if that key doesn't
already have a current value, basically exactly what my use case. Using this `setdefault` my previous code example can be refactored to:

```
data.setdefault(key, '')
```

Hopefully this is something that you can find useful in your programs. <a href="https://docs.python.org/3/library/stdtypes.html#dict.setdefault">Here is the official documentation for `dict.setdefault()`</a>.
