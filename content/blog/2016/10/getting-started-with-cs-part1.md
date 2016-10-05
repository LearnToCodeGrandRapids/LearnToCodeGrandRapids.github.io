+++
date = "2016-10-01T00:00:00-04:00"
draft = true
title = "Getting Started with C# - Part 1"
tags = ["c#"]
categories = [ "Languages" ]
featureimage = ""
menu = ""
+++

Here are the notes from our October 2016 workshop.

## Setting up your Development Environment

For this workshop, we will be using Xamarin Studio (or MonoDevelop if you are using linux). The first thing we will do is get the environment setup.

* For macOS you can use brew: `brew cask install xamarin-studio` ([click here](http://macappstore.org/xamarin-studio/) for detailed directions).

* For Windows, use can [download the universal installer](https://www.xamarin.com/download).
    * NOTE: If you are using the offline installers for Windows rather than the installer listed above, you need to install GTK# for .NET, and MSBuild Tools before installing Xamarin Studio. 

* For linux, your distribution probably already has a package for monodevelop, but if it doesn't you can [download the source here](http://www.monodevelop.com/download/)

## Your First C# Application

Open Xamarin Studio

Click on File -> New Solution 

![New solution screenshot](/blog/2016/10/new-solution.png)

In the dialog, select .NET, and then "Console Project." Click, "Next."

![New Console Project](/blog/2016/10/console-project.png)

In the Project Name box, enter HelloWorld, and then click, "Create."

![New Console Project](/blog/2016/10/hello-world.png)

After a moment you should be taken to the Program.cs file within your new project. It should look something like this:

```cs
using System;

namespace HelloWorld
{
	class MainClass
	{
		public static void Main(string[] args)
		{
			Console.WriteLine("Hello World!");
		}
	}
}
```

At this point you could run the application. It would print "Hello World!" to a console window and then immeidately exit. It is difficult to see if your programming is generating the correct output when it just blinks on the screent. We are going to add a line to the program to make it wait for keyboard input before closing so that we can visually inspect the output.

After the `Console.WriteLine("Hello World!");` line, add a new line, and then add the following: `Console.ReadLine();` this will cause the program to pause after printing Hellow World! until you pretty return. 

```cs
		public static void Main(string[] args)
		{
			Console.WriteLine("Hello World!");
			Console.ReadLine();
		}
```

Now we can run the application. Be sure you have saved your change, and then click on the play button to execute the program.

![Play button](/blog/2016/10/play.png)

You should now see a console window with the worlds "Hello World!" printed on screen.

![Console hello](/blog/2016/10/console-hello.png)

## Breaking Down Our First Program

The .NET framework has two fundamental components that make your life as a developer much easier.

* Class Library - The set of prewritten libraries that take care of routine development activities so that you do not have to. This includes such routines as handling inputs and outputs for your program. 

* Runtime - The protective bubble that your application lives inside. The runtime handles low level concerns so that you can focus on your application rather than being concerned by how the computer is management memory, etc.

In the program that we just wrote, we have already taken advance of the class lirbary. The command `Console.WriteLine("Hello World!");` handles writing the text, "Hellow World!" to the console. All we have to be concerned about is what we want printed on the screen. The Console library takes care of everything necessary to take that information and disable is in the console window for us.

### Code Blocks

You may have noticed that the application has several sets of curly braces. Each of these sets of curly braces represents a block of code. Each code block represents a specific scope for the code that exists within it. We will talk about the implications of this more later on, for now lets focus on the inner most set of curly braces. 

This code block is named, Main. The name is what is used to invoke a particular code block. This code block is a method, since it is defined within the scope of a class. In this case the class is, Program. This of the Main method in the Program class is that it is executed when the program is runs.

Let's take a look at the class, Program. You can think of a class as an organizational structure. It is a way to group together common data, and functionality. 

The outer most set of curly braces is the namespace, HelloWorld. Namespaces are another way of organizing your code. Namespaces represent a collection of related classes. 

### Inside Main

Revisiting the command `Console.WriteLine("Hello World!");` we can note think about this in the context established earlier. We see the method WriteLine on the class Console. The next line references another method named ReadLine, also within the Console class. Since both of these methods deal with the console it makes sense for them to be in a common class named, Console, right? 

Both of these methods were written by someone at Microsoft as part of the class library. We benefit from their work by simply invoking methods they wrote using the syntax above. 

In `Console.WriteLine("Hello World!");` notice that there is a dot between the class Console and the method WriteLine. The dot is called a member accessor. This tells the program that you want to access something on the class, in this case, the method WriteLine.

The parentheses indicate the input we would like the method to use during execution. In this case, we are passing in the string literal, "Hello World!" This means we are explicitly defining the value to be displayed on screen by the method. Later on we will talk about variables and have they can be used as input as well. Notice that the ReadLine method has parentheses, but there is nothing between them. In this case, we are invoking the method, but are not passing in any values.

The semicolon at the end of the line indicates the end of a statement. A statement is an single action to be taken during program execution. We have a line between the two method calls for readability. However, since both statements end in a semicolon the line break is not necssary for the program to function. In fact, the entire program could be writen on a single line. I wouldn't recommend doing that both for your own sanity and for the sanity of anyone having to maintain your code.

Alternatively, you can take a single statement and break it up between multiple lines, again for readability. It does not make sense in this case since the statements are so short, but we will be using this functionality in the future.

## Xamarin Studio

### Syntax Highlighting

You will notice that some the code within the IDE is colored differently. The IDE is assisting in making the code easier to read by color coding different elements. By default within Xamarin Studio, classes are highlighted in blue, reserved words are highlighted in teal, and strings are highlighted in orange.

### Solution Structure

In the pane to the far left you will see the solution for our program. At the highlest level is the solution, and below that is the project. Our program is a single project, but a solution can be made up of multiple projects. It is not important now, but as you develop increasingly complicated applications organizing your code into different projects will be invaluable. Within the project are the various pieces that make up the project, so far we are only concerned with the Program.cs file.

### Debug vs Release

By default, when you build applications in Xamarin Studio the result is intended to be used for debugging. As a result, it includes additional metadata and certain optimizations are disabled. This is great when you are developing an application, but not when you are ready to distribute it to other people. When you reach a point where you are ready to create a build for distribution, you will want to change your build target to Release, as seen below:

![Console hello](/blog/2016/10/debug.png)

### Where are the Files?

When you are ready to grab your executable you can access the folder that contains your project by right clicking on it in the solution explorer. On a Mac, you select, "Reveal in Finder." On a PC, select "Open in Explorer?"

![Console hello](/blog/2016/10/reveal-in-finder.png)

One your file browser has opened, navigate to the bin directory, inside this directory you should see a Debug folder, and possibly a Release folder. Each of these folders contains the build for the specified target.

![Console hello](/blog/2016/10/release.png)

### Debugging

While you are developing your applicaiton you may find it valuable to be able to step through the execution of your program. This will allow you to monitor the application as execution progresses.

In order to trigger the debugger we must set a breakpoint within the application. At this breakpoint the program execution will pause, and allow us to control further execution of the program, inspect the state of the application, and even change the state of the application as it executes.

To create a breakpoint, click in the dark gray border to the left of the line numbers in the editor. If you did this correctly, there should be a read dot where you clicked. 

![Console hello](/blog/2016/10/set-breakpoint.png)

Now, to start debugging click play like we did earlier to start the program. At this point we should see the the line we set the breakpoint highlighted in yellow. This indicates that the program is currently paused, and ready to execute the highlighted line.

![Console hello](/blog/2016/10/debugging.png)

### Intellisense

You may have noticed earlier that a dropbox box appears when you were typing the command `Console.ReadLine();` after you typed the member accessor. By typing the member accessor, the IDE kicked in intellisense which anticipated your next action, and attempts to help by presenting you with the set of members on the Console class. This can be very helpful as you work on applicaitons, because you will not have to remember everything about the structure of the application, or the classes that you are using.

![Console hello](/blog/2016/10/intellisense.png)

### Red Squiggles 

In the example below note that there is a read squiggle under `WriteLne` this is the IDE catching an error in the code. Here we have a typo, we missed the "i" in Line in our method. As a result, it highlighted the mistake by indicating it by the read squiggle. You can usually highlight over a squiggle to get a tool tip that indicates why it is there. The IDE will not find all errors, but it is pretty good at picking up simple errors like this one.

![Console hello](/blog/2016/10/red-squiggle.png)

## Variables

Let's consider an algebraic function, 5 + 7 = x. You have probably seen an equation like this in a math class, typically with the directive: solve for x. This scenario is pretty simple, as x is 12. Now, let's extend this thinking into code: 

```cs
var x = 7;
var y = x + 3;
Console.WriteLine(y);
``` 

What do you think will be the output written to the console? Let's find out. 

Create another program using the direction above, and call it `Variables`. Once the project has been created, and finishes loading replace the line `Console.WriteLine("Hello World!");` with the code listed above, and then run it.

Was `10` written to the console? If so, everything worked as expected.

Let's break this down. In this code sample, we have `x` and `y`, which are called variables. A variable is just a way to hold a value during program execution. In this case, our variables are numbers, integers to be specific. Variables can be pretty much any type available to you, numbers, strings, dates, etc. You can think of a variable as a bucket in the computers memory where you can put data into and retrieve data from.

The `var` keyword tells the compiler that we want to initialize a variable, and we want the compiler to figure out what type it should be based on our usage. In our code sample we are only using whole numers that are small in scope. So, the compiler will determine that it should use integers (`int`) for the values. An integer is a mathematical concept for a number that has not fractional or decimal value. An integer can be pretty large, a little over two billion either positive or negative. If we wanted to store larger values we would have to use a different data type that reserves enough memory for larger values. If we wanted to be explicit, we could have used the keyword for the specific type we wanted to use, i.e., `int x = 7;`. 

In our example, we have two variable assignments. The equals sign is the assignment operator. This is where we are telling the run time to store values in memory for retrieval later on. 

```cs
var x = 7;
var y = x + 3;
```

In the line, `Console.WriteLine(y);` we are telling the run time, go out and retrieve the value stored in the variable y so that it can be displayed in the console.

Moving on, let's comment out this section of code using the block comment operators, `/*` and `*/`. These operators allow us to comment out multiple lines at once as indicated below:

```cs
using System;

namespace HelloWorld
{
	class MainClass
	{
		public static void Main(string[] args)
		{
			/*
			var x = 7;
			var y = x + 3;
			Console.WriteLine(y);
			*/

			Console.ReadLine();
		}
	}
}
```

By commenting out the code, we are telling the compiler to ignore the code when it builds the program.




## References

This workshop covers material from the [C# Fundamentals for Absolute Beginners](https://channel9.msdn.com/Series/C-Fundamentals-for-Absolute-Beginners/) series. The videos segements in the course are very thorough, and we recommend you view them in addition to the materials preseneted here as this post contains a subset of the information in the videos. Note that the videos use Visual Studio Express rather than Xamarin Studio, so while the IDE features are similiar they are different. The code backing the solutions is identical.

This part covers the material from the following segments:

* [3 - Your First C# Program](https://channel9.msdn.com/Series/C-Fundamentals-for-Absolute-Beginners/03)
* [4 - Dissecting the First C# Program You Created](https://channel9.msdn.com/Series/C-Fundamentals-for-Absolute-Beginners/04)
* [5 - Quick Overview of the Visual C# Express Edition IDE](https://channel9.msdn.com/Series/C-Fundamentals-for-Absolute-Beginners/05)