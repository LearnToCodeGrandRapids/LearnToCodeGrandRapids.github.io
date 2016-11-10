+++
date = "2016-11-01T00:00:00-04:00"
draft = true
title = "Getting Started with C# - Part 2"
tags = ["c#"]
categories = [ "Languages" ]
featureimage = ""
menu = ""
+++

Below are my notes for the November 2016 meetup where we go over the basics of C# and work through some sample problems. We highly recommend that you [watch the video series](https://channel9.msdn.com/Series/C-Fundamentals-for-Absolute-Beginners/) associated with collections and data structures since it is far more complete.

<!--more-->

## Programming Problems

We begin with some basic programming problems that allow us to use the skills we learned in our last session.

* Write a program that asks the user for her name and greets her with her name.

* Write a program that prints the next 20 leap years.

* Write a program that asks the user for a number n and prints the sum of the numbers 1 to n

## Collections

Thus far we have been using primatives, and strings capable of storing a single value. It is frequently useful to work with a collection of values as you develop programs. One of the simpliest, and most widely used collection type is the array.

### Arrays 

Arrays in C# have the following attributes:

* All elements in the array must be the same type
* All elements in the array are stored within a contiguous block of memory
* Individual elements of the array and be retrieved directly

In our previous meeting we discussed how variables are essentially buckets that hold values in the computer's memory. Each of these buckets has a specific size based on the type held within. 

```cs
var x = 5;
```

In the diagram below, we see a section of memory in chunks of the size of an int. 

<table style="text-align: center;">
    <tr>
        <td>0</td>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
        <td>5</td>
        <td>6</td>
        <td>7</td>
    </tr>
    <tr>
        <td style="background: #cfc">7</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
    </tr>
    <tr>
        <td style="background: #cff">x</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
    </tr>
</table>

Let's say we have a situation where you want to keep track of how many times a day you think about the weekend during the week. For this, we would need to create an array that can hold five values, one for each week day.

```cs
var weekdays = new int[5];
```

Now we have a chunk of memory that looks something like this:

<table style="text-align: center;">
    <tr>
        <td>0</td>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
        <td>5</td>
        <td>6</td>
        <td>7</td>
    </tr>
    <tr>
        <td style="background: #cfc">0</td>
        <td style="background: #cfc">0</td>
        <td style="background: #cfc">0</td>
        <td style="background: #cfc">0</td>
        <td style="background: #cfc">0</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
    </tr>
    <tr>
        <td style="background: #cff" colspan="5">weekdays</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
    </tr>
</table>

Elements in an array can be accessed, or assigned using an indexed defined by its position relative to the beginning of the array. Let's say that you start thinking about the weekend on Wednesday afternoon. We can access this value at `weekday[2]`. The index of Wednesday is 2, even though it is the third day of this week. That is because it is offset by two positions from the beginning of the array. Let's set the value to 1.

```cs
weekday[2] = 1;
```

Now our block of memory looks something like this:

<table style="text-align: center;">
    <tr>
        <td>0</td>
        <td>1</td>
        <td>2</td>
        <td>3</td>
        <td>4</td>
        <td>5</td>
        <td>6</td>
        <td>7</td>
    </tr>
    <tr>
        <td style="background: #cfc">0</td>
        <td style="background: #cfc">0</td>
        <td style="background: #cfc">1</td>
        <td style="background: #cfc">0</td>
        <td style="background: #cfc">0</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
        <td style="background: #ccc">&nbsp;</td>
    </tr>
    <tr>
        <td style="background: #cff" colspan="5">weekdays</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
        <td>&nbsp;</td>
    </tr>
</table>

Now's lets say that you want to add Sunday to the list, because that is the day that you start planning your activities for the following weekend. Unfortunately, arrays are fixed in size once you create them. If you want to expand an array, you need to create a new array, and copy the values from your old array. It is a lot to keep track of, right? Well, luckily the foundation library comes with some collections that make our lives easier. In this case, the list.

### Lists

Lists allow you to work with a collection of values like an array, but you do not have to worry about any of the allocation issues. Lets create our list of weekdays.

```cs
var weekdays = new List<int>();
```

This code looks similiar to how we defined our array earlier, but we do not specific a specific number of elements contained within. This does require us to add each of the days to the list ourselves.

```cs
weekdays.Add(0); // monday
weekdays.Add(0); // tuesday
weekdays.Add(0); // wendesday
weekdays.Add(0); // thursday
weekdays.Add(0); // friday
```

We can still assign values in the list like we did in the array. To set Wednesday to 1 again, we simply:

```cs
weekday[2] = 1;
```

Now, if we decide to add in the weekdays to account for Sunday's planning sessions we can without having to worry about how memory is managed behind the scenes.

```cs
weekdays.Add(0); // saturday
weekdays.Add(2); // sunday
```

After we have added the weekend, we can still retrieve the value we set for Wednesday. 

```cs
Console.WriteLine(weekdays[2]);
```

```
1
```

Let's take what we have learned and apply it to some more programming problems:

* Write a function that returns the largest element in a list.

* Write a function that concatenates two lists.

* Write a function that takes an integer and returns a list of its digits.

* Write a function that takes a list of strings an prints them, one per line, in a rectangular frame. For example the list `["Hello", "World", "in", "a", "frame"]` gets printed as:

```
*********
* Hello *
* World *
* in    *
* a     *
* frame *
*********
```

### Extra Credit

Research about the collection type, [Dictionary](https://msdn.microsoft.com/en-us/library/xfhwa508(v=vs.110).aspx) and write a program that automatically converts English text to Morse code.

## References

For more on collections you can read the [Collections and Data Structures](https://msdn.microsoft.com/en-us/library/7y3x785f(v=vs.110).aspx) section of the MSDN.

Many of the programming problems presented here came from [Simple Programming Problems](https://adriann.github.io/programming_problems.html). This is a good resource for programming problems for beginners. 