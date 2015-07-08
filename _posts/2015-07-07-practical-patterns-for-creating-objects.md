---
layout: post
title: Practical patterns for createing objects
cover: cover.jpg
category: Javascript
tags: [javascript, oop]
---

### Table of contents
- [Factory Pattern](#factory-pattern)
- [Prototype Pattern](#prototype-pattern)
- [Combination of Factory/Prototype](#combination-of-constructor-prototype-pattern)


### Factory Pattern

```
function createCar(make, model, year) {
    var o = new Object();

    o.make   = make;
    o.model  = model;
    o.year   = year;
    o.sayCar = function() {
        alert('I have a ' + this.year + ' ' + this.make + ' ' + this.model + '.');
    };

    return o;
}

// create 2 car objects for John and Jane
var johnCar = createCar('Ford', 'F150', '2011'),
    janeCar = createCar('Audi', 'A4', '2007');
```

Cons
1. Efficiency, functions are copied to all new object instances
2. Type determination. new object instances are typed as object

### Constructor Pattern

```
function Fruit (theColor, theSweetness, theFruitName, theNativeToLand) {
​
    this.color = theColor;
    this.sweetness = theSweetness;
    this.fruitName = theFruitName;
    this.nativeToLand = theNativeToLand;
​
    this.showName = function () {
        console.log("This is a " + this.fruitName);
    }
​
    this.nativeTo = function () {
    this.nativeToLand.forEach(function (eachCountry)  {
       console.log("Grown in:" + eachCountry);
        });
    }
​
​
}
```

To create

```
var mangoFruit = new Fruit ("Yellow", 8, "Mango", ["South America", "Central America", "West Africa"]);
mangoFruit.showName(); // This is a Mango.​
mangoFruit.nativeTo();
```

Solves type determination
But still **ineffiecient**

### Prototype Pattern

```
function Fruit () {
​
}
​
Fruit.prototype.color = "Yellow";
Fruit.prototype.sweetness = 7;
Fruit.prototype.fruitName = "Generic Fruit";
Fruit.prototype.nativeToLand = "USA";
​
Fruit.prototype.showName = function () {
console.log("This is a " + this.fruitName);
}
​
Fruit.prototype.nativeTo = function () {
            console.log("Grown in:" + this.nativeToLand);
}
```

```
var mangoFruit = new Fruit ();
mangoFruit.showName(); //​
mangoFruit.nativeTo();
```

### Combination of Constructor/Prototype Pattern

**The most popular one**

```
// constructor function to create car objects
function Car(make, model, year) {
    this.make   = make;
    this.model  = model;
    this.year   = year;
}

// constructor prototype to share properties and methods
Car.prototype.sayCar = function() {
    alert('I have a ' + this.year + ' ' + this.make + ' ' + this.model + '.');    
};

// create 2 car objects for John and Jane
var johnCar = new Car('Ford', 'F150', '2011'),
    janeCar = new Car('Audi', 'A4', '2007');
```

1. Solves the efficiency issue by utilizing prototypal inheritance, share properties and methods on the constructor function's prototype

