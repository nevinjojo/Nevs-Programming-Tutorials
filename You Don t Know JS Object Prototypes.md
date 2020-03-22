# You Don't Know JS: Object Prototypes

*Note: This is from the first-edition of the series, since the second series is still under production. Chapter 1 and 2 notes about `this` can be found in the 'Getting Started' summary.*

## Chapter 3: Objects

### Syntax

Object can be in literal form or constructed form. *[It is preffered for objects to be made in the literal form.](sd)*

    // literal
    var myObj = {
    	key: value
    	// ...
    };
    
    // constructed
    var myObj = new Object();
    myObj.key = value;

### Contents

The contents of an object consist of values (any type) stored at specifically named locations, which we call **properties**.

    var myObject = {
    	a: 2
    };
    
    myObject.a;		// 2
    
    **myObject["a"];	// 2 <- This works! Whaaat?!**

### Methods

Methods are functions within objects 

    var myObject = {
    	foo: function foo() {
    		console.log( "foo" );
    	}
    };

### Arrays v/s Objects

Arrays are objects with slightly more structure on how to access it and where in the object it is located (indexes)

### Duplicating Objects

**There is no defualt `copy()` function**. This is because there is no agreement on whether the copy should be a [shallow copy](s) (where objects within objects are not copied) or a [deep copy](s) (where objects within objects are copied).

If your object is JSON freindly, you can duplicate it with:

    var newObj = JSON.parse(JSON.stringify(someObj));

### Property Descriptors

- You can define if an object is read/write-only.
- If you want to make a constant property, set `writable:false` and `configurable:false`
- To prevent extensions of object, `Object.preventExtensions( myObject );`

    var myObject = {};
    
    Object.defineProperty( myObject, "a", {
    	value: 2,
    	writable: true,     // can change value
    	configurable: true, // can delete property
    	enumerable: true    // can enumarate property in for loops
    } );
    
    myObject.a; // 2

### Getters and Setters

    var myObject = {
    	// define a getter for `a`
    	get a() {
    		return this._a_;
    	},
    
    	// define a setter for `a`
    	set a(val) {
    		this._a_ = val * 2;
    	}
    };
    
    myObject.a = 2;
    
    myObject.a; // 4

### Existence

You can check if a property exists before calling it using `hasOwnProperty`:

    var myObject = {
    	a: 2
    };
    
    ("a" in myObject);				// true
    ("b" in myObject);				// false
    
    myObject.hasOwnProperty( "a" );	// true
    myObject.hasOwnProperty( "b" );	// false

## Chapter 4: Mixing (Up) "Class" Objects

Class describes a certain way of classifying a data structure. [Javascript does not actually have classes, but it has syntax that looks like class design pattern.](d) This pattern can be optionally implemented if you wish.

**Polymorphism**: the idea that a general behaviour from a parent class can be [overwritten](s) in a child class to give it more specifics.

### Class Mechanics

A class is merely an abstract explanation of what characteristics its implementation should implement, but it's not itself a useable object. You must **instantiate** the class before you have a concrete data structure thing to operate against.

### Constructor

    class Car {
      ***constructor(brand) {
        this.carname = brand;
      }***
      present() {
        return 'I have a ' + this.carname;
      }
    }
    
    mycar = new Car("Ford");
    mycar.present();

### Inheritance

    class Square **extends** Shape {
    	constructor(length, color) {
    		**super(length);**
    		this.name = 'square';
    		this.color = color;
    	}
    
    	area() {
    		return this.length*this.length;
    	}
    }
    
    let square = new Square(3, 'green');
    square.area(); // 9

## Chapter 5: Prototypes

Objects in JS has an internal property, called `prototype` which is simply **a reference to another object**. 

*Note: Check out 'Getting Started' to learn more about prototypes.*

### Confusion with Classes and Prototypes

    function Foo() {
    	// ...
    }
    
    Foo.prototype.constructor === Foo; // true
    
    var a = new Foo(); // <- This not a class, but a function
    a.constructor === Foo; // true

Even though the code above has `new` keyword in it, it is not referencing a class, but the `Foo.prototype` object. The `Foo.prototype` object by default gets a property called `.constructor`. Moreover, the `new` refers ot the function which created it.

## Chapter 6: Behavior Delegation

*Note: No summary for this one. Check the following page for further information:*

[getify/You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/this%20%26%20object%20prototypes/ch6.md)