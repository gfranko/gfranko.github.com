---
layout: post
title: "I Love My IIFE"
date: 2013-01-05 14:29
comments: true
categories: [JavaScript, Design Patterns]
description: "Write more readable Immediately Invoked Function Expression's (IIFE's)"
keywords: "Greg Franko, JavaScript Design Patters, IIFE, Immediately Invoked Function Expression"
---

An **IIFE**, or [Immediately Invoked Function Expression](http://benalman.com/news/2010/11/immediately-invoked-function-expression/), is a common JavaScript design pattern used by most popular libraries (jQuery, Backbone.js, Modernizr, etc) to place all library code inside of a local scope.  

<!-- more -->

<a href="blog/i-love-my-iife">{% img center /images/JavaScript-logo-small.png %}</a>

In the words of [James Padolsey](http://james.padolsey.com/javascript/iife-argument-madness/):

> An IIFE protects a module's scope from the environment in which it is placed.

There are more benefits to using IIFE's than just local scoping, but before we talk about some of the other benefits, let's take a look at some code to see how to create a basic IIFE:

{% codeblock lang:js %}
(function(){
  // my special code
}());
{% endcodeblock %}

The above **IFFE** is just an anonymous function (no name attached to it) that is wrapped inside of a set of parentheses and called (invoked) immediately.  Let's break this down step by step.

###The Anonymous Function:

{% codeblock lang:js %}
// Anonymous function
function(){
  // my special code
}(); // The parentheses make sure the anonymous function gets called immediately
{% endcodeblock %}

A function, without a name, is created and then called (invoked) immediately via the `()` at the very end.

###The Parentheses

You might be wondering why we need to wrap the immediately invoked anonymous function inside of parentheses.  Before I explain why, try the above code inside of a console window.  Get a syntax error?

By wrapping the anonymous function inside of parentheses, the JavaScript parser knows to treat the anonymous function as a function expression instead of a function declaration.  A function expression can be called (invoked) immediately by using a set of parentheses, but a function declaration cannot be. 

<!-- more -->

**Note**: In case you forget the difference between JavaScript function expressions and function declarations:

_Function Expression_ - `var test = function() {};`

_Function Declaration_ - `function test() {};`

Since the anonymous function within our IIFE is a function expression and is not being assigned to a global variable, no global property is being created, and all of the properties created inside of the function expression are scoped locally to the expression itself.

###Other Benefits of using an IIFE

Whew, now that we understand the basic concept of an IIFE, let's determine some other benefits of using them in your code.

####Reduce scope lookups

A small performance benefit of using IIFE's is the ability to pass commonly used global objects (window, document, jQuery, etc) to an IIFE's anonymous function, and then reference these global objects within the IIFE via a local scope.

**Note**: JavaScript first looks for a property in the local scope, and then it goes all the way up the chain, last stopping at the global scope.  Being able to place global objects in the local scope provides faster internal lookup speeds and performance.

Let's look at a code example of passing global objects to an IIFE:

{% codeblock lang:js %}
// Anonymous function that has three arguments
function(window, document, $) {

  // You can now reference the window, document, and jQuery objects in a local scope

}(window, document, window.jQuery); // The global window, document, and jQuery objects are passed into the anonymous function
{% endcodeblock %}

####Minification Optimization

Another small performance benefit of using IIFE's is that it helps with minification optimization.  Since we are able to pass global objects into the anonymous function as local values (parameters), a minifier is able to reduce the name of each global object to a one letter word (as long as you don't already have a variable of the same name).  Let's look at the above code sample after minification:

{% codeblock lang:js %}
// Anonymous function that has three arguments
function(w, d, $) {

  // You can now reference the window, document, and jQuery objects in a local scope

}(window, document, window.jQuery); // The global window, document, and jQuery objects are passed into the anonymous function
{% endcodeblock %}

**Note**:  Another added benefit (when using jQuery), is that you can freely use the `$` without worrying about other library conflicts, since you passed in the global `jQuery` object and scoped it to the `$` as a local parameter.

###Readability

One of the con's with using IIFE's is readability.  If you have a lot of JavaScript code inside of an IIFE, and you want to find the parameters that you are passing into an IIFE, then you need to scroll all the way to the bottom of the page.  Luckily, there is a more readable pattern that I have begun to use:

{% codeblock lang:js %}
(function (library) {

    // Calls the second IIFE and locally passes in the global jQuery, window, and document objects
    library(window, document, window.jQuery);

}

// Locally scoped parameters 
(function (window, document, $) {

// Library code goes here

}));
{% endcodeblock %}

This IIFE pattern allows you see what global objects you are passing into your IIFE, without you having to scroll to the very bottom of a potentially very long file.  Happy IIFE'ing!