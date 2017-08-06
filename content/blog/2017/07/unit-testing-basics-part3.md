+++
date = "2017-07-13T00:00:00-04:00"
draft = false
title = "Unit Testing Basics with xUnit and VS 2017 - Part 3 - Notes"
tags = ["c#"]
categories = [ "Languages", "Testing" ]
featureimage = ""
menu = ""
+++

In this month's meetup we started working through the second [string calculator](http://osherove.com/tdd-kata-2/) kata. We completed each of the exercises. 

You can download the solution from our github repo [string-calculator-part3](https://github.com/LearnToCodeGrandRapids/string-calculator-part3). Just follow the link, click on "clone or download," and then click "download zip."

## Additional Resources

We talked a little bit about the concept of dependency injection, and inversion of control in this workshop. Our preferred IoC container is [Autofac](https://autofac.org/).

In the context of testing we discuss mocks, and built our own. We illuded to tools that make this process even easier. Our favorite mocking framework is [NSubstitute](http://nsubstitute.github.io/). When using a mocking framework along side an IoC container is it helpful to use a library that will handle resolving your dependencies. We like to use [AutoSubstitute](https://github.com/MRCollective/AutofacContrib.NSubstitute) when working with Autofac and NSubstitute.