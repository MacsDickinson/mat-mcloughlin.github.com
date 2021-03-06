---
layout: post
title: Alternatives on the Javascript Constructor Pattern
post_url: http://mat-mcloughlin.github.io
---

Had a discussion in [@jabbr](https://jabbr.net/) recently about different ways of implementing the constructor/prototype pattern in javascript and a couple popped up. 

##1. Implementing the constructor as a function and all the prototype methods in one go:

{% highlight js linenos %}
function MyObject(foo, bar) {
  this.foo = foo;
  this.bar = bar;
};

MyObject.prototype = {
  constructor: MyObject,
  someFunction: function() {
    console.log(foo + " and " + bar);
  },
  someOtherFunction: function() {
  }  
};
{% endhighlight %}

##2. Implementing the constructor as a function and the prototype methods individually:

{% highlight js linenos %}
function MyObject(foo, bar) {
  this.foo = foo;
  this.bar = bar;
};
	
MyObject.prototype.someFunction = function(){
  console.log(this.foo + " and " + this.bar);
};

MyObject.prototype.someOtherFunction = function(){
};
{% endhighlight %}

##3. Enclosing all methods within a function and returning the constructor:

{% highlight js linenos %}
var MyObject = (function(){
  var EXAMPLE_CONSTANT = 4;

  function constructor(foo, bar){
    this.foo = foo;
    this.bar = bar;
  }
	
  constructor.prototype.someFunction = function(){
    console.log(this.foo + " and " + this.bar);	
  };
	
  constructor.prototype.someOtherFunction = function(){
  };
	
  return constructor;
})();
{% endhighlight %}

I like the final method best it encapsulates all the prototype methods and allows you to have private constants (as shown in the example).

Couple of things to bear in mind when using these methods:

- Always capitalise the first letter of the your function. This tells people that it needs to be called with the new keyword.
- Always use the new keyword. If you forget it then 'this' will be the global object instead of the instantiated object as you were expecting.
- Prototype methods are always public. If you need things to be private your better looking at the revealing module pattern.

If you want to be extra careful and make sure that the new keyword is used you can check this is an instance of the constructor:

{% highlight js linenos %}
if (!(this instanceof Constructor)){
  console.log('throw some kind of error');
}
{% endhighlight %}