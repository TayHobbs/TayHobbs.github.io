---
layout: post
title:  "Last Week I Learned: Installing Node Modules from Github"
date:   2018-11-12 19:00:00
image: "../../../../../../../images/lwil2.jpg"
categories: Last Week I Learned
---

Needing to create and install private node modules is a situation that I figure many indiviuals find themselves in, especially if they work in medium sized companies.
There is the option to have a private <a href="https://www.npmjs.com/pricing">npm</a>, but if you only have a couple modules, and already use Github, it doesn't make a
lot of sense to pay for this service.

You can install from github using by including the ssh link to github followed by a branch or tag name, `git+ssh://git@github.com:npm/cli#5.0`. I think this is a pretty
typical use case that many are familiar with, but what I learned last week was that you can include ranges in when installing from Github. Taking the same link from above,
changing it to `git+ssh://git@github.com:npm/cli#semver:^5` will allow it to get the latest version of the `5` releases. This is extremely handy when you release a bug fix.
You no longer have to go into every project that installs from github and manually bump the version. They will be retrieved with the next release.

You can read more about this feature in the <a href="https://docs.npmjs.com/cli/install">NPM docs</a>.
