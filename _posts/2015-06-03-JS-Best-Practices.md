---
layout: post
title: Javascript Best Practices
cover: cover.jpg
category: Programming Style
tags: [javascript, programming style]
---

### Easy, Short & Readable Names

#### Variables Name

**Hungarian notation** is a good variable name scheme to adopt.

> typeName

Examples:
- bBusy : boolean
- chInitial : char
- cApples : count of items

#### Function Name
Function name should start with a lowercase verb following 

>verbSomething

Examples:
- isOldEnough()
- getResult()

#### Class Name
the first char should be capital
Examples:
- Apple()
- Node()

### Avoid Global

#### revealing module pattern
```
myNameSpace = function(){
    var current = null;
    function init(){};
    function change(){};
    function verify(){};
    return{
        init: init,
        change: change
    }
}();
```

If you don't need any of your vars or functions to be avaiable to the outside, simply wrap the whole construct in another set of parentheses to execute it without assigning any name.
```
(function(){
    var current = null;
    function init(){}
})();
```

### Use strict

Check code quality with JSLint

### Comment

**Comment as much as needed but no more**

Use /\* \*/

### Avoid mixing with css

### Use Shortcut Notation

Examples:
```
var direction = ( x > 100 ) ? 1 : -1
```

```
var x = v || 10;
```
> this would automatically give x a value of 10 if v is not defined

### Modularize - One Function per Task

### Configuration and Translation

One of the most successful tips to keep your code maintainable and clean is to create a configuration object that contains all the things that are likely to change over time.

These include *any text used in elements you create*, *CSS class*, *ID names* and *general parameters of the interface you build*.

Examples: easy youtube player project config object
```
/*
  This is the configuration of the player. Most likely you will
  never have to change anything here, but it is good to be able 
  to, isn't it? 
*/
config = {
  CSS:{
    /* 
      IDs used in the document. The script will get access to 
      the different elements of the player with these IDs, so 
      if you change them in the HTML below, make sure to also 
      change the name here!
    */
    IDs:{
      container:'eytp-maincontainer',
      canvas:'eytp-playercanvas',
      player:'eytp-player',
      controls:'eytp-controls',

      volumeField:'eytp-volume',
      volumeBar:'eytp-volumebar',

      playerForm:'eytp-playerform',
      urlField:'eytp-url',

      sizeControl:'eytp-sizecontrol',

      searchField:'eytp-searchfield',
      searchForm:'eytp-search',
      searchOutput:'eytp-searchoutput'
    },
    /*
      These are the names of the CSS classes, the player adds
      dynamically to the volume bar in certain 
      situations.
    */
    classes:{
      maxvolume:'maxed',
      disabled:'disabled'
    }
  },
  /* 
    That is the end of the CSS definitions, from here on 
    you can change settings of the player itself. 
  */
  application:{
    /*
      The YouTube API base URL. This changed during development of this,
      so I thought it useful to make it a parameter.
    */
    youtubeAPI:'http://gdata.youtube.com/apiplayer/cl.swf',
    /* 
      The YouTube Developer key,
      please replace this with your own when you host the player!!!!!
    */
    devkey:'AI39si7d...Y9fu_cQ',
    /*
      The volume increase/decrease in percent and the volume message 
      shown in a hidden form field (for screen readers). The $x in the 
      message will be replaced with the real value.
    */
    volumeChange:10,
    volumeMessage:'volume $x percent',
    /*
      Amount of search results and the error message should there 
      be no results.
    */
    searchResults:6,
    loadingMessage:'Searching, please wait',
    noVideosFoundMessage:'No videos found : (',
    /*
      Amount of seconds to repeat when the user hits the rewind 
      button.
    */
    secondsToRepeat:10,
    /*
      Movie dimensions.
    */
    movieWidth:400,
    movieHeight:300
  }  
}
```

### Avoid Heavy Nesting

Use functions to avoid it

### Optimize Loops

#### For loop read length everytime
```
var names = ['George','Ringo','Paul','John'];
for(var i=0;i<names.length;i++){
  doSomeThingWith(names[i]);
}
```

To Fix
```
var names = ['George','Ringo','Paul','John'];
for(var i=0,j=names.length;i<j;i++){
  doSomeThingWith(names[i]);
}
```

#### Keep computation-heavy code outside
* regular expression
* DOM manipulation

### Keep DOM Access to a minimum

### Do not Trust Any Data

1. It is very important to tes the type of parameters send to your functions(using the `typeof` keyword)

```
function buildMemberList(members){
  if(typeof members === 'object' && 
     typeof members.slice === 'function'){
    var all = members.length;
    var ul = document.createElement('ul');
    for(var i=0;i<all;i++){
      var li = document.createElement('li');
      li.appendChild(document.createTextNode(members[i].name));
      ul.appendChild(li);
    }
    return ul;
  }
}
```

2. Compare and check info read from the DOM

### Do not create too much with Javascript

Using an HTML template and load it via AJAX

### Do not Use New Object

* Use {} instead of new Object()
* Use "" instead of new String()
* Use 0 instead of new Number()
* Use false instead of new Boolean()
* Use [] instead of new Array()
* Use /()/ instead of new RegExp()
* Use function (){} instead of new function()
