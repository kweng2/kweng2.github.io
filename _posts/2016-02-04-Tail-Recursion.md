---
title: Tail Recursion Optimization Applied
updated: 2016-02-04
---

ES6 supports tail call optimization. How can we take advantage of this and what does it enable?

### What is it?

Suppose we are trying to find the max call stack size

```javascript
var findMaxStackSize = function (n) {
    return findMaxStackSize(n+1);
};
findMaxStackSize(0);
```

Each time ```findMaxStackSize``` is called, since it is nested within a function call, it gets added to the call stack. Eventually, the call stack will be full and throw an error.

### ES6 To The Rescue

ES6 notices that the function call of ```findMaxStackSize``` is the last operation in the current function call, and therefore no longer needs to keep track of the current execution context. As a result, when the next ```findMaxStackSize``` is called, the call stack does not increase, and therefore this function can run forever!

### So What?

A particular application is using a recursed [redis list](http://redis.io/commands/BLPOP) and its [blocking pop](http://redis.io/topics/data-types) as a load balancer. With this tail call recursion support, this blocking pop operation can run indefinitely!

Here's what that would look like:

```javascript
var client = require('redis').createClient();

var keepListening = function () {
  client.blpop('LIST_NAME', TIMEOUT, function(err, response) {
    // Do something with the popped element, response
    console.log(response);
    keepListening();
  });
}

keepListening();
```

This code block uses the ```redis``` node module and its invocation of the blocking operation. This function will log the element being popped from the redis list called ```'LIST_NAME'```, then call itself. Since it is the last operation in its current execution context, tail-recursion optimized interpreter will NOT add to the call stack, thereby allowing this function to run indefinitely.

This is just one use case. Let me know if you find another cool use case.