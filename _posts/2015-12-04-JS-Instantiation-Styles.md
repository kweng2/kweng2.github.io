---
title: JS Functional Instantiation Styles
updated: 2015-12-04
---

The key that helps me keeps track of the four ways of functional class creation is thinking of it as an evoltuion:

functional -> functional shared -> prototypal -> pseudoclassical

The simplest and easiest to understand is functional, which look like this:

### Functional Instantiation
```javascript
var person = function(inputName, inputAge) {
  var newPerson = {
    name: name,
    age: age,
    sayHi: function() {
      return 'Hello, my name is ' + name;
    }
  };
  return newPerson
 };
```

A downside to this style is that each time a person object gets created, the sayHi method also get created. That’s not very space efficient.

A better way to do is to make the sayHi method, or any other method, be a pointer to a function, so that when multiple person objects are created, the sayHi method isn’t created multiple times, and instead, only once. Here’s an example of how to achieve this:

### Functional Shared Instantiation
```javascript
var person = function(name, age) {
  var newPerson = {
    name: name,
    age: age
  };
  extend(newPerson, sayHi);
  return newPerson;
};

var sayHi = function() {
  return 'Hello, my name is ' + this.name;
};
```

The above code uses the “functional shared instantiation” style, which allows multiple instances of person object share the sayHi method, rather than each having their own distinct, yet redundant method.
This style necessitates the use of the keyword “this” and a common function “extend”

The `extend` function is really simply telling the person function that there are more things, like methods, that aren’t defined here, and may be defined elsewhere. So Javascript has a way of adding methods to a function by using `Object.create` like below, which we call the prototypal instantiation style

### Prototypal Instantiation
```javascript
var person = function(name, age) {
  var newPerson = Object.create(personMethods);
  newPerson.name = name;
  newPerson.age = age;
  return newPerson;
};

var personMethods = {};
personMethods.sayHi = function() {
  return 'Hello, my name is ' + this.name;
};
```

Object.create effectively replaces extend in the above example. It’s important to be aware though, that Object.create and extend do very different things in the background, but for learning purposes, can be seen as an evolution from a simple way of life to a more advanced and powerful life. For me, this progression helps keep the various styles separated while diving deeper into the underlying mechanics of “this” assignment, prototype chains, etc.

The above code could’ve also been written as:

```javascript
var person = function(name, age) {
  var newPerson = Object.create(person.prototype.Methods);
  newPerson.name = name;
  newPerson.age = age;
  return newPerson;
}

var person.prototype = {};
person.prototype.sayHi = function() {
  return 'Hello, my name is ' + this.name;
}
```

Now, we notice that both the prototype and functional shared instantiation styles have a line of code to extend methods, as well as a line of code to return the newPerson. Javascript provides an even more evolved style, called pseudoclassical instantiation style, which does the method extension as well as the return for us:

### Pseudoclassical Instantiation
```javascript
var person = function(name, age) {
  this.name = name;
  this.age = age;
}

person.prototype.sayHi = function() {
  return 'Hello, my name is ' + this.name;
}
```
The above code is short and to the point, and often popular with CS ninjas who are familiar with object oriented programming.

That’s it! That’s the evolution of function instantiation styles from the primordial ooze to hyper-intelligent beings.
