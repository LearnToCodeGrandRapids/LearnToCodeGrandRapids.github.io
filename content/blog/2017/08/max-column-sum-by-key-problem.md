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

## Stubbing out the Processor

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

## Wire in our test

* Update `ProcessFile_Returns5000and21_Given1and2forSet1` to instantiate an instance of Processor, and test the results

At this point, our test is still failing, so its time to start building the processor.

## How do we read the file?

There is an excellent utility for reading delimiter separated value files called `CsvHelper`. CsvHelper takes a TextReader that is used to read the input. This can be provided by `File.OpenText(absolutePath)`. The problem with this is that `File.OpenText` is that its a static method, which we cannot mock out for our tests. We are going to create our own `File` class that wraps the `IO.File` class.

## Create an IO library

* Create a new class library called `Ltcgr.IO`
* Remove the default `Class1.cs` file
* Create a new class called `File`
* Create a wrapper method for `OpenText`

```cs
        /// <summary>
        /// Opens an existing UTF-8 encoded text file for reading (this
        /// method wraps System.IO.File.OpenText).
        /// </summary>
        /// <param name="path">The file to be opened for reading.</param>
        /// <returns>A StreamReader on the specified path.</returns>
        public StreamReader OpenText(string path)
        {
            return System.IO.File.OpenText(path);
        }
```

* Extract `OpenText` into an `IFile` interface

# Add a dependency to `IFile` for `Processor`

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
* Add `set1.tsv` as a resource to the test project
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

# Create a model for our abstract record

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

# CSV Helper

* Install `CsvHelper` in `Ltcgr.MaxColumnSumByKey` and `Ltcgr.MaxColumnSumByKey.Tests`
* Things get a little tricky here. Normally we would create a static map for the `Result` class, but our requirements allow the implementor to define the field positions at runtime. Therefore, we need to build our map for CsvHelper dynamically. 
* First, let's write a test for the new method:

```cs
        [Fact]
        public void CreateRecordMap_ReturnsPropMapsForKeyAndValue_WhenCalled()
        {
            var autoSubstitute = new AutoSubstitute();

            autoSubstitute
                .Resolve<IFile>();

            var actual = autoSubstitute
                .Resolve<Processor>()
                .CreateRecordMap(1, 2);

            Assert.Equal(actual.PropertyMaps[0].Data.Index, 1); // Key Pos
            Assert.Equal(actual.PropertyMaps[1].Data.Index, 2); // Value Pos
        }
```

* Now we can write the method

```cs
        public DefaultCsvClassMap<Record> CreateRecordMap(int keyPosition, int valuePosition)
        {
            var map = new DefaultCsvClassMap<Record>();

            var keyProperty = typeof(Record).GetProperty(nameof(Record.Key));
            var valueProperty = typeof(Record).GetProperty(nameof(Record.Value));

            var keyMap = new CsvPropertyMap(keyProperty);
            keyMap.Index(keyPosition);

            var valueMap = new CsvPropertyMap(valueProperty);
            valueMap.Index(valuePosition);

            map.PropertyMaps.Add(keyMap);
            map.PropertyMaps.Add(valueMap);

            return map;
        }
```

# Implement the ProcessFile method

* at long last we can implement the `ProcessFile` method
* first we need to create a variable to hold the records we read in from the file
* generate the class map for Result
* create a csvreader instance, and configure it to use tab as the delimiter, and register our class map
* read the records in from the file

* group the records by key
* calculate the sum of values for each key
* find the key with the greatest summation total
* return the resulting key and total

```cs
        public Record ProcessFile(string filePath, int keyPosition, int valuePosition)
        {
            List<Record> records;

            var map = CreateRecordMap(keyPosition, valuePosition);
            
            using (var reader = File.OpenText(filePath))
            {
                var csv = new CsvReader(reader);
                csv.Configuration.Delimiter = "\t";
                csv.Configuration.RegisterClassMap(map);
                records = csv.GetRecords<Record>().ToList();
            }

            var result = records
                .GroupBy(r => r.Key)
                .Select(rg => new Record { Key = rg.Key, Value = rg.Sum(x => x.Value) })
                .OrderByDescending(x => x.Value)
                .First();

            return result;
        }
```

# Let's check our performance

* The first data set is pretty small, let's make sure we can calculate the aggregate in less than two seconds

```cs
        [Fact]
        public void ProcessFile_ReturnsInLessThan2Sec_Given1and2forSet1()
        {
            var timer = new Stopwatch();
            timer.Start();

            ProcessFile_Returns5000and21_Given1and2forSet1();

            timer.Stop();

            Console.WriteLine(timer.ElapsedMilliseconds);

            Assert.True(timer.ElapsedMilliseconds < 2000);
        }
```

# Let's load the real test file

* Add the n-gram file as set2.tsv in your test project
* Refactor our the ProceFile test we wrote earlier into a Theory so we can test with both datasets

```cs
        [Theory]
        [InlineData("set1", 1, 2, 5000, 21)]
        public void ProcessFile_ReturnsExpected_GivenInputs(
            string set, int keyPosition, int valuePosition, int expectedKey, int expectedTotal)
        {
            var autoSubstitute = new AutoSubstitute();

            var setBytes = (set == "set2") ? Resources.set2 : Resources.set1;

            autoSubstitute
                .Resolve<IFile>()
                .OpenText(string.Empty)
                .Returns(new StreamReader(new MemoryStream(setBytes)));

            var actual = autoSubstitute
                .Resolve<Processor>()
                .ProcessFile(string.Empty, keyPosition, valuePosition);
            
            Assert.Equal(expectedKey, actual.Key);
            Assert.Equal(expectedTotal, actual.Value);
        }
```

* Now we can write another profiling test for set2

```cs
 [Fact]
        public void ProcessFile_ReturnsInLessThan30Sec_Given1and2forSet2()
        {
            var timer = new Stopwatch();
            timer.Start();

            ProcessFile_ReturnsExpected_GivenInputs("set2", 1, 2, 2006, 22569013);

            timer.Stop();

            Console.WriteLine(timer.ElapsedMilliseconds);

            Assert.True(timer.ElapsedMilliseconds < 30000);
        }
```

* You may need to tweak this test depending on the performance of the machine you are running on

# Profiling

At this point, the library satisfies our initial problem. However, it is pretty slow. Let's do a little investigative work and see if we can figure out why.

* Install dotTrace
* https://www.jetbrains.com/help/profiler/Get_Started_with_Performance_Viewer.html
* ReSharper | Profile | Profile Startup Configuration Performance Profiling....
* Select Sampling for a .net process
* Run the unit test
* View the results
* Looks like CsvHelper is out bottleneck

# PART 2

* https://stackoverflow.com/questions/2161895/reading-large-text-files-with-streams-in-c-sharp

