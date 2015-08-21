---
layout: post
title: Javascript Style Guide Notes
cover: cover.jpg
category: Programming Style
tags: [javascript, programming style]
---

>This is not a completed style guide, just write down things I think worth mentioning

### Indentation

Each indentation is made up of fours spaces. **Do not use tabs**

### Line lenght

Each line should be on longer than 80 characters. If a line goes longer than 80 characters, it should be wrapped after an operator.
The following line should be indented *two levels*(8 chars).

```javascript
// Good
doSomething(arg1, arg2, arg3, arg4,
        arg5)

// Bad: Following line only indented four spaces
doSomething(arg1, arg2, arg3, arg4,
    arg5)

```

### Primitive Literals

`String` should always use double quotes and should always appear on a single line

`Number` should be written as `decimal integers`, `e-notation integers`, `hexadecimal` and `floating-point decimals`

`null` should be used only in the following situations:

- to initialize a variable that may later be assigned an object value
- to compate against an initialized variable that may or may nor have an obejct value
- to pass into a function where an object is expected
- to return from a function where an object is expected

`undefined` should never be used. only check if a variable has been defined.

### Operator Spacing

Operators with 2 operands must be preceded and followed by a single space.
include `assignments` and `logical operators`.

### Parentheses Spacing

no space after opening paren or before the closing paren

```javascript
// Good
var found = (values[i] === item);

//Bad: extra space
var found = ( values[i] === item );
```

### Object Literals

- the opening brace should be on the same line as the containing statement
- each property-value pair should be indented one level
- property name should be *unquoted*
- if the value is a function, should wrap under the property name, and have a blank line both before and after the function
- The closing brace should be on a separate line

```javascript
// Good
var object = {
    
    key1: value1,
    key2: value2,

    func: function(){
        //do something
    },

    key3: value3
};
```

### Comments

Use comments when:

- Code difficult ot understand
- The code might be mistaken for an error
- Browser-specific code is necessary but not obvious
- Documentation generation is necessary for an `object`, `method`, or `property`

#### Single-Line Comments

- Separate line

blank line before single-line comment

- comments at the end of a line

ensure that there is at least one indentation level between code and comment

#### Multiline Comments

- line break
- * indention
- blank line before comments

```javascript
// Good
if (condition){
    
    /*
     * if you made it here
     * then all security checks passed
     */
    allowed();
}
``` 

#### Variable Declarations

All variables should be declared before they are used.

```javascript
// Good
var count = 10,
    name  = "Nicholas",
    found = false,
    empty;
```

local variables should be declaried at the first line of function

### Function Declarations

Functions should be declared before they are used.

- no space between function name and the opening parenthesis.
- one space between the closing parenthesis and the right brace

```javascript
// Good 
function doSomething(arg1, arg2) {
    
}
```

Inner function declared immediately after the `var` statement.

```javascript
// Good
function outer() {
    
    var count = 10,
        name  = "Nicholas",
        found = false,
        empty;

    function inner() {
        // code
    }
}
```

Anonymous functions

```javascript
// Good
object.method = function() {
    // code
}
```

Immediately invokled function should surround the entire function call with parenthese

```javascript
// Good
var value = (function() {
    
    // function body

    return {
        message: "Hi"
    }
}());
```

### Naming

>Do not use underscores

##### Variable Name 

- should be formatted in Camel Case
- the first word should be a noun

#### Function Name

- should be formatted in Camel Case
- the first word should be a verb

#### Constructor Function

- Camel Case, first letter uppercase
- should begin with a nonverb

#### Constant

All Uppercase letters

#### Object Properties

private property or method should be prefixed with an _ character

### Strict Mode

Strict mode should be used only inside of functions

If wanna apply to multiple functions, use immediate function invocation

```javascript
(function() {
    "use strict";

    function doS() {

    }

    function doSE() {

    }
}())
```

### Ternary Operator

The ternary operator should be used only for assigning values conditionally and never as a shortcut for an `if` statement.

```javascript
// Good
var value = condition ? value1 : value2;
```

### For Statement

Variables should not be declared in the initialization section of a `for` statement.

```javascript
// Good
var i,
    len;

for (i=0, len=10; i < len; i++) {
    // code
}
```

### Switch Statement

Each `case` is indented one level and should be preceded by a single empty line

```javascript
//Good

switch (value) {
    case 1:
        /* falls through */

    case 2:
        doSomething();
        break;

    case 3:
        return true;

    default:
        throw new Error("This shouldn't happen.");
}
```

