# You Don't Know JS: Types & Grammar

## Chapter 1: Types

**Type** is a [built-in set of characteristics](n) that uniquely [identifies the behaviour of a particular value](n) and distinguishes it from other value.

- JavaScript has 8 built-in types. [Everything except `object` are primitive types](x):
    - `**null**` ← `typeof null === "object";` ← **This is technically true, but is a bug in JS. It should actually be `"null"`**
    - `**undefined**` ← `typeof undefined === "undefined";`
    - `**boolean**` ← `typeof true === "boolean";`
    - `**number**` ← `typeof 42 === "number";`
    - `**string**` ← `typeof "42" === "string";`
    - `**object**` ← `typeof { life: 42 } === "object";`
    - `**symbol**` -- added in ES6! ← `typeof Symbol() === "symbol";`
    - `**function`** ← `typeof function a() { /*..*/ } === "function"`
- **Functions are "callable objects"**. They can have parameters inside it, and we can call `.length` on the function name to get the number of parameters  it has.
- **Arrays are just structured objects** (which is why it is not considered as a built-in type). `typeof [1,2,3] === "object";`

### Values as Types

In JS, V**[ariables don't have types, only values have types](s)**. Variables just holds the value with the particular type. This is why JavaScript doesn't have type enforcement, unlike TypeScript.

### `undefined` vs "undeclared"

- `undefined` → Variables that has no value currently assigned to it.
- "undeclared" → A variable that has not been declared/created yet.

`**typeof` operator returns `undefined` in both undefined and undeclared cases.** Therefore a safety guard to check for this case is this:

    if (typeof atob === "undefined") {
    	atob = function() { /*..*/ };
    }
    
    //Here is a better/simpler solution:
    **if (!atob) {
    	// Do something with function atob
    }**

## Chapter 2: Values

### Arrays

Since arrays are objects you can set properties to arrays. However, this way of setting properties does not increment the length of the array. Generally, it's not a great idea to add string keys/properties to arrays.

    var a = [ ];
    
    a[0] = 1;
    a["foobar"] = 2;
    
    a.length;		// 1
    a["foobar"];	// 2
    a.foobar;

### Array-likes

If you have an array-like object and want to use array features such as `findIndexOf(..)`, you can use `Array.prototype` on the object:

    function foo() {
    	var arr = **Array.prototype.**slice**.call( arguments );**
    	console.log( arr );
    }
    
    foo( "bar", "baz" ); // ["bar","baz"]
    // or, you can do this...
    Array.from(arguments); // [arg1, arg2, ..., argn]

### Strings

**Strings are not array of `char`s**, even though they have features such as `findIndexOf(..)`, `.length`, `.concat(..)` etc. Strings are immutable, while arrays are mutable.

Therefore, If you want the features of an array to be available on a string, do the following:

    let a = "foo!";
    a.reverse;		// undefined
    a = **a.split('').reverse().join('')**;

### Numbers

- In JS, "integer" is just a value that has no fractional decimal value. This means that `42.0` is as much of an "integer" in JS as `42`.

        Number.isInteger( 42 );		// true
        Number.isInteger( 42.000 );	// true
        Number.isInteger( 42.3 );	// false

- The leading or trailing `0` in `0.42` and `42.0` respectively, are optional. However, `42.` is very uncommon.
- You can write shortform of exponential values and convert it to full form if you wish:

        var a = 5E10;
        a;					        // 50000000000
        a.toExponential();	// "5e+10"

- **Small Decimal Values:** Note that `0.1 + 0.2 === 0.3` return `false`. This is because `0.1` and `0.2` are binary floating points which adds up to `0.30000000000000004` not `0.3`.

### Special Values

- **The Non-value Values**: both `null` and `undefined` are used interchangably, which is wrong.
    - `null` is an empty value ⇒ had a value and doesn't anymore
    - `undefined` is a missing value ⇒ hasn't had a value yet
- Any mathematical operation that does not include all `numbers` will return a `NaN` value. ⇒ `var a = 2 / "foo"; // NaN`.
    - `NaN == NaN` returns `false`.
- `Number.POSITIVE_INFINITY` and `Number.NEGATIVE_INFINITY`

        var a = 1 / 0;	// Infinity
        var b = -1 / 0;	// -Infinity

- Zeroes

        var a = 0 / -3; // -0
        var b = 0 * -3; // -0

### Value v/s Reference

- There are no pointers in JavaScript.
- If you have x references to a (shared) value, it means that they are all x distinct references to a single shared value; none of them are references/pointers to each other.

        var a = 2;
        var b = a; // `b` is always a copy of the value in `a`
        b++;
        a; // 2
        b; // 3

## Chapter 3: Natives

### Internal Class

Values that are `typeof` `"object"` (such as an array) are additionally tagged with an internal `[[Class]]` property:

    Object.prototype.toString.call( [1,2,3] ); // "[object Array]"
    Object.prototype.toString.call( null );		 // "[object Null]"

### Wrapping and Unboxing values

    // Wrapping "abc" to a string object
    var a = new String("abc");
    
    // Unboxing `a` to fetch the value
    a.valueOf(); // "abc"

### Natives as Constructor

    var a = new Array( 1, 2, 3 );
    a; // [1, 2, 3]
    
    var c = new Object();
    c.foo = "bar";
    c; // { foo: "bar" }
    
    var h = new RegExp( "^a*b+", "g" );
    var i = /^a*b+/g;

## Chapter 4: Coercion

**Type casting**: converting a value from one type to another explicitly

`var a = String(42)`

**Coercion**: converting a value from one type to another implicitly

`var a = 42 + "";`

### Abstract Value Operations

- `**toString()`:**

        var a = [1,2,3];
        
        a.toString(); // "1,2,3"

- `**Number()**`:

        Number(null)      // 0
        Number(undefined) // NaN
        Number("43")      // 43
        Number(true)      // 1
        Number(false)      // 0

- `**Boolean()**`:

        var a = new Boolean( false );
        var b = new Number( 0 );
        var c = new String( "" );
        
        Boolean( a ); // true
        Boolean( b ); // true
        Boolean( c ); // true
        /* The boolean constructor is checking if a,b or c are
         objects, which they are, hence, they are true! */

### **Explicitly: Parsing Numeric Strings using `parseInt(..)` or `parseFloat(..)`**

    var a = "42";
    var b = "42px";
    
    Number( a );	// 42
    parseInt( a );	// 42
    
    Number( b );	// NaN
    parseInt( b );	// 42

## Chapter 5: Grammar

### Statement v/s Expressions

Statement is complete formation of one or more expressions. `var a = 3 * 6;` is a statement, while `3 * 6` is an expression.

*Note: Most of the grammar rules in JS should be straightforward. You can read more about it [here](https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/types%20%26%20grammar/ch5.md).*