# You Don't Know JS Yet: Getting Started

## Chapter 1: What is JS

- `alert()`, `getCurrentLocation()`, `console.log()` etc are not a JS specified function. It is a web JS API specification.
- Developer tools might not strictly adhere to the way JS is handled. It is not made by JS developers.
- Javascript is **multi-paradigm** ⇒ Paradigm is the style of the language:
    - Procedural Paradigm ⇒ top-down linear code ⇒ e.g. C
    - Object Oriented ⇒ collect data and logic together as classes ⇒ Java
    - Functional Paradig, ⇒ organizes code into functions ⇒ Haskel
    - Multi-Paradigm ⇒ Mix and match of above ⇒ Javascript
- JS is **backwards-compatible** ⇒ once something is acceptable as valid JS, there will not be a future change to language that makes it invalid. [JS is not forward compatible](d)
- JS is an **interpreted scripting language**.
- **JS Execution format:** Once the JS code is [parsed](d) and [transpiled to Babel](d), it is delivered to JS engine, which [parses the code to an AST](s) (Abstract Syntax Tree). The engine converts the [AST to byte code](d)  which is refined and optimized by JIT compiler. Finally, the [JS VM executes the program](s). Because of all of this parsing and conversion, JS 'kinda' acts like a compiled language.
- **Strict mode**: `"use strict";` is an optionality in JS. This is more of a guidance on how to write JS programs the best. This can be used globally or just inside a function.

## Chapter 2: Surveying JS

- **Each file is a program**. Each file share their states via global scopes (e.g. `import` and `export` or `<script>` tags) to function as one big program. ⇒ Allows error handling if one file fails

### Values

- **Values can be primitives or objects:** Values are embedded in programs using `literals`. E.g "Hello" is a primitive  string literal.
    - Tip: literals are the 'literally', the actual value, not the variable assigned to it
    - **Common primitive literals**: `string`, `boolean`, `numbers`, `bigint` (used for large numbers), `null,` `undefined`, (both null and undefined represent 'emptiness') and `symbol` (a special-purpose value that behaves as a hidden unguessable value)
    - **Array**: type of **object** that comprised of an **ordered** and **numerically indexed list** of data
    - **Object**: **unorderd, 'key'ed collection; E.g.**

            var me = {
            	first = "Nevin",
            	last = "Jojo",
            	age = 20,
            	specialties = ["JS", "Swimming"]
            };

    - **Find type of value using `typeof`**:

        `typof "abc" // "string"`

        `typeof [1,2,3] // "object"`

        `typeof null // "object" ← bug, should be null`

        `typeof function hello(){} // "function"`

- **Variables = containers for values**

    Variables have to be declared to be used. Otherwise we will get `undeclared` error.

    - `**let` vs `var`**: [let allows a more limited access to the variabl](s)[e than var.](ss) This is called "**block scoping**" . This means that if both var and let are inside a block of code, the var is accessible outside the block, but let is not. var is usually avoided in most cases.
    - `const`: once a const is declared, it cannot be re-assigned to another value.

### Functions

In JS, function means "a procedure", which is a collection of statements that can be invoked one or more times. **Functions can only return one value (unless the values are wrapped into an object/array).** Fuctions can be used within objects since they are values.

    function hello() { ... }
    
    // You can also assign a function to a variable
    var greeting = function hello() { ... }

### Equality & Comparisons

- `===` operator look for **strict equality** ⇒ **checks for both value and type**
    - Note: Doesn't work in `NaN === NaN` (use Number.isNaN() instead) and `0 === -0`
    - E.g. `[ 1, 2, 3 ] === [ 1, 2, 3 ]; // false`
- `==` operator look for **loose equality ⇒ only checks for value**

        42 == "42";             // true
        1 == true;              // true

### Organising in JS: Using Classes and Modules

**Classes**: a type of [custom data structure](a) that i[ncludes both data and behaviour](s) that operates on the data.

    class TextBook() {
    	// constructor of class. Important!!!
    	**constructor() {
    		this.pages = [];
    	}**
    
    	// method
    	print() {
    		for (let page of this.pages) {
    			console.log(page);
    		}
    	}
    }
    
    // creating TextBook instead called `notes`
    let text = new TextBook();
    text.print();

**Inheritance and polymorphism:** 

    class Notebook **extends TextBook** {
    	constructor() {
    		**super(this.pages);**
    		this.date = new Date();
    	}
    
    	print() {
    		super.print();
    		console.log(date);
    	}
    }
    
    let notes = new NoteBook();
    notes.print();

**Modules:** Module is j**ust a function.** Written similar to Classes, but has [different syntax](s). Modules doesn're require `this.` prefix.

    // Notebook.js
    function Notebook() {
    	var publicAPI = {
    		var pages = [];
    		var print() {
    			for (let page of this.pages) {
    				console.log(page);
    			}
    		}
    	}
    	return publicAPI;
    }
    
    
    //import this module to main.js
    import { Notebook } from "Notebook.js"

## Chapter 3: Digging to the Roots of JS

### Iteration

Iterator pattern **progressivdly handle the collection of data** by processing first part, then the next, and so on. Iterator pattern consumes ***iterables* (such as arrays)**.  Some examples of iterator patterns are `for...of` loop, `...` operator, etc.

    var pages = [1,2,3];
    
    // for...of loop
    for (let page of pages) {
    	console.log(page);
    }
    
    // for loop for arrays if you want the index
    for (let [idx, val] of pages.entries()) {
    	console.log(`${idx}: ${val}`);
    }
    // 0: 10
    // 1: 20
    // 2: 30
    
    //... operator => Spreads array out of the array object
    var vals = [...pages];
    // You can also spread string to char array
    var chars = [..."Hello"] // ["H", "e", "l", "l", "o"]

**Maps**: uses **keys** associated to **values. An entry is a tuple (2-element array) including both key and value.** 

    var buttonNames = new Map();
    buttonNames.set(btn1,"Button 1");
    buttonNames.set(btn2,"Button 2");
    
    // iterate through keys and values
    for (let [key, val] of buttonNames) {
    	...
    }
    
    //iterate trhough values only
    for (let val of buttonNames.values()) {
    	...
    }
    
    //iterate trhough values only
    for (let key of buttonNames.keys()) {
    	...
    }

### Closure

Closure is when function r**emebers and continues to access variables outside scope**, even when the function is executed in different scope.

    function greeting(msg) {
        return function who(name) {
            console.log(`${ msg }, ${ name }!`);
        };
    }
    
    var hello = greeting("Hello");
    
    hello("Kyle");
    // Hello, Kyle!

### `this` keyword

`this` is not a fixed characteristic of a function, but rather a dynamic characteristic that is determined each time a function is called.

In the case below, `this.` refers to an instance of `Person`.

    function Person(fn, ln) {
    	this.first_name = fn;
    	this.last_name = ln;
    
    	this.displayName = function() {
    		console.log(`Name: ${this.first_name} ${this.last_name}`);
    	}
    }

### Prototypes

Prototype is a characteristic of an object. **It is a "hidden linkage" between the object you created and another object that already exist.** 

    var homework = {
        topic: "JS"
    };
    
    homework.toString();    // [object Object]

In the example above, homework object has a single property `topic`. However, [its default property linkage connects to `Object.prototype` object, which has common built-in methods on it like `toString()` and `valueOf()`, among others](as).

To define a linkage between your object and existing Object, `create` the object like this:

    var homework = {
        topic: "JS"
    };
    
    var otherHomework = Object.create(homework);
    
    otherHomework.topic;   // "JS"

## Chapter 4: The Bigger Picture

### Pillar 1: Scope and Closure

**Scopes** nest inside each other, and for any given expression or statement, only variables at that level of scope nesting, or in higher/outer scopes, are accessible; variables from lower/inner scopes are hidden and inaccessible.

When a function makes reference to variables from an outer scope, and that function is passed around as a value and executed in other scopes, it [maintains access to its original scope variables](d); this is **closure**.

### Pillar 2: Prototypes

Prototypes origined before `class` keyword was created. Before `class` people implemented the class design pattern on top of prototypes (prototype inheritance).

### Pillar 3: Types and Coercion

Read Chapter 2 to learn more