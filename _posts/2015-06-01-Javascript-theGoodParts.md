---
layout: post
title: Javascript Good Parts
cover: cover.jpg
date:   2015-06-01 12:00:00
categories: posts
---


Part 1 Good Parts
----------------------------
Javascript is getting more and more popular now

>Good Parts:
>	- function
>	- loose typing
>	- dynamic objects
>	- object literal notation
>Bad Parts:
>	- Global variables(**They are evil!**)

Part 2 Grammar
-----------------------------
###Comment
	
	- //  (recommended)
	- /* */  (Not safe because it can occur in regex)

###Number

JS only has a single number type. Internally, it is a 64-bit floating point. So 1 and 1.0 are the same value.
isNaN() function to detect NaN.

###Strings

All characters are 16 bits wide. Immutable.

Part 3 Objects
-----------------------------
###JS types

Immutable
	- Number
	- String
	- boolean
null
undefined

objects
	- property, can be any string, including **empty string**
	- value, can be any JS value except **undefined**

###Retrieval

retrieve a nonexistent member
```javascript
flight.status // undefined
```
The || operator can be used to fill in default values:
```javascript
var middle = stooge["middle-name"] || "(none)";
```
Attempting to retrieve valus from **undefined** will throw a **TypeError** exception. Can be guarded against with && operator:
```javascript
flight.equipment && flight.equipment.model
```

###Reference
Objects are passed around by reference. They are **never copied**.
```javascript
    var x = another_x;
    x.nickname = "nick";
    var name = another_x.nickname;
    // name is "nick" because x and another_x are references to the same obj
```  
Initialization
```javascript
    var a = {}, b = {}, c = {}; //a, b and c are differenct obj
	var a = b = c = {}; //a, b and c refer to the same empty obj
```
###Prototype
The prototype link is used only in retrieval, has no effect on updating.
When retrieve a property, it would search the prototype link.

###Reflection
typeof 
> **Note:**
> any property on the protorype chain can produce a value

hasOwnProperty
	- it does not look at the prototype chain

###Enumeration
the **for in** statement can loop over all the property names in an obj. It will include all properties -- including functions and prototypes properties. So it is necessary to **filter** out the values.
	- hasOwnProperty
	- typeof 
```javascript
	var name;
	for(name in another_stooge){
		if(typeof(another_stooge[name]) !== 'function'){
			document.writeln(name + ': ' + another_stooge[name]);
		}
	}
```
> **Note:**
> There is no guarantee on the order of properties.

###Delete
It will remove a property from the object **if it has one**. It will **not** touch any  of the objects in the prototype linkage.

###Global Abatement
Avoid global variable. Skip this part, use module pattern.
