+++
date = "2017-08-10T00:00:00-04:00"
draft = true 
title = "Real World Application Developer - Part 1 - Notes"
tags = ["c#"]
categories = [ "Languages", "Software Design" ]
featureimage = ""
menu = ""
+++

Here are the notes used for the second segment of the _max column sum by key_ workshop.

<!--more-->

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

