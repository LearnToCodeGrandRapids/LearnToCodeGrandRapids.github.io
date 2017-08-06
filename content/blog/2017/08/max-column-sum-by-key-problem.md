+++
date = "2017-08-10T00:00:00-04:00"
draft = true 
title = "Real World Application Developer - Part 1 - Notes"
tags = ["c#"]
categories = [ "Languages", "Software Design" ]
featureimage = ""
menu = ""
+++

Here are the notes used for the first segment of the _max column sum by key_ workshop.

<!--more-->

## Create the solution

* In Visual Studio 2017, create a new solution that contains a .NET class library named `Ltcgr.MaxColumnSumByKey`.
* Delete the pregenerated `Class1.cs` file.

## Create the test project

* Create a new xunit test project called `Ltcgr.MaxColumnSumByKey.Tests`
* Delete the pregenerated `Class1.cs` file.
* Create a new class called `Tests.cs`
* Create a simple placeholder test

```cs
        [Fact]
        public void Calculate_Returns2006and22569013_Given1and2()
        {
            Assert.True(true);
        }
```

* Run the test to make sure your test runner is functioning correctly

* Add a reference to the main `Ltcgr.MaxColumnSumByKey` project

## Stubbing out the Processor

* Create a class called `Processor`
