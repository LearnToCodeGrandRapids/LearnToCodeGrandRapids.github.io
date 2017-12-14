# JavaScript Basics

This workshop is heavily influenced by "[JavaScript Basics - Learn Web Development](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/JavaScript_basics)" by MDN.

JavaScript is one of the most widely used programming languages today. If you've viewed a webpage there is a good chance it used JavaScript. The objective of this workshop is to take you through the steps necessary to setup a basic sandbox to experiment with JavaScript, and to learn just enough JavaScript to be dangerous.

## Setting up the Sandbox

The first thing that we are going to do is setup a basic sandbox that you can use to experiment with JavaScript. To do this we are going to setup a couple of files. This process is pretty straight simple since all you need to do is download the package from github:

https://github.com/LearnToCodeGrandRapids/javascript-sandbox

Let's take a look at the files in this repository:

`sandbox.html` this is the html file that you will load in the browser. If you look at the source you will notice that most of the content is filler the important piece is:

```js
<script type="text/javascript" src="sandbox.js"></script>
```

This statement is what load our `sandbox.js` file, this is where we will be writing our JavaScript throughout the workshop. You may have also noticed that `sandbox.js` is the last thing loaded in the body. The reason for this is that when the JavaScript is loaded it can only affect elements that existed prior to its execution, so if we had elements after it, it would not be able to interact with them right away.

Once you are familiar with the files in the sandbox, open `sandbox.html` in your browser (if its Chrome that will help you follow along later).

## Hello World!

The first thing we are going to do is some basic DOM manipulation. DOM stands for document object model. It descriptions the logic structure of the website within the browser. In this example we will be interacting with the DOM to change the title displayed on the page. This can be accomplished by writing the following code in your `sandbox.js` file:

```js
var myHeading = document.querySelector('h1');
myHeading.textContent = 'Hello world!';
```

After making this change you should be able to refresh your browser and see the title of the page change.

### How does this work?

On the first line we are grabbing the DOM element that contains the title using the `querySelector` method on the `document` object. You can think of `document` as the root of the DOM. Once the title header element has been retreived we store it in a variable named `myHeading`. We will talk more about variables later. The important part here is understanding that for us to interact with an element in the DOM we first must select it. The `querySelector` method takes an argument that is similiar to a css selector. 

Once we have the title in the variable `myHeading` we assign the value `Hello world!` to the `textContent` field. `textContent` is the content held within the element in plain text.

### What's next?

At this point we've master JavaScript. Go out and write the next Facebook. What's a variable you ask? Perhaps we should go over a few things.

## JavaScript Basics

As you work in the face paced world of front end development you will spend most of your time interacting with frameworks, but it is still useful to understand the fundamentals of JavaScript.

### Comments

Comment provide you with a way to add commentary within your code that explains why things are the way they are without affecting the actual execution of the code. There are two ways to defined comments in JavaScript: 

First, you defined a multiline comment in a way similiar to CSS:

```js
/*
    This is a comment in JavaScript
    it can contain multiple lines.
 */
```

Second, you can do inline, single line comments:

```js
// just use double slashes and then all trailing text will become a comment
```

It may be tempting to use comments to describe what your code is doing when you first start out. If this is helpful you can do it while developing, just make sure to remove those comments before you give your code to anyone else. Comments are there to explain the _why_ not the _what_ of your code.

### Variables

You can think of a variable as a bucket that holds a value. This isn't technically accurate from a computer science perspective, but when you're just getting started its conceptually accurate. Why do we need variables? In order to do anything interesting you need a way to preserve, and build state over time. Variables give us a way to do this.

You create a variable using the `var` keyword. You can name a variable almost anything you want, for example:

```js
var myAwesomeVariable;
```

There are a few things you will want to make note of here. The first is that the name of our variable does not contain spaces, and the capitalization varies. We call this camel case, because the capitals at the beginning of words look kind of like the humps on the back of a camel. Camel case is a defacto style in JavaScript. Why does this matter? JavaScript is case sensitive. That means that `myAwesomeVariable` is distinct from `myawesomevariable` so you could use both in the same script. Don't use both, you will only confuse yourself in the future.

The other thing to note is that the statement ends with a semi-colon. The semi-colon at the end of the statement tells the browser that the statement is complete. You don't always need to put a semi-colon at the end of a statement as the interpretter will try its best to guess your intent and insert them for you automatically. In practice it is better to explicitly define your semi-colons.

What would make our awesome variable more awesome? How about assigning it a value? We can do that like this:

```js
myAwesomeVariable = 'Learn to Code';
```

In this statement you'll notice the equal sign. This is called the assignment operator. It takes the value to the right of the equal sign and assigns it to the variable to the left of it. 

If you want to create a variable and immediately assign it a value you can do that too:

```js
var myAwesomeVariable = 'Learn to Code';
```

You can retreive the value in a variable just by calling it. For example, if you enter the prevous statement in the console of your browser, you can print the value by entering this in the console:

```js
myAwesomeVariable;
```

You can change the value within a variable as well, we call this mutation. If you hear people talking about mutable vs immutable this is related. Mutable means the value can change, whereas immutable means that it cannot.

```js
var myAwesomeVariable = 'Learn to Code';
myAwesomeVariable = 'Grand Rapids';
```

Variables have datatypes. The examples we have looked at so far have all been `string`, but there are a few other core types:

| Variable      | Explanation                                                                                                      | Example                                      |  
| ------------- | ---------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |  
| String  | A string is text, think something like your name or address. It has to be enclosed in quotes. | `var myAwesomeVariable = 'Learn to Code';` |  
| Number  | A number, think you age or zip code | `var myAwesomeVariable = 10;` |  
| Boolean | A True/False value, `true` and `false` are keywords, not strings so you don't need quotes | `var myAwesomeVariable = true;` |  
| Array   | A data structure that contains values, think shopping list | `var myVariable = ['Banana','Peanut Butter','Wonderbread','Quantumgemerald'];` |  
| Object |  Anything. All variables in JavaScript are technically objects, there are further implications to this but we will not discuss them here. | | 

### Operators

We have already talked about operators a little, we looked at the assignment operator, `=`, while discussing variables. An operator is a mathematical symbol that takes two values (that can be contained in variables) and produces some kind of result.

`-`, `/`, and `*` are basic math operators and do what you would expect.

`+` is a little more complicated, because in the context of numbers it performs addition as you would expect, but in the contains of strings it appends them together, we call this string concatenation.

From here things get a little weird. `===` is the equality operator. It checks two values and evaluates if they are equal. You may also so `==` in code. This is the _loose_ equality operator, it works in a way that is less predicable. When in doubt use strict equality with `===`.

`!` is the not operator. It can be used to invert the truthiness of a statement. By extention `!==` is the strict inequality operator. It checks to make sure that two values are not equal.

There are more operators, but these should cover most cases you will run across.

Mixing data types can yield all kinds of fun and interesting results...unless you're trying to debug them. JavaScript is a dynamic language, which allows you to intermix values of different types. The interpretter will do its best to infer your intent when data types mix. For example, if you do something like `"10" + 5` you may think the result would be `15`, but since `"10"` is a string the interpretter will think you want to do string concatenation and convert `5` to `"5"` automatically. We call this type coersion. 


===============
        


### Conditionals

Conditionals are code structures which allow you to test if an expression returns true or not, running alternative code revealed by its result. A very common form of conditionals is the `if ... else` statement. For example:
    
    
    var iceCream = 'chocolate';
    if (iceCream === 'chocolate') {
      alert('Yay, I love chocolate ice cream!');    
    } else {
      alert('Awwww, but chocolate is my favorite...');    
    }

The expression inside the `if ( ... )` is the test — this uses the identity operator (as described above) to compare the variable `iceCream` with the string `chocolate` to see if the two are equal. If this comparison returns `true`, the first block of code is run. If the comparison is not true, the first block is skipped and the second code block, after the `else` statement, is run instead.

### Functions

[Functions][24] are a way of packaging functionality that you wish to reuse. When you need the procedure you can call a function, with the function name, instead of rewriting the entire code each time. You have already seen some uses of functions above, for example:

1.     var myVariable = document.querySelector('h1');

2.     alert('hello!');

These functions, `document.querySelector` and `alert`, are built into the browser for you to use whenever you desire.

If you see something which looks like a variable name, but has parentheses — `()` — after it, it is likely a function. Functions often take [arguments][25] — bits of data they need to do their job. These go inside the parentheses, separated by commas if there is more than one argument.

For example, the `alert()` function makes a pop-up box appear inside the browser window, but we need to give it a string as an argument to tell the function what to write in the pop-up box.

The good news is you can define your own functions — in this next example we write a simple function which takes two numbers as arguments and multiplies them:
    
    
    function multiply(num1,num2) {
      var result = num1 * num2;
      return result;
    }

Try running the above function in the console, then test with several arguments. For example:
    
    
    multiply(4,7);
    multiply(20,20);
    multiply(0.5,3);

**Note**: The [`return`][26] statement tells the browser to return the `result` variable out of the function so it is available to use. This is necessary because variables defined inside functions are only available inside those functions. This is called variable [scoping][27]. (Read [more on variable scoping][28].)

### Events

Real interactivity on a website needs events. These are code structures which listen for things happening in browser, running code in response. The most obvious example is the [click event][29], which is fired by the browser when you click on something with your mouse. To demonstrate this, enter the following into your console, then click on the current webpage:
    
    
    document.querySelector('html').onclick = function() {
        alert('Ouch! Stop poking me!');
    }

There are many ways to attach an event to an element. Here we select the `<[html>`][30] element, setting its [`onclick][31]` handler property equal to an anonymous (i.e. nameless) function, which contains the code we want the click event to run.

Note that
    
    
    document.querySelector('html').onclick = function() {};

is equivalent to
    
    
    var myHTML = document.querySelector('html');
    myHTML.onclick = function() {};

It's just shorter.

## Supercharging our example website

Now we've gone over a few JavaScript basics, let's add a few cool features to our example site to see what is possible.

### Adding an image changer

In this section, we'll add an additional image to our site using some more DOM API features, using some JavaScript to switch between the two when the image is clicked.

1. First of all, find another image you'd like to feature on your site. Be sure it is the same size as the first image, or as close as possible.
2. Save this image in your `images` folder.
3. Rename this image 'firefox2.png' (without quotes).
4. Go to your `main.js` file, and enter the following JavaScript. (If your "hello world" JavaScript is still there, delete it.) 
    
        var myImage = document.querySelector('img');
    
    myImage.onclick = function() {
        var mySrc = myImage.getAttribute('src');
        if(mySrc === 'images/firefox-icon.png') {
          myImage.setAttribute ('src','images/firefox2.png');
        } else {
          myImage.setAttribute ('src','images/firefox-icon.png');
        }
    }

5. Save all files and load `index.html` in the browser. Now when you click the image, it should change to the other one!

You store a reference to your `<[img>`][32] element in the `myImage` variable. Next, you make this variable's `onclick` event handler property equal to a function with no name (an "anonymous" function). Now, every time this element is clicked:

1. You retrieve the value of the image's `src` attribute.
2. You use a conditional to check whether the `src` value is equal to the path to the original image: 
    1. If it is, you change the `src` value to the path to the 2nd image, forcing the other image to be loaded inside the `<[img>`][32] element.
    2. If it isn't (meaning it must already have changed), the `src` value swaps back to the original image path, to the original state.

### Adding a personalized welcome message

Next we will add another bit of code, changing the page's title to a personalized welcome message when the user first navigates to the site. This welcome message will persist, should the user leave the site and later return — we will save it using the [Web Storage API][33]. We will also include an option to change the user and, therefore, the welcome message anytime it is required.

1. In `index.html`, add the following line just before the `<[script>`][10] element: 
    
        Change user

2. In `main.js`, place the following code at the bottom of the file, exactly as written — this takes references to the new button and the heading, storing them inside variables: 
    
        var myButton = document.querySelector('button');
    var myHeading = document.querySelector('h1');

3. Now add the following function to set the personalized greeting — this won't do anything yet, but we'll fix this in a moment: 
    
        function setUserName() {
      var myName = prompt('Please enter your name.');
      localStorage.setItem('name', myName);
      myHeading.textContent = 'Mozilla is cool, ' + myName;
    }

This function contains a [`prompt()`][34] function, which brings up a dialog box, a bit like `alert()`. This `prompt()`, however, asks the user to enter some data, storing it in a variable after the user presses **OK**_._ In this case, we are asking the user to enter their name. Next, we call on an API called `localStorage`, which allows us to store data in the browser and later retrieve it. We use localStorage's `setItem()` function to create and store a data item called `'name'`, setting its value to the `myName` variable which contains the data the user entered. Finally, we set the `textContent` of the heading to a string, plus the user's newly stored name.
4. Next, add this `if ... else` block — we could call this the initialization code, as it structures the app when it first loads: 
    
        if(!localStorage.getItem('name')) {
      setUserName();
    } else {
      var storedName = localStorage.getItem('name');
      myHeading.textContent = 'Mozilla is cool, ' + storedName;
    }

This block first uses the negation operator (logical NOT, represented by the !) to check whether the `name` data exists. If not, the `setUserName()` function is run to create it. If it exists (that is, the user set it during a previous visit), we retrieve the stored name using `getItem()` and set the `textContent` of the heading to a string, plus the user's name, as we did inside `setUserName()`.
5. Finally, put the below `onclick` event handler on the button. When clicked, the `setUserName()` function is run. This allows the user to set a new name, when they wish, by pressing the button: 
    
        myButton.onclick = function() {
      setUserName();
    }
    

Now when you first visit the site, it will ask you for your username, then give you a personalized message. You can change the name any time you like by pressing the button. As an added bonus, because the name is stored inside localStorage, it persists after the site is closed down, keeping the personalized message there when you next open the site!

## Conclusion

If you have followed all the instructions in this article, you should end up with a page that looks something like this (you can also [view our version here][35]):

![][36]

If you get stuck, you can compare your work with our [finished example code on GitHub][37].

We have barely scratched the surface of JavaScript. If you have enjoyed playing, and wish to advance even further, head to our [JavaScript learning topic][5].

 
