# JavaScript Tutorial
Welcome to the JavaScript tutorial! If you are familiar with Java, you are gonna feel right at home. If not, let's get started!

## Beginner
You can skip this if you have done Java. Have a quick read if you want to checkout the syntax.

### Internal scripting
This goes inside HTML

```
<script>
    alert("I am called from a .html file!");
</script>
```

Note: you can view the result of an alert in the console of your browser (developer mode).

### External scripting
This goes in a .js file

```
alert("I am called from a .js file!");
```

You reference the .js file in an .html file by adding the following:
`<script src="script.js"></script>`


### Variables
The following asks the user for a name using a pop up and set the result to a var
`var name = prompt('What's your name?');`

You can also initialize the variable yourself:
`var name = "Jeff";`


### Quick Maths!

```
var apples = 10;
var oranges = 12;
var total = apples + oranges;
```

### Logic

* Equal to: `===`
* Not equal to: `!==`
* Greater than: `>`
* Less than: `<`
* Greater than or equal to: `>=`
* Less than or equal to: `<=`

Note: `10 === 10` returns `true`, but `10 === '10'` returns `false`.

### Conditional

```
if (43 > 2) {
   // some code
} else {
   // some other code
}
```

Note: `//` are comments

### Loops
#### While loops

```
var i = 0;
while (i < 10) {
  alert(i);
  i++; // same as i = i + 1
}
```

#### For loops

```
for (var i =0; i < 10; i++) {
  alert(i);
}
```

### Functions

```
var add = function (a,b) {
  return a + b;
}

var result = add(10, 12);  // result equals 22
```

### Objects
Creating objects in JavaScript is as simple as creating structs in C (if you are familiar with it). You can declare and/or initialise variables and functions within the objects.

```
var newObj = {};  // empty object

var person = {
  name: "Jeff",
  age: 43,
  talk: function () {
    alert ("My name is" + name);
  }
}
```

You can assign values to the variables within the objects by doing the following:
`person.name = "Thomas the tank engine";`

You can add new variables and functions on the fly as well:
`person.dance = function () { alert("I'm dancing!"); }`

### Arrays

```
var newList = []; // empty list
var shoppingList = ['Spaghetti', 'Baked Beans'];

var itemOne = shoppingList[0]; // itemOne is 'Spaghetti'
shoppingList[1] = 'potato'; // changed value from 'Baked Beans' to 'potato'

shoppingList.push('Baguette'); // adds 'Baguette' to the END of the list
shoppingList.pop(); // removes LAST item, i.e. 'Baguette' from the list
```



## Intermediate Level
This is where we are actually going to integrate HTML, CSS and JavaScript.

### DOM (Document Object Model)
The Document Object Model is a way to manipulate the structure and style of an HTML page. It represents the internals of the page as the browser sees it, and allows the developer to alter it with JavaScript.

You can find the complete DOM element documentation [here](https://www.w3schools.com/js/js_htmldom_document.asp).

#### Getting an element from HTML or XML
To work with the elements in HTML or XML, you need to get the element first:

* By ID: `var pageHeader = document.getElementById('page-header');`
* By Tag Name: `var divs = document.getElementsByTagName("div");`
* By Class Name: `var x = document.getElementsByClassName("intro");`
* By CSS Selector: `var x = document.querySelectorAll("p.intro");` or `var x = document.querySelectorAll("p#title");`

### Events and Callbacks
Events in websites occur when the page loads, when user clicks a button, scrolls down etc.
To react to an event you listen for it and supply a function which will be called by the browser when the event occurs. This function is known as a callback.

```
var handleClick = function (event) {
    // do something!
};

var button = document.querySelector('#big-button');
button.addEventListener('click', handleClick);
```

Note: As we all know, Internet Explorer is a useless piece of trash. Therefore it does not support certain functions such as `addEventListener`.

### [AJAX](https://htmldog.com/guides/javascript/intermediate/ajax/)
To retrieve new content for a page, like new articles on an infinite-scroll site or to notify you of new emails, a tool called an **XML HTTP Request (XHR)** is used. Web apps that do this are also called **AJAX apps**, AJAX standing for **Asynchronous JavaScript and XML**.

```
var req = new XMLHttpRequest();
req.onload = function (event) { . . . };
req.open('get', 'some-file.txt', true);
req.send();
```

The first thing to do is create a new XMLHttpRequest request, using the new keyword, and calling XMLHttpRequest like a function.

Then we specify a callback function, to be called when the data is loaded. It is passed information about the event as its first argument.

Then we specify how to get the data we want, using req.open. The first argument is the HTTP method (GET, POST, PUT etc). Next is the URL to fetch from - this is similar to the href attribute of a link.

The third is a boolean specifying whether the request is asynchronous - here we have it true, so the XMLHttpRequest is fired off and then code execution continues until a response from the server causes the onload callback to be fired.

The asynchronous parameter defaults to false - if it’s false, execution of the code will pause at this line until the data is retrieved and the request is called synchronous. Synchronous XMLHttpRequests are not used often as a request to a server can, potentially, take an eternity. Which is a long time for the browser to be doing nothing.

On the last line we tell the browser to fire off the request for data.

### JSON - JavaScript Object Notation
JSON is a set of text formatting rules for storing and transferring data. Note that **JSON is not JavaScript** but a totally different language by itself. JSON is used by a lot of other languages as well. JSON can be used to transfer simple data such as text, arrays etc, but not complex data like functions.

Example:

```
{ "name": "Jeff", age: 43, "tools": {"gun": "AK-47"} }
```

#### Using JSON
`JSON.stringify` is a method that can be called to convert objects into a JSON string:

```
var input = JSON.stringify({
  name: "Jeff",
  age: 43
});
```

The string can then be converted back to JavaSrcipt object using `JSON.parse` function
`var output = JSON.parse(input);`
The JSON object can now be used like a normal object:

var newName = output.name;`


### [Scope](https://htmldog.com/guides/javascript/intermediate/scope/)
If you have done OOP using languages such as Java, you should be familiar with Scopes. Scope is the term used to mean variable visibility — a variable’s scope is the part of your code that can access and modify that variable.

For example, a is not defined outside the function:

```
var doSomething = function () {
    var a = 10;
};

doSomething();

console.log(a); // a is undefined
```

However, in this block of code, b is defined as it is not within a function:

```
var a = 10;

if (a > 5) {
    var b = 5;
}

var c = a + b; // c is 15
```

### jQuery - The cross-browser hero we deserve!
jQuery is the cross-browser DOM library that handles the differences in functions between various browsers. This is needed because different browsers will only accept particulars functions or not accept some of the mosr popular functions at all (cough cough Internet Explorer cough). The way you interact with DOM is called Application Programming Interface API. 

jQuery has a very distinctive syntax, all based around the dollar symbol:

```
$('.btn').click(function () {
    // do something
});
```

**Important Note:** To use jQuery, you need to either [download](http://jquery.com/download/) the script and add it to your site directory, or include the script to your HTML:

```
<script src="http://cdnjs.cloudflare.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
```

#### jQuery and DOM API
jQuery uses a really neat chainable syntax that allows the following code format:

`$('.note').css('background', 'red').height(100);`

`$` is a function that returns a jQuery wrapper around an element. .css is a method of that jQuery wrapper and it too returns the same wrapper. .height sets the height (duh) of the element selected, and of course there’s an equivalent for width.

Other examples:

```
var currentHeight = $('.note').height(),
    currentColor = $('.note').css('color');
```

#### JavaScript v/s jQuery tricks
Using jQuery for certain tasks are more prefered over using JavaScript as it works with all browsers.

##### DOMContentLoaded
Suppose you want to do something after the DOM content (HTML content) is loaded but before the css is loaded:

| JavaScript                                                  | jQuery                          |
| ----------------------------------------------------------- | ------------------------------- |
| `window.addEventListener('DOMContentLoaded', doSomething);` | `$(window).ready(doSomething);` |

##### Load
Suppose you wanna do something once the page is fully loaded:

| JavaScript                                      | jQuery                         |
| ----------------------------------------------- | ------------------------------ |
| `window.addEventListener('load', doSomething);` | `$(window).load(doSomething);` |

##### Type Checking
Since JavaScript lets you create variables without specifying the type, it is easy to make errors with value types. It's way easier to check for types of variables in jQuery than in JavaScript:

```
$.isArray([1, 2, 3]); //checks if [1, 2, 3] is an array

$.isFunction(function() {}); // checks if function() {} is function or not

$.isNumeric('Tom'); // returns false

$.isPlainObject({ name: 'Tom' }); // returns true
```

#### Setting Context for jQuery
Sometime you want to specify that jQuery should only access certain branches of the DOM tree. To do that you can specify the context the following way:

```
var $header = $('header'),
    $headerBoxes = $('.note', $header);
```

This way jQuery will only look for the `note` object within the `$header` context.

#### jQuery and AJAX helper methods
jQuery allows you to also construct, call and send ajax requests.

##### `$.ajax`
Allows to manually construct a request.

```
$.ajax({
  url: `/src.json`,
  method: 'GET',
  success: function () {
    console.log("success: " + data);
  },
  error: function () {
    // something something..
  }
});
```

##### `$.get`
Allows to get data from a json file.

```
$.get('/src.json', function (data) {
  console.log(data);
});
```

##### `$.post`
Allows to send request/ data to a server.

```
$.post('/save', { username: 'tom' }, function (data) {
    console.log(data);
}).fail(function () {
    // Uh oh, something went wrong
});
```



## Advanced Level
Well, this is not really 'advanced' if you are familiar with programming in other programming languages. These topics will be focused to people who are more new to the web-development world. Have a quick read through the topics even if you are familiar it. Who knows, maybe you'll pick up something new!

### Object Oriented Programming
Note: I will only be going through these topics briefly. Check out [htmldog's](https://htmldog.com/guides/javascript/advanced/oo/) description for a more detailed understanding.

Objects helps us categorise the data and functionalities in a program. These objects helps you connect the different parts of the program together. The grouping of data and behavior into one entity is called **encapsulation**.

#### Inheriance
A powerful tool available to you when building applications using objects is called inheritance. This is where an object inherits properties and methods from another object. An object can alter the properties and methods it inherits and add additional properties and methods too.

##### Creating an object constructor

```
var Person = function (name) {
    this.name = name;
};
```

Note the capital letter on Person - this is an important convention. Generally, in JavaScript, if it’s got a capital first letter, it’s a constructor and should be used with the new keyword.

Now you can use the new keyword to create Person objects:
`var jeff = new Person('jeff');`

##### Prototype Constructor function
You can manually create the object from which the new objects will inherit by adding properties and methods to the prototype property of the constructor function.

```
// here, new function say is created on the fly using the existing variables from the constructor
Person.prototype.say = function (words) {
  alert(this.name + ' said ' + words);
};
```


