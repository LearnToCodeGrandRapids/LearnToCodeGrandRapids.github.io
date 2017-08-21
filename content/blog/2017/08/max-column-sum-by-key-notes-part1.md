+++
date = "2017-08-10T00:00:00-04:00"
draft = false 
title = "Real World Application Development - Part 1 - Notes"
tags = ["c#"]
categories = [ "Languages", "Software Design" ]
featureimage = ""
menu = ""
+++

Here are the notes used for the first segment of the _max column sum by key_ workshop.

<!--more-->

## Create the solution

The first thing that we need to do is create a solution. This solution will contain all the projects we build as part of the workshop.

* In Visual Studio 2017, create a new solution that contains a .NET class library named `Ltcgr.MaxColumnSumByKey`.
* Delete the pregenerated `Class1.cs` file.

## Create the test project

The solution created above gets us started with an Ltcgr.MaxColumnSumByKey project. We now need to create a project to hold our tests.

* Create a new class library project called `Ltcgr.MaxColumnSumByKey.Tests`
* Delete the pregenerated `Class1.cs` file.
* Use nuget to install xunit
* Create a new class called `Tests.cs`
* Create a simple placeholder test

```cs
[Fact]
public void ProcessFile_Returns5000and21_Given1and2forSet1()
{
    Assert.True(true);
}
```

* Run the test to make sure your test runner is functioning correctly (you may need to install `xunit.runner.visualstudio` to run tests from Visual Studio).

* Add a reference to the main `Ltcgr.MaxColumnSumByKey` project

# Create a model for our abstract record

We need to create model to represent the records that we are extracting from the data file. It just needs to contain a key and a value. We'll be able to use this model for records in the file, and the aggregate result of our processing.

* In `Ltcgr.MaxColumnSumByKey` create a folder called `Models`
* In the `Models` folder create a class called `Record`
    * Add the read only property Key (int)
    * Add the read only property Value (int)

```cs
public class Record
{
    public int Key { get; set; }
    public int Value { get; set; }
}
```

## Stubbing out the Processor

The next step is to define the method signature for our entry method that will be used to process data files. 

* Create a class called `Processor`
* Create a method to process files 

```cs
public Record ProcessFile(string filePath, int keyPosition, int valuePosition)
{
    return null;
}
```

* Extract the method into an interface IProcessor
* Add documentation

```cs
/// <summary>
/// Processes the given file by grouping the values associated
/// with each key, summing them, and then returning the key
/// and total for the key with the greatest total.
/// </summary>
/// <param name="filePath">Absolute path to the tab separated file containing the records</param>
/// <param name="keyPosition">The column of the key in the record (first pos is 0)</param>
/// <param name="valuePosition">The column of the value corresponding to the key in the record (first pos is 0)</param>
/// <returns>A record representing the key with the height sum value</returns>
Record ProcessFile(string filePath, int keyPosition, int valuePosition);
```

Note that we are only placing the documentation on the interface. It is possible to put the documentation on the concrete class, but it does not add value to the library. The intent is for implementors to be using the interface, which intellisense will pull documentation from. The lack of documentation on the concrete class is a gentle reminder to implementors to use the interface instead.

## Wire in our test

* Update `ProcessFile_Returns5000and21_Given1and2forSet1` to instantiate an instance of Processor, and test the results

At this point, our test is still failing, so its time to start building the processor.

## How do we read the file?

There is an excellent utility for reading delimiter separated value files called `CsvHelper`. CsvHelper takes a TextReader that is used to read the input. This can be provided by `File.OpenText(absolutePath)`. The problem with this is `File.OpenText` which is a static method. We cannot mock out static methods for our tests. We need to create our own `File` class that wraps the `IO.File` class.

## Create an IO library

* Create a new class library called `Ltcgr.IO`
* Remove the default `Class1.cs` file
* Create a new class called `File`
* Create a wrapper method for `OpenText`

```cs
public StreamReader OpenText(string path)
{
    return System.IO.File.OpenText(path);
}
```

* Extract `OpenText` into an `IFile` interface

```cs
/// <summary>
/// Opens an existing UTF-8 encoded text file for reading (this
/// method wraps System.IO.File.OpenText).
/// </summary>
/// <param name="path">The file to be opened for reading.</param>
/// <returns>A StreamReader on the specified path.</returns>
StreamReader OpenText(string path);
```

# Add a dependency on `IFile` to `Processor`

* Add reference to `Ltcgr.IO` in `Ltcgr.MaxColumnSumByKey` 
* Add an internal prop for the file handler `private IFile File { get; }`
* Add a constructor the takes an IFile instance as a parameter

```cs
public Processor(IFile file)
{
    File = file;
}
```

# Updating our test to inject IFile

* Install `AutofacContrib.NSubstitute` (Autosubstitute) in your test project
* Add reference to `Ltcgr.IO` in `Ltcgr.MaxColumnSumByKey.Tests` 
* Add `set1.tsv` as a resource to the test project ([download here](https://github.com/LearnToCodeGrandRapids/max-column-sum-by-key-part1/blob/master/data/set1.tsv))
* Update the test to substitute an IFile that returns a stream based on set1

```cs
public void ProcessFile_Returns5000and21_Given1and2forSet1()
{
    const int expectedKey = 5000;
    const int expectedTotal = 21;

    var autoSubstitute = new AutoSubstitute();

    autoSubstitute
        .Resolve<IFile>()
        .OpenText(string.Empty)
        .Returns(new StreamReader(new MemoryStream(Resources.set1)));

    var (actualKey, actualTotal) = autoSubstitute
        .Resolve<Processor>()
        .ProcessFile(string.Empty, 1, 2);
    
    Assert.Equal(expectedKey, actualKey);
    Assert.Equal(expectedTotal, actualTotal);
}
```

Our test still fails, but we're now ready to start building the process file logic.

In next month's workshop we will begin implementing the logic for our processor.

You can download the solution from the first workshop from the repository on github: [max-column-sum-by-key-part1](https://github.com/LearnToCodeGrandRapids/max-column-sum-by-key-part1).