---
title: Spread Operator
updated: 2016-01-24
---

### What is it?

Spread operator spreads the contents of an object or array into individual components, sort of like flattening the array by one level.

Here's a simple example, with the intention of finding the maximum of an array of numbers

```javascript
Math.max(1,2,3,4,5);

var array = [1,2,3,4,5];
Math.max(...array);
```

Spread operator is new to ES6. So what can we do while ES6 is not widely supported? Use apply!

```javascript
var array = [1,2,3,4,5];
Math.max.apply(null, array);
```