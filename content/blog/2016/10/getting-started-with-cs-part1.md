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

## .NET Overview

* Class Library
* Runtime

## References

This workshop covers material from the [C# Fundamentals for Absolute Beginners](https://channel9.msdn.com/Series/C-Fundamentals-for-Absolute-Beginners/) series. This part covers the material from the following segments:

* [3 - Your First C# Program](https://channel9.msdn.com/Series/C-Fundamentals-for-Absolute-Beginners/03)