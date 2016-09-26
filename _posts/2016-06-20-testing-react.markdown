---
layout: post
title:  "Testing React"
date:   2016-06-20 20:51:32
image: "../../../../../../../images/testing-react.jpeg"
categories: react programming
---

## Testing in React

When I use a Javascript framework, I primarily use Ember. Ember was my first Javascript framework and I am very experienced with it and its idioms. For the most part, I have enjoyed
working with Ember and I am able to build apps quickly with it. However, I also enjoy seeing what other frameworks have to offer and I see a lot of people talking about React. So I
decided to build an application using React. Something that is very important to me when developing any app, in any framework, is the ability to test it quickly and easily. With Ember,
Ember-CLI makes it extremely easy to get up and running with tests. [There is a section](https://facebook.github.io/react/docs/test-utils.html) on testing in the React documentation, so I thought that testing may be a quick and simple
process like it is in Ember. It was not quite as straight forward as I had hoped.

### Using Jest

The `ReactTestUtils` makes it extremely simple to use whatever testing framework you prefer. The docs state that [Jest](https://facebook.github.io/jest/) is what they use, so naturally I tried this first.
At first Jest was handling all of my issues flawlessly, but after I started writing more and more components, I realized this was because I was only using examples from the initial
Jest tutorial page. After I started getting more complicated, I found Jest to be less intuitive than I had initially thought. I was getting frustrated, spending more time trying
to figure out how to test my code than actually writing any real production code. I started looking for alternatives, which is when I found [Enzyme](http://airbnb.io/enzyme/) by [airbnb](https://www.airbnb.com/).

### Enzyme

React actually lists Enzyme as an alternative in their tutorial. I decided to give it a shot. At first I was unsure how to go about replacing Jest with Enzyme, but then I found
a very helpful [tutorial](https://semaphoreci.com/community/tutorials/testing-react-components-with-enzyme-and-mocha) about how to do exactly that. Using this tutorial as my guide I was actually able
to switch out the testing framework in about an hour. I enjoy using Enzyme a lot more than using Jest. I find it to be much more intuitive to me. Now I can spend my time actually writing
real code and using my testing suite to support it, rather than spending my time fighting the testing framework.

After a couple weeks of using React, I have really been enjoying it. One thing I would love to see is some sort of command line interface or build tooling, like Ember-CLI.
I look forward to using React in more future projects and watching the ecosystem grow. I think there are some really interesting problems being solved in the React atmosphere.
