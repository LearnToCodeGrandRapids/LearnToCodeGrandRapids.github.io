+++
date = "2018-02-09T00:00:00-04:00"
draft = false 
title = "File I/O and Exceptions Meetup Notes"
tags = ["c#"]
categories = [ "Languages", "Software Design" ]
featureimage = ""
menu = ""
+++

In our discussion of File I/O and Exceptions in C# sharp there were three major take aways:

* Exception handling should only be used when necessary, because it is not free.
* When dealing with File I/O use the static methods of File and Directory when possible.
* Build your own I/O class that wraps the framework classes so that you can easily test for exceptions.

## Exception Handling

Exceptions, and exception handling adds overhead to your applications since they span functions by their nature. Keeping track of this is not free. We encourage you to consider designing your applications to only use exception handling when necessary, and it will be necessary in some circumstances because exceptional cases exist that will need to be accounted for. When possible, you should design your application to account for error situations without introducing a state that may result in an exception. Furthermore, if you are using C# 7, you can use multiple return values to propagate error information in your functions so that throwing an exception is not necessary.

## File and Directory Helper Methods

Its important to know how to work with streams, and handling I/O with primatives. It is not important to use those techniques all the time. Most application development is not subject to critical performance constaints, so using the built-in helper methods provided by the .NET framework will help you safe time, and prevent you from accidentaly introducing errors by reimplementing the same basic routines over and over again.

## Build a Wrapper for Your I/O Methods

When you are dealing with I/O, you are going to be dealing with exceptions. Sometimes these exceptions can be very difficult to reproduce, and test for. A trick that we like to use is to create a class the wraps the I/O routines. By creating a wrapper class you can create an interface that defines a common API that you will use in your program. When you're writing tests to ensure you can handle typical I/O exceptions, you can create a mock class that implements the interface. This mock class can be used to force exceptions to be thrown in a reliable, reproducible way so that it can be tested easily.

## Resources

During the meetup, we only touched the surface of file I/O and exceptions. If you would like to learn more, I recommend the following resources:

* [Book - C# 6.0 in a Nutshell: The Definitive Reference](https://www.amazon.com/C-6-0-Nutshell-Definitive-Reference/dp/1491927062/)
* [Book - C# 6.0 and the .NET 4.6 Framework](https://www.amazon.com/C-6-0-NET-4-6-Framework/dp/1484213335/)
* [Video - C# For Absolute Beginners](https://mva.microsoft.com/en-us/training-courses/c-fundamentals-for-absolute-beginners-16169)
