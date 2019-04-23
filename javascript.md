# Javascript Tutorial
## Beginner
You can skip this if you have done Java, or have a quick read if you want to checkout the syntax.

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
Creating objects in Javascript is as simple as creating structs in C (if you are familiar with it). You can declare and/or initialise variables and functions within the objects.
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



