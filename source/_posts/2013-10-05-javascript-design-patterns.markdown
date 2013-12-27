---
layout: post
title: "JavaScript Design Patterns"
date: 2013-10-05 11:43
comments: true
categories: [JavaScript, AddThis]
description: "JavaScript design patterns used for AddThis Smart Layers"
keywords: "Greg Franko, Design Patterns, AddThis"
---

JavaScript design patterns are important for the maintainability and scalability of web applications. While working on the <a href="http://addthis.com/get/smart-layers" target="_blank">AddThis Smart Layers</a> product, the team focused on writing DRY (Don’t Repeat Yourself), consistent, and cross-browser compliant code. Before we talk about the specific techniques that we used, let’s first understand the Smart Layers use case.

<!-- more -->

##Use Case

Smart Layers is a product suite that currently includes 6 different UI widgets (including share, follow, recommended content). Although each widget contains unique functionality, there are many similarities between the widgets.

<a href="http://www.addthis.com/blog/2013/10/03/javascript-design-patterns-used-in-smart-layers/" target="_blank" title="JavaScript Design Patterns">{% img center /images/smartlayers.png %}</a>

For example, all widgets create and append DOM elements, listen to events, support user-specified options, utilize CSS3 show/hide animations, etc. Once we put our code architect hats on, we realized that:

>We needed an inheritance model that could handle both unique widget functionality and common logic without code duplication.

##Simple JavaScript Prototypal Inheritance

At this point, it is important to delve into object-oriented JavaScript, and recognize the difference between **instance properties** and **prototype properties**.

###Instance Properties

Instance properties are properties that are scoped to a particular instance object and are not propagated to other instances (unless you go out of your way to change that). Let’s look at an example:

{% codeblock lang:js %}
  // Creates an AddThisEmployee constructor function
  function AddThisEmployee(name) {
    this.name = name;
  };

  // Creates an AddThisEmployee instance object
  // and stores it in the greg local variable
  var greg = new AddThisEmployee('Greg Franko'),
    // Creates an AddThisEmployee instance object
    // and stores it in the sol local variable
    sol = new AddThisEmployee('Sol Chea');

  // Greg doesn't like his name and decides to
  // legally change it to 'Frank The Tank'
  greg.name = 'Frank The Tank';

  // Sol has no idea that Greg has changed his name to
  // 'Frank The Tank' and continues to call him Greg
  // This angers Greg
{% endcodeblock %}

In the previous example, each instance object contains one instance property (name). Since each instance object’s properties are not propagated to each other, they are not aware of each other’s changes.

###Prototype Properties

On the other hand, prototype properties are properties that are propagated to all instances. In JavaScript, an object’s property is looked up at run time, by first checking if there is an instance property. If there is not an instance property, then the object’s prototype is checked next to see if it contains the property.

> All prototype properties are kept in sync and visible to each instance.

Let’s demonstrate prototypes by expanding upon our previous code example:

{% codeblock lang:js %}
  // Creates a Person constructor function
  function AddThisEmployee(name) {
    this.name = name;
  };

  // Assigns the AddThisEmployee prototype property
  AddThisEmployee.prototype = {
    companyName: 'AddThis'
  };
  // Creates an AddThisEmployee instance object and
  // stores it in the greg local variable
  var greg = new AddThisEmployee('Greg Franko'),
    // Creates an AddThisEmployee instance object and
    // stores it in the sol local variable
    sol = new AddThisEmployee('Sol Chea');

  // Greg doesn't like his name and decides to
  // legally change it to 'Frank The Tank'
  greg.name = 'Frank The Tank';

  // Sol has no idea that Greg has changed his name to
  // 'Frank The Tank' and continues to call him Greg
  // This angers Greg

  // AddThis changes it's company name to 'Team Awesome'
  AddThisEmployee.prototype.companyName = 'Team Awesome';

  // Both Greg and Sol know that AddThis changed it's name
  console.log(greg.companyName);
  console.log(sol.companyName);
{% endcodeblock %}

For Smart Layers, every widget instance reuses the same object as it’s prototype. This both simplifies the codebase and makes it easy to make changes that need to be propagated to all widgets.

##Factory Pattern

Since every widget is created in a similar way, we needed a design pattern to take care of the instance creation process. Smart Layers uses the Factory pattern as a generic interface for creating instance objects.

Here’s a high-level look:

{% codeblock lang:js %}
  var commonObj = {
    exampleProp: 'Each instance will have this property';
  },
    factory = function(name, instanceProps) {
      var Constructor = function() {
        for (var x in instanceProps) {
          if (instanceProps.hasOwnProperty(x)) {
            this[x] = instanceProps[x];
          }
        }
      },
        widget;
      widget.prototype = commonObj;
      widget = new Constructor();
      widget.name = name;
    };

  // Calls the factory method
  factory('share', {
    // Constructor function
    _create: function() {}
  });
{% endcodeblock %}

The previous example shows a `factory()` method that can be called by passing a string name and an object literal containing all of the desired instance properties.

> The factory pattern abstracts away the JavaScript constructor and prototype assigning ugliness, and provides an easy way to implement a consistent API for each instance.

**Note:** For similar techniques that work with jQuery plugins, check out the <a href="http://api.jqueryui.com/jQuery.widget/" target="_blank">jQueryUI Widget Factory</a> and <a href="https://github.com/gfranko/jqfactory" target="_blank">jqfactory</a> open-source projects.

##Module Pattern

Another major design pattern used in Smart Layers is the module pattern. Currently, the Smart Layers JavaScript API only has one public method (destroy) that can be used after Smart Layers is initialized on the page. Under the hood though, the JavaScript API consists of multiple hidden methods that are used internally. To accomplish public/private API methods, we took advantage of JavaScript function scoping to only expose what we wanted to.

Let’s look at a basic example of the module pattern:

{% codeblock lang:js %}
  // Function constructor
  var Person = function(obj) {
    // Local variables that are privately scoped
    var firstName = obj.firstName,
      lastName = obj.lastName,
      occupation = obj.occupation;

    // Public API properties
    return {
      firstName: firstName
    }
  },
  // Instantiating a new Person instance object
  greg = new Person({
    firstName: 'Greg',
    lastName: 'Franko',
    occupation: 'JavaScript Engineer'
  });

  // Prints out "Greg"
  console.log('first name', greg.firstName);

  // Prints out "undefined"
  console.log('last name', greg.lastName);

  // Prints out "undefined"
  console.log('occupation', greg.occupation);
{% endcodeblock %}

This example demonstrates that all local variables, declared using the `var` keyword, are only accessible within the declaring function or other nested functions.

In our example, a person’s first name, last name, and occupation are all local variables. An object, that includes the `firstName` property, is then returned inside of our constructor function.  Since the `firstName` local variable is the only variable returned, it is the only property that can be retrieved from a different scope.

> The module pattern is an example of a **closure**, since a local property is kept alive and can be referenced inside of a different scope.

Here’s a more practical example:

{% codeblock lang:js %}
  // Method that will only return functions that do not
  // begin with an underscore
  function publicApi (api) {
    var method,
      currentMethod,
      publicApi = {};
    for(method in api) {
      currentMethod = api[method];
      if(method.charAt(0) !== '_') {
        publicApi[method] = api[method];
      }
    }
    return publicApi;
  }

  // Stores the return value of the publicApi method
  var publicMethods = publicApi({
    publicMethod: function() {},
    _privateMethod: function(){}
  });

  // Should only contain the publicMethod as a property
  console.log('Public API', publicMethods);
{% endcodeblock %}

This demonstrates the technique that Smart Layers uses to expose public API methods. All methods that start with an underscore are assumed to be private and are not returned as a public method.

##Summary
Prototypal inheritance, the factory pattern, and the module pattern are all extremely useful techniques that can be used together to create powerful applications. I’d love to hear your thoughts and the patterns you use in your own JavaScript codebases!