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

