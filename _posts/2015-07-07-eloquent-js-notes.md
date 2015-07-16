---
layout: post
title: Eloquent Javascript Notes
cover: cover.jpg
category: Notes
tags: [javascript, notes]
---

## Table of Contents
- [Chapter 3 - Function](#chapter-3)
    -   [Parameters and Scope](#parameters-and-scope)
    -   [Functions as Values](#functions-as-values)
    -   [Declaration](#decalration)
    -   [Closure](#closure)
    -   [Functions and Side Effects](#functions-and-side-effects)
- [Chapter 4 - Data Structures](#chapter-4)
    -   [Properties](#properties)
    -   [Mutability](#mutability)
    -   [The arguments object](#the-arguments-object)
- [Chapter 5 - Higher-Order Functions](#Chapter-5)
    +   [Array Higher-Order Functions](#array-higher-order-functions)
    +   [call(), bind() and apply()](#call-bind-and-apply)
    +   [Borrowing Functions with Apply and Call (A Must Know)](#borrowing-functions-with-apply-and-call-a-must-know)
- [Chapter 9 - Regular Expressions](#regular-expressions)
    +   [Create a Regular Expression](#create-a-regular-expression)

-------------------------------------------------------------------

### Chapter 3 
### Functions

#### Parameters and Scopes

An important property of functions is that the variables created inside of them, *including their parameter*, are **local** to the function.
>applies only to parameters and to variables declared with the `var` keyword

All variables from blocks *around* a function's definition are visible - lexical scoping

**functions are the only thing that create a new scope**

#### Functions as Values

Discussed in chapter 7

#### Declaration

functions are conceptually move to the top of their scope and can be used by **all** the code in that scope

#### Closure

Local variables really are re-created for every call
This feature - being able to reference a specific instance of local variables in an enclosing function is called **closure**.

```javascript
function multiplier(factor){
    return function(number){
        return number * factor;
    }
}

var twice = multiplier(2);
console.log(twice(5));
//parameter is itself a local variable
```

#### Functions and Side Effects

Functions
- side effects
- return value

pure function : a value-production function that not only has no side effects but also doesn't rely on side effects from other code

------------------------------------------------------

### Chapter 4
### Data Structures: Objects and Arrays

#### Properties

value.x and value[x]
>The part after the dot must be a **valid variable name**

#### Mutability

numbers, strings and booleans are all *immutable*
Object values can be modified

```javascript
var object1 = {value:10};
var object2 = object1;
var object3 = {value:10};

console.log(object1 == object2)
//true
console.log(object1 == object3)
//false

object1.value = 15
console.log(object2.value);
//true
```

`object1` and `object2` are references to the same object

#### The arguments object

whenever a function is called, a special variable named arguments is added. It refers to an object that holds all of the arguments passed to the function.

----------------------------------------------

### Chapter 5
### Higher-Order Functions

#### Abstraction

#### Array Higher-Order Functions

`forEach()`


`filter()`

builds up a new array with only the elements that pass the test, this function is *pure*, does not modify the array it is given

`map()`

The map method transforms an array by applying a function to all of its elments and building a new array from the returned values

`reduce()`

Combine into one value

```
[0, 1, 2, 3, 4].reduce(function(previousValue, currentValue, index, array) {
  return previousValue + currentValue;
});
```

`every()`
`some()`

#### call(), bind() and apply()

##### Bind()
We use the Bind () method primarily to call a function with the *this* value set explicitly. 

```javascript
var user = {
    data        :[
        {name:"T. Woods", age:37},
        {name:"P. Mickelson", age:43}
    ],
    clickHandler:function (event) {
        var randomNum = ((Math.random () * 2 | 0) + 1) - 1; // random number between 0 and 1​
​
        // This line is adding a random person from the data array to the text field​
        $ ("input").val (this.data[randomNum].name + " " + this.data[randomNum].age);
    }
​
}
​
​// Assign an eventHandler to the button's click event​
$ ("button").click (user.clickHandler.bind(user));
```

We can use Bind() to borrow method or curry method.

```javascript
function greet (gender, age, name) {
    // if a male, use Mr., else use Ms.​
    var salutation = gender === "male" ? "Mr. " : "Ms. ";
​
    if (age > 25) {
        return "Hello, " + salutation + name + ".";
    }
    else {
        return "Hey, " + name + ".";
    }
}

// So we are passing null because we are not using the "this" keyword in our greet function.​
var greetAnAdultMale = greet.bind (null, "male", 45);
​
greetAnAdultMale ("John Hartlove"); // "Hello, Mr. John Hartlove."​
​
var greetAYoungster = greet.bind (null, "", 16);
greetAYoungster ("Alex"); // "Hey, Alex."​
greetAYoungster ("Emma Waterloo"); // "Hey, Emma Waterloo."​

```

##### Apply() and call()

they allow us to borrow functions and set the this value in **function invocation**. In addition, the `apply()` function in particular allows us to execute a function with an array of parameters, such that each parameter is passed to the function individually when the function executes.

The apply and call methods are almost identical when setting the this value except that 
- you pass the function parameters to **apply () as an array**
- while you have to **list the parameters individually to pass them** to the call () method.

#### Borrowing Functions with Apply and Call (A Must Know)

##### Borrowing Array Methods

An **array-like object** is an object that has its keys defined as non-negative integers. It is best to specifically add a length property on the object that has the length of the object, since the a length property does not exist on objects it does on Arrays.

```javascript
// An array-like object: note the non-negative integers used as keys​
var anArrayLikeObj = {0:"Martin", 1:78, 2:67, 3:["Letta", "Marieta", "Pauline"], length:4 };

// First parameter sets the "this" value​
var newArray = Array.prototype.slice.call (anArrayLikeObj, 0);
​
console.log (newArray); // ["Martin", 78, 67, Array[3]]​
​
// Search for "Martin" in the array-like object​
console.log (Array.prototype.indexOf.call (anArrayLikeObj, "Martin") === -1 ? false : true); // true​
​
// Try using an Array method without the call () or apply ()​
console.log (anArrayLikeObj.indexOf ("Martin") === -1 ? false : true); // Error: Object has no method 'indexOf'​
​
// Reverse the object:​
console.log (Array.prototype.reverse.call (anArrayLikeObj));
// {0: Array[3], 1: 67, 2: 78, 3: "Martin", length: 4}​

```

##### Borrowing String Methods with Apply and Call

##### Borrowing other methods

```javascript
var gameController = {
    scores  :[20, 34, 55, 46, 77],
    avgScore:null,
    players :[
        {name:"Tommy", playerID:987, age:23},
        {name:"Pau", playerID:87, age:33}
    ]
}
​
var appController = {
    scores  :[900, 845, 809, 950],
    avgScore:null,
    avg     :function () {
​
        var sumOfScores = this.scores.reduce (function (prev, cur, index, array) {
            return prev + cur;
        });
​
        this.avgScore = sumOfScores / this.scores.length;
    }
}
​
// Note that we are using the apply () method, so the 2nd argument has to be an array​
appController.avg.apply (gameController);
console.log (gameController.avgScore); // 46.4​
​
// appController.avgScore is still null; it was not updated, only gameController.avgScore was updated​
console.log (appController.avgScore); // null​
```

##### Use Apply () to Execute Variable-Arity Functions

```javascript
// We can pass any number of arguments to the Math.max () method​
console.log (Math.max (23, 11, 34, 56)); // 56
var allNumbers = [23, 11, 34, 56];

// We cannot pass an array of numbers to the the Math.max method like this​
console.log (Math.max (allNumbers)); // NaN
var allNumbers = [23, 11, 34, 56];

// Using the apply () method, we can pass the array of numbers:​
console.log (Math.max.apply (null, allNumbers)); // 56
```

------------------------------------------------------

### Chapter 9
### Regular Expressions

#### Create a regular expression
    
1. with RegExp constructor

    `var re1 = new RegExp("abd");`

2. written as a literal value

    `var re2 = /abc/;`

some chars like `+`, have special meanings in RegExp. 
>when in doubt, just put a backslash before any character that is not a letter, number, or whitespace.

#### Testing for Matches

`.test()` return a Boolean telling you whether the string contains a match of the pattern in the expression.

#### Matching a Set of Characters

`[0-9]`, a `-` between two cahracters can be used to indiate a range of chars

built-in shorcuts:
- \d    any digit character
- \w    any alphanumeric character
- \s    any whitespace character(space, tab, newline and similar)
- \D    a character that is not a digit
- \W    a nonalphanumeric character
- \S    a nonwhitespace character
- .     any character except for newline

These backslash codes can be used inside [], like [\d.], means any digit or a period character. But the . itself, lost the special meaning.

To *invert* a set of characters add a ^, to express that you want to match any character **except** the ones in the sets.

#### Repeating Parts of a Pattern

- +     >= 1
- *     >= 0
- ?     0 or 1
- {4}   4 times
- {2, 4}    2 - 4 times
- {, 5}     0 - 5 times
- {5, }     >= 5 times
 
#### Grouping Subexpressions

```
var cartoonCrying = /boo+(hoo+)+/i
cartoonCrying.test("Boohoooohoohooo") -- true
```

- The 1st and 2nd `+` apply only to the second `o` in `boo` and `hoo`
- The 3rd `+` applies to the whole group `(hoo+)`
- The `i` makes this regexp case insensitive

#### Matches and Groups

`exec()` will return `null` if no match was found and return an object with information about the match otherwise

```
var match = /\d+/.exec("one two 100");
console.log(match);
// -> ["100"]
console.log(match.index);
// -> 8
```

String has a `match()` method behaves similarly

When the regexp contains *subexpressions* grouped with parenthese, the text that matched those groups will also show up in the array.

```
var quotedText = /'([^']*)'/;
quotedText.exec("She said 'hello'");
// -> ["'hello'", "hello"]
```

If a group does not end up matched at all, its position in the array would hold `undefined`. If it's matched multiple times, only the last match ends up in the array.

#### Word and String Boundaries

If we want to enforce that the match must span the whole string, we can add the markers ^ and $.

**word boundary** - \b
a word char on one side and a nonword char on the other

####Choice Patterns

The Pipe Character `|` denotes a choice between the pattern to its left and the pattern to its right.

```
var animalCount = /\b\d+ (pig|cow|chicken)s?\b/;
animalCount.test("15 pigs");
// -> true
animalCount.test("15 pigchickens");
// -> false
``` 

#### Replace Method

String `replace` method, the first argument can be a regexp

```
"Borobudur".replace(/[ou]/, "a")
"Borobudur".replace(/[ou]/g, "a")
```

The real power of using regexp with `replace` comes from the fact that we can refer back to matched groups in the replace strings.

```
"Hopper, Grace\n McCarthy, John".replace(/([\w ]+), ([\w ]+)/g, "$2 $1")
// Grace Hopper
// John McCarthy
```

$1 - $9, the whold match can be refered to as $&

It's also possible to pass a function to the 2nd argument to `replace`. The function will be called with the matched groups as arguments, and its return value will be inserted into the new string.

```
var s = "the cia and fbi";
s.replace(/\b(fbi|cia)\b/g, function(str){
   return str.toUpperCase(); 
});
// -> the CIA and FBI
```

#### Greed

```
function stripComments(code){
    return code.replace(/\/\/.*|\/\*[^]*\*\//g, "");
}
stripComments("1 /* a */+/* b */ 1");
// -> 1 1 (wrong)
```

The repetition operator(+, *, ?, and {}) are greedy, meaning they match as much as they can and backtrack from there. If you put a question mark after them, they become nongreedy and start by matching as little as possible.

#### Dynamically creating regexp objects

Use the `RegExp` constructor

```
var name = "harry";
var text = "harry is a suspicious character";
var regexp = new RegExp("\\b(" + name + ")\\b", "gi");
text.replace(regexp, "_$1_");
```

>use 2 backslashes because we are writing them in a normal string
>the second argument is the options

#### Search 

`search` method can take a regexp as argument and returns the first index on which the expression was found, or -1 when it's not found

#### lastIndex

Regular expression objects have properties.
- source: the string that expression was created from
- lastIndex: where the next match will start

>must have global(g) option enabled

```
var pattern = /y/g;
pattern.lastIndex = 3;
var match = pattern.exec("xyzzy");
match.index
// 4
pattern.lastIndex
// 5
```

the call to `exec` would automatically updates the `lastIndex` property to point after the match, if no match was found, `lastIndex` is set back to 0

#### Looping Over Matches

looping through all occurances in a string, using `lastIndex` and `exec`

```
var input = "A string with 3 numbers in it... 42 and 88.";
var number = /\b(\d+)\b/g;
var match;
while (match = number.exec(input))
  console.log("Found", match[1], "at", match.index);
// → Found 3 at 14
//   Found 42 at 33
//   Found 88 at 40
```

