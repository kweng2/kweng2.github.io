---
title: Passing Functions Downstream in React
updated: 2016-01-17
---

Simple example of how to pass a event handler from a parent component to a child component:

{% highlight html %}
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

1. ```this.clickHandler``` on line 22
2. ```this.props.clicked``` on line 23 references ```{this.handleClick}``` from the app, per definition on line 16
3. ```this.handleClick(input)``` on line 10
4. ```alert('From the child component: ', 'Hello from Button');```