---
title: Angular Keypress Handler
updated: 2016-01-05
---

### Basic Example
Using Angular's ```ng-keypress```:

```html
<div ng-controller="exampleCtrl">
  <input type="text" ng-keypress="pressed($event)" >
</div>
```

{% highlight javascript %}
angular.module('Example', [])
  .controller('exampleCtrl', function ($scope) {
    $scope.pressed = function ($event) {
      console.log($event.charCode);
    };
  });
{% endhighlight %}

The input text field is really convenient for the key-press event here because the input field can be "focused". To detect key press events on the window itself, the ```exampleCtrl``` must be attached to the ```<body>``` tag rather than a ```<div>``` tag. 

```html
<body ng-controller="exampleCtrl">
  <input type="text" ng-keypress="pressed($event)" >
</body>
```

This binds the keypress listener to the document body, so that when a user, who's on the page presses any keys, ```pressed($event)``` gets called.

### Caveat

Using ```ng-keypress``` may not detect arrow keys or enter keys. Use ```ng-keydown``` instead. Depending on which one is used, the ```.charCode``` for the same keys are different.

### Live Example

<a href="https://stormy-ridge-2371.herokuapp.com/" target="_blank">SNAKE</A> is a project I completed in January 2016, using MongoDB, Express, Angular, and Node. It uses ```ng-keydown``` to detect keypresses.
