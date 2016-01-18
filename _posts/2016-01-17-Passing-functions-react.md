---
title: Passing Functions Downstream in React
updated: 2016-01-17
---

Simple example of how to pass a event handler from a parent component to a child component:

{% highlight html linenos %}
var app = React.createClass({
  handleClick: function (input) {
    alert('From the child component: ', input);
  },
  render: function () {
    return <div>
      <p>Hello World</p>
      <Button clicked={this.handleClick} />
    </div>
  }
});

var Button = React.createClass({
  clickHandler: function () {
    this.props.clicked('Hello from Button');
  },
  render: function () {
    return <button onClick={this.clickHandler}>
      Click Me!
    </button>
  }
});
{% endhighlight %}

When the app renders, the button also renders. When the button is clicked, here is the chain of event that cascades starting from the button's ```onClick``` definition

1. Line 22: ```this.clickHandler```
2. Line 23: ```this.props.clicked``` references ```{this.handleClick}``` from the app, per definition on line 16
4. Line 10: ```this.handleClick(input)```
5. Line 11: ```alert('From the child component: ', 'Hello from Button');```