---
layout: post
title:  "Python Package"
date:   2014-09-14 20:51:32
image: "../../../../../../../images/python-package.jpeg"
categories: python
---

I have recently been spending time working on different Python packages for work and found that it is fairly simple, but doesn't seem to be
well documented. Here I'll try to explain some of the different things that tripped me up while I was writing my packages.

Important to keep in mind is that what I do here is not necessarily convention, the only convention is a setup.py file, which you'll see soon.

Firstly, PyPi - the Python Package Index - is where your Package will be hosted. You'll have to create an account there first.
After making an account, you'll have to create a .pypirc in your home directory structured like this:
{% highlight python %}
[distutils]
index-servers =
    pypi

[pypi]
username:your-username
password:your-password
repository:http://pypi.python.org/pypi
{% endhighlight %}

With that you're ready.

### Decide What To Make
We're going to make a very simple example package called "Hello", that will take a name as a flag, returning "Hello, {name}"

### Make Your Folder Structure
I like to make a folder, with the name of my project, then at the same level of that folder, have my setup.py file,
a README.rst and a MANIFEST.in.

{% highlight python %}
|--hello/
|--MANIFEST.in
|--README.rst
|--setup.py
{% endhighlight %}

Inside the 'hello/' folder, I like to do all of my logic in a \_\_init\_\_.py file, and then call it in a scripts.py file.
{% highlight python %}
|--hello/
  |--__init__.py
  |--scripts.py
{% endhighlight %}


### Write Your Package
Inside the \_\_init\_\_.py we'll start our code. It's going to be a very simple method that takes a parameter and returns
a string with that parameter at the end.
{% highlight python %}
def hello(name):
  print "Hello {}".format(name)
{% endhighlight %}

Then in our scripts.py file we have to import our method and call it within our "main" method.
{% highlight python %}
from hello import hello

def main():
  hello(name)
{% endhighlight %}

Now obviously we need a name to pass to hello. To do that we'll import argparse and use it to define what kind of flag we want.
{% highlight python %}
import argparse

parser = argparse.ArgumentParser()
parser.add_argument("-n", "--name", type=str, required=True, help="Your name")
...
{% endhighlight %}
Then add our argument to the "main" method
{% highlight python %}
...
def main():
    args = parser.parse_args()
    return hello(args.name)
{% endhighlight %}
Lastly we need to make a fill our setup.py file with the following:
{% highlight python %}
import multiprocessing, logging
from setuptools import setup, find_packages

setup(
        name='hello',
        version='0.0.1',
        author="Your Name",
        author_email="your@email.com",
        description="Tell your friends hello!",
        long_description=open('README.rst').read(),
        url='where-your-source-lives.com',
        install_requires=[''],
        entry_points={
            'console_scripts': {
                "hello = hello.scripts:main",
                },
            },
        )
{% endhighlight %}
Everything here is fairly straight forward, but the "install_requires" field is for any dependencies that your package has.
Simply pass it the name of a package like you're trying to pip install it, and it'll be installed when your package is installed.


With that we're ready to build the plugin. In the main directory alongside setup.py run "python setup.py sdist" and that will
build the package. To upload your package run "python setup.py sdist upload -r pypi" and if there are no conflicts with the
package name it'll be added to PyPi.

And that's it! You've now built and distributed your first Python package. Go make the Pythom world a better place.


### Explaining The MANIFEST.in

Something that confused me was the requirement of the MANIFEST.in file, I just wanted to include a README for my github
repo and let that serve as my "long_description". This is actually very trivial, but is a little confusing your first time around.
In the MANIFEST.in file, simple write a line that says:
{% highlight ruby %}
include README.rst *
{% endhighlight %}
That's all it takes.

### Installing Your Package Before Upload

Another thing I find handy is testing my package before I upload it. This tripped me up quite a bit my first time,
so I thought I'd include it here.
To install your package from your machine, you have to run the "sdist" command, and this will create a "dist" folder in the directory
with a "tar.gz" file. That is the file that you are wanting. Just pip install with the path to that file instead of the package.
{% highlight ruby %}
pip install ~/PathToPackage/hello/dist/hello-0.0.1.tar.gz
{% endhighlight %}
Now you can test your package just like you had installed it from PyPi.
