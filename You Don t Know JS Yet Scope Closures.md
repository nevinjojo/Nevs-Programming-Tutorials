# You Don't Know JS Yet: Scope & Closures

## Chapter 1: What's the Scope?

### Compiled vs. Interpreted

- Compilation is a process that turns the code into a set of instructions the computer can understand/execute.
- Interpretation also process your program into machine-understandable instructions. But the process model is different. Unlike a program being compiled all at once in Java, source code is ***processed and executed*** line by line in interpreted language.
- **JS code is processed in two phases: parsing/comilation first, then execution.**
- **scope is determined during compilation (or interpreation)**

### Compiler Speak

- Other than declarations, all occurences of [variables/identifiers](km\) in a program serve in one of the two roles: **either they're the target of an assignment or they're the source of a value (i.e. you either about to have a value assinged to you, or you have a value already assigned to you)**
- **Target** is usually the LHS of a declaration, while **source** is the RHS
    - In `student = [1,2,3]`, target = `student` and source = `[1,2,3]`
    - In `for (let student of students)` , target = `student` and source = `students`
- **Source is *typically* defined as the program is compiled**, and should not be affected during runtime
    - **Exception**: `eval("var oops = 'ugh';");` will compile and execute code during runtime that was not parsed during compilation

## Chapter 2: Illustrating Lexical Scope

### Lexical Scope

It is a type of scope definition where every inner level has access to it's outer levels ⇒ a variable defined outside a function can be accessible inside another function

### How scope works

Scope bubbles are determined during compilation based on where the functions/blocks of scope are written, the nesting inside each other, and so on. **References to variables/identifiers are allowed if there's a matching declaration either in the current scope, or any scope above/outside the current scope, but not with declarations lower/inside current scope.**

![Fig 2.0](You%20Don%20t%20Know%20JS%20Yet%20-%20Media/Screen_Shot_2020-03-21_at_9.49.26_AM.png)

1. Global scope
2. function scope
3. Loop scope

### Accidental global variable

If the variable is a target and strict-mode is not in effect, a confusing and surprising legacy behavior kicks in. The troublesome outcome is that the global scope's Scope Manager will just create an accidental global variable to fulfill that target assignment!

![Fig 2.1](You%20Don%20t%20Know%20JS%20Yet%20-%20Media/Screen_Shot_2020-03-21_at_9.55.14_AM.png)

## Chapter 3: The Scope Chain

The JS engine does not need to lookup through a bunch of scopes to figure out the scope bukcet of a variable. This information is already known.

### Shadowing

When you have two variables with the same name, each in different scopes (one in global scope and another in functional scope), **the functional scope variable shadows the global scope variable**.

### Function Declaration v/s Function Expression

    // Function Declaration
    function askQuestion() {
        // ..
    }
    
    // Function Expression
    var askQuestion = function(){
        // ..
    };

### Function Name Scope

Function declaration will create an identifier in the enclosing scope (parent scope). However function expressions work differently:

    var askQuestion = function ofTheTeacher(){
        // ..
    };
    
    var askQuestion = () => {
    
    }

`askQuestion` belongs to parent scope, while `ofTheTeacher` belongs to the function scope.

## Chapter 4: Around the Global Scope

Even though JS programs consists of smaller programs made up of many different JS files, global variables works between these programs. Global variables work in the following ways:

- Through `import` statements
- If you're using a bundler in your build process, all files are typically concatenated together beofre delivering to JS engine and browsers.

### Where exactly is global scope

**Browser "Window":** In the case of a browser, the global object is the window. 

**Web Workers:** Web Workers are a **web platform extension** on top of browser-JS behavior, which **allows a JS file to run in a completely separate thread (operating system wise) from the thread that's running the main JS program.** Web workers does not share global scope with the main JS program. 

**Developer Tools:** It emulates the behaviour of the main program

**ES Modules (ESM):** Modules are imported to the main program and does not allow it's personal variables to be used as a global variable unless specified.

## Chapter 5: The (Not So) Secret Lifecycle of Variables

### When Can I Use a Variable/Function?

You can use a function even before it is declared/created. Whaaaaat? This is beacase technically, **every identifier is "*created"* at the beginning of the scope it belongs to. This is called hoisting.** [Hoisting works on function declarations, but not function expressions.](s) 

    greeting();
    // Hello! <- This works!!
    
    function greeting() {
        console.log("Hello!");
    }
    
    greeting();
    // TypeError <- This does not work
    // 'greeting' variable is found, but not greeting function
    
    var greeting = function greeting() {
        console.log("Hello!");
    };
    
    ------------------------------
    var studentName = "Frank";
    console.log(studentName);
    // Frank
    
    var studentName; // <- This is a no-op, i.e pointless statment that doesn't do anything
    console.log(studentName);   // Frank
    
    ------------------------------
    var studentName = "Frank";
    console.log(studentName);
    // Frank
    
    var studentName = undefined; // <- This is a no-op, i.e pointless statment that doesn't do anything
    console.log(studentName);   // undefined

 **JS doesn't want us to re-declare a variable within the same scope.**

## Chapter 6: Limiting Scope Exposure

### Least Exposure

The Principle of Least Privilege (POLP) expresses a defensive posture to software architecture: *components of the system should be designed to function with least privilege, least access, least exposure*. This will avoid problems such as:

- **naming collision**: two variables with same popular keywords being used within the same scope.
- **Unexecpected behaviour**: problems that arises when developers use 'supposed-to-be private' variables in an unintended way.
- **Unintended Dependency**: Unnecessarily making a variable public will allow developers to depend on that variable for their modules causing future issues with refactoring your code without affecting their code.

### Where to `var` and `let`?

Any variable that is needed across all functions should be declared so that the usage is obvious. **Reserve `var` for only top level function scope** (`var` can still be used inside a function block, as it is functional-scoped). **Use `let` in every other case.**

## Chapter 7: Using Closures

**Closure is a behavior of [functions](m) and only functions**. If you aren't dealing with a function, closure does not **apply**. Only functions have closure. 

Example:

    // outer/global scope: RED(1)
    
    function lookupStudent(studentID) {
        // function scope: BLUE(2)
    
        var students = [
            { id: 14, name: "Kyle" },
            { id: 73, name: "Suzy" },
            { id: 112, name: "Frank" },
            { id: 6, name: "Sarah" }
        ];
    
        return function greetStudent(greeting){
            // function scope: GREEN(3)
    
            var student = students.find(
                student => student.id == studentID
            );
    
            return `${ greeting }, ${ student.name }!`;
        };
    }
    
    var chosenStudents = [
        lookupStudent(6),
        lookupStudent(112)
    ];
    
    // accessing the function's name:
    chosenStudents[0].name;
    // greetStudent
    
    chosenStudents[0]("Hello");
    // Hello, Sarah!
    
    chosenStudents[1]("Howdy");
    // Howdy, Frank!

**Closure allows `greetStudent(..)` to continue to access those outer variables even after the outer scope is finished** (when each call to `lookupStudent(..)` completes). Instead of the instances of students and studentID being Garvage Collected, they stay around in memory. At a later time when either instance of the `greetStudent(..)` function is invoked, those variables are still there, holding their current values.

### Closure and Callbacks

Closure is most commonly encountered with callbacks and eventListeners:

    function lookupStudentRecord(studentID) {
        **ajax(
            `https://some.api/student/${ studentID }`,
            function onRecord(record) {**
                console.log(
                    `${ record.name } (${ studentID })`
                );
            }
        );
    }
    
    lookupStudentRecord(114);
    // Frank (114)

Here `onRecord(..)` function is invoked at some point in the future when ajax call comes back. Furthermore, when that happens, the  `lookupStudentRecord(..)` call will long since have completed. Why then is `studentID` still around and accessible to the callback? Closure.

## Chapter 8: The Module Pattern

### What Is a Module?

A module is **a collection of related data and functions characterized by a division between private details and public accessible details**, usually called "*public API*".

- **Modules are stateful**: they keep some information over time, along with functionality to access and update the information.

    var global = 'Hello, I am a global variable :)';
    
    (function () {
      // We keep these variables private inside this closure scope
      
      var myGrades = [93, 95, 88, 0, 55, 91];
      
      var average = function() {
        var total = myGrades.reduce(function(accumulator, item) {
          return accumulator + item}, 0);
        
        return 'Your average grade is ' + total / myGrades.length + '.';
      }
    
      var failing = function(){
        var failingGrades = myGrades.filter(function(item) {
          return item < 70;});
          
        return 'You failed ' + failingGrades.length + ' times.';
      }
    
      console.log(failing());
      console.log(global);
    }());
    
    // 'You failed 2 times.'
    // 'Hello, I am a global variable :)'

### What makes something a module?

- There must be an outer scope
- The modules inner scope must have at least one piece of hidden information that represents the state for the module
- The module must return on its public API a reference to at least one function that has closure over the hidden module state

### Modern ES Modules (ESM)

- ESM files are assumed to be strict-mode, without needing `"use strict"` pragma at the top.
- ESM is file based, while modules are singletons, with everything is private by default.
