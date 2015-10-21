---
layout: post
title: Javascript Scope and Closures
cover: cover.jpg
category: javascript
tags: [javascript]
---

## Table of Contents

- [Function Scope](#function-scope)
- [Inner Functions](#inner-functions)
- [Closures](#closures)
- [How closure work and are used](#how-closures-word-and-are-used)
    + [keep things private](#keep-things-private)
    + [Why closures so useful](#why-closures-so-useful)
- [The Infamous Loop Problem](#the-infamout-loop-problem)
- [Self Invoking Functions](#self-invoking-functions)

## Function Scope

Function scope means that everything between bracket operators {} is scoped **as long as it is part of a function**.

Function scope is used heavily in JavaScript. In fact, there is a common pattern to **create a function just for the purpose of scope**.

As an example, the standard advice for writing JQuery plugins is to use the following pattern:

```javascript
(function($) {
    //Do things here - they are scoped
}(JQuery))
```

## Inner Functions

```javascript
var func1 = function() { //outer function
    var msg = "foo";

    var func2 = function() { //inner function
        var msg = "bar";
        document.write(msg); //bar
    }

    func2();
    document.write(msg); //foo
}
func1();
```

an inner function has access to the variables of the outer function as long as it does not declare variables with identical names (in which case it will overwrite the variables, but **just within that function scope**).

## Closures

A variable that is assigned to a function is a structure in memory that is used to represent the function.
Some Languages like JavaScript implement a slightly more complex setup for the structure. For these languages, the structure holds both **the address of the function** and **its referencing environment**.

Having access to the referencing environment means access to certain non-local variables, for example, the variables in its outer function.

JavaScript implementors refer to the referencing environment as the function's context. When the structure that points to a function also contains its original context (referencing environment), it is called a *closure*.

![pic](http://doctrina.org/images/closure.png)

## How closures work and are used

when the inner function *lives longer than* the outer function, the inner function (i.e. the closure) will have access to state from that outer function that is not accessible anywhere else. 

Therefore closures have the ability to implement something that is taken for granted in classical object oriented languages: *Private State*.

### keep things private

private state and methods are very useful somethins. It can be achieved in JS by using a closure:

```javascript
var closure = function() {
    //This is a private variable
    var counter = 0;

    return { //This object being returned is assigned to closure
        getCounter : function() { return counter;},
        increment : function(inc) {
            if (typeof inc !== 'number')
                inc = 1;

            counter += inc;
        }
    };
}();

closure.increment(10);
alert(closure.getCounter()); //10
```

- The function is immediately invoked. we are not assigning the variable *closure* a function but the result of invoked function.
- The result assigned to *closure* is an Object

> The pattern of an inner function living longer than the outer function is the useful pattern for closures because it implements private state. The two closure functions within the object assigned to closure have access to counter. counter is private: It cannot be altered nor accessed except through the closure functions.

### why closures so useful

Javascript is asynchronicity with *callback* functions.

If the callback function is an inner function, it is a closure that is able to access variables in the outer function.

#### problem with `constructor invocation pattern`

```javascript
var createCallBack = function() { //First function
    return new function() { //Second function
        this.message = "Hello World";

        return function() { //Third function
            alert(this.message);
        }
    }
}

window.onload = createCallBack(); //Invoke the function assigned to createCallBack
```

When invoking the createCallBack variable, what gets passed to the window.onload event handler is the result of what is returned by the seond function operator, which is the code in the third function operator.  

**standard fix** use `that`

```javascript
var createCallBack = function() { //First function
    return new function() { //Second function
        var that = this;
        this.message = "Hello World";

        return function() { //Third function
            alert(that.message);
        }
    }
}
```

use closure to create seperate objects

```javascript
var createCallBack = function(init) { //First function
    var obj = { //This object is created within the first function and is accessible
                //to the second function due to the closure
        'message':init
    };

    return function() { //Second function
        alert(obj.message);
    }
}

window.addEventListener('load', createCallBack("First Message"));
window.addEventListener('load', createCallBack("Second Message"));
```

## The infamous loop problem

How many times have you created some sort of loop where you wanted to assign the value of i in some way, e.g. to an element, and found out it just returned the last value i had?

The faulty code

```javascript
function addLinks () {
	for (var i=0, link; i<5; i++) {
		link = document.createElement("a");
		link.innerHTML = "Link " + i;
		link.onclick = function () {
			alert(i);
		};
		document.body.appendChild(link);
	}
}
window.onload = addLinks;
```

it's not invoked, so the i reference to the i variable in for-loop, and it will all become 5.

What you should do instead is to create a closure.

```javascript
function addLinks () {
	for (var i=0, link; i<5; i++) {
		link = document.createElement("a");
		link.innerHTML = "Link " + i;
		link.onclick = function (num) {
			return function () {
				alert(num);
			};
		}(i);
		document.body.appendChild(link);
	}
}
window.onload = addLinks;
```

## Self Invoking Functions

Self-invoking functions are functions who execute immediately, and create their own closure.

The global object has an associated execution context. Additionally **every invocation** of a function establishes and enters **a new execution context**. The execution context is the dynamic counterpart to the static lexical scope.

The base of module pattern, can create private state and variable.



## Reference

- [why understanding scope and closures matters](http://doctrina.org/JavaScript:Why-Understanding-Scope-And-Closures-Matter.html)
- [expalining javascript scope and closures](http://robertnyman.com/2008/10/09/explaining-javascript-scope-and-closures/)
- [understanding javascript closures](https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/)
    + have good points
- [javascript is sexy](http://javascriptissexy.com/understand-javascript-closures-with-ease/)
