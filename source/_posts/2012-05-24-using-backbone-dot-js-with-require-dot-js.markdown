---
layout: post
title: "Using Backbone.js with Require.js"
date: 2012-05-24 17:30
comments: true
categories: [JavaScript, Backbone.js, Require.js]
description: "Use Backbone.js and Require.js together to decouple your JavaScript into modules, separate business logic from application logic using Models and Views, reuse your JavaScript between Desktop and Mobile Web versions, include non-AMD Compatible Third Party Scripts in your project, and optimize all of your JavaScript (minify, concatenate, etc)."
keywords: "Greg Franko, Backbone.js, Require.js, Using Backbone and Require Together, Backbone, Require, Greg Franko" 
---

{% img left http://backbonejs.org/docs/images/backbone.png 405 %}{% img left http://requirejs.org/i/logo.png 124 %}
<br /><br /><br /><br /><br />

**Backbone.js** is a great client-side MV* (not MVC) JavaScript framework that provides structure to JavaScript applications by providing View, Model, Collection, and Router classes.  Backbone also provides a pub sub (publish subscribe) mechanism, by allowing each of it's objects to trigger and bind to events.

Backbone **Views** act as a combination of traditional MVC Views and Controllers, since they allow you to organize all of your JavaScript event handlers while also providing a mechanism for adding dynamic HTML to your page through the optional use of JavaScript templates.  Views will also often be where you set data on your Models.

Here is an example Backbone View class:
{% codeblock lang:js %}
	var View = Backbone.View.extend({

        // Represents the actual DOM element that corresponds to your View (There is a one to one relationship between View Objects and DOM elements)
        el: 'body',

        // Constructor
        initialize: function() {

            //Setting the view's model property.  This assumes you have created a model class and stored it in the Model variable
            this.model = new Model();

        },

        // Event Handlers
        events: {

            "click #example": "promptUser"

        },

        render: function() {

            // Updates the text of the element with an ID attribute of example            
            this.$el.find("#example").text("This is an example");

        },

        promptUser: function() {

            prompt("Isn't this great?", "Yes, yes it is");

        }

    });
{% endcodeblock %}

<!-- more -->

Backbone **Models** store all of your applications business logic and data. This allows you to organize all of your application's data validation inside of your Models.  In most cases, Models should not know about any of your Views, or the DOM.  There are certain design patterns (eg. modelbinding) where Models are aware of the DOM and set data on it, but most patterns do not implement this strategy since it is beneficial for Model's to be completely decoupled from other pieces of the application.

Here is an example Backbone Model class:

{% codeblock lang:js %}
    var Model = Backbone.Model.extend({

        // Default properties
        defaults: {

            example: "I love having my data separated from the DOM"

        }

        // Constructor
        initialize: function() {

        },

        // Any time a Model attribute is set, this method is called
        validate: function(attrs) {

        }

    });
{% endcodeblock %}

If your application is complex, it is also helpful to use Backbone **Collections**, which provides a mechanism to interact with many different Model instances. Since Backbone has a hard dependency on Underscore.js, you are able to utilize many Underscore.js methods to interact with Collections.  Keep in mind that you may alternatively use **lodash**, a drop-in replacement library for Underscore.js that provides cross browser bug fixes and performance improvements for Underscore.js.  Lodash also allows custom builds, so you can only use a minimum number of Underscore.js functions if you like (reducing file size).

Here is an example Backbone Collection class:
{% codeblock lang:js %}
var Collection = Backbone.Collection.extend({

    //Allows the Collection to work with User models
    model: User

});
{% endcodeblock %}

Backbone **Routers** are typically used to define "routes" in your application. Routes are essentially a unique page in a traditional web application, but in a one page JavaScript application, they help organize your client-side logic without requiring a page refresh (traditionally appending a route as a hash tag to a browser's url or using the new HTML5 push state api to silently track page state).

Here is an example Backbone Router class:
{% codeblock lang:js %}
    var Router = Backbone.Router.extend({

        // Constructor
        initialize: function(){

        //Required for Backbone to start listening to hashchange events
        Backbone.history.start();

        },

        routes: {

            // Calls the home method when there is no hashtag on the url
            '': 'home'

        },

        'home': function(){

            // Gets called when there is no hashtag on the url
            console.log("My very first Backbone route");

        }
    });
{% endcodeblock %}


Backbone **Events** are an important concept in Backbone, since they provide you with an easy mechanism to use the pub sub pattern and decouple your code.  Backbone triggers certain events by default (eg. a Model change event is triggered after a Model property has been validated and saved), but Backbone also allows you to trigger and bind to custom events.  This is an extremely useful pattern, since it allows many different classes to listen to one class, without that one class knowing about any of its listeners.

Here is an example Event being triggered by a Backbone Model class:
{% codeblock lang:js %}
    var Model = Backbone.Model.extend({

        // Default properties
        defaults: {

            example: "I love having my data separated from the DOM"

        }

        // Constructor
        initialize: function() {

        },

        // Any time a Model attribute is set, this method is called
        validate: function(attrs) {

        },

        triggerEvent: function() {

            // Triggers an test event and passes data that can be accessed in the event handler
            this.trigger("test", { someData: "data" });

        }     
 
    });
{% endcodeblock %}

Here is an example Event being bound by a Backbone View class:
{% codeblock lang:js %}
	var view = Backbone.View.extend({

        // Represents the actual DOM element that corresponds to your View (There is a one to one relationship between View Objects and DOM elements)
        el: 'body',

        // Constructor
        initialize: function() {

            //Setting the view's model property.  This assumes you have created a model class and stored it in the Model variable
            this.model = new Model();

            //Event handler that calls the initHandler method when the init Model Event is triggered
            this.model.on("test", this.test);

        },

        // Event Handlers
        events: {

            "click #example": "testModelEvent"

        },

        render: function() {

            // Updates the text of the element with an ID attribute of example            
            this.$el.find("#example").text("This is an example");

        },

        promptUser: function() {

            prompt("Isn't this great?", "Yes, yes it is");

        },

        testModelEvent: function() {
            this.model.triggerEvent();
        },

        test: function(obj) {

            console.log("Just got " + obj.someData + " from my model!");

        }

    });
{% endcodeblock %}

**Require.js** serves two different purposes than Backbone.js. Require.js is an AMD (Asynchronous Module Definition) script loader that asynchronously loads your JavaScript to improve page load performance, while also providing you the ability to organize your JavaScript into self contained modules.  Each JavaScript file represents a module.

Each module is enclosed in a define tag that lists the module's file dependencies, and keeps the global namespace free (essentially acting as a JavaScript closure).  Since none of your modules are global, inside of each module, you need to declare which other modules are dependencies and pass them to your current module.  This provides a solution for limiting global variables and dependency management. This solution is much better then having many script tags in a single page, which can be cumbersome to keep track of which files depend on which other files.  It also encourages you to decouple your JavaScript logic (instead of the traditionally hard to read one page JavaScript application).

Here is a more advanced example of a Backbone View class used with Require.js:
{% codeblock lang:js %}
define(['jquery', 'use!backbone','models/model'], function($, Backbone, Model){

    var self,

	View = Backbone.View.extend({

        // Represents the actual DOM element that corresponds to your View (There is a one to one relationship between View Objects and DOM elements)
        el: 'body',

        // View constructor
        initialize: function() {

            // Storing the View context
            self = this;

            // Setting the View's model property to the passed in Model
            this.model = new Model({
                message: "You are now using Backbone, Require, and jQuery!"
            });

        },

        events: {
            "click #example": "promptUser"
	    },

        render: function() {

            // Updates the text of the element with an ID attribute of example
            self.$el.find("#example").text("This is an example");

        },

        promptUser: function() {
            prompt("Isn't this amazing?", "Yes, yes it is");
        }

    });
	
    // Returns the entire view (allows you to reuse your View class in a different module)
    return new View();
});
{% endcodeblock %}

Backbone.js and Require.js perfectly complement each other, but there is one problem... Backbone.js is not AMD compatible.  To be AMD compatible, a script must declare itself as a module by calling the **define()** method if it exists and list it's dependencies.  Backbone decided to not natively support AMD/Require.js, because it's creater, Jeremy Ashkenas, felt it was unnatural to have to change Backbone's source code so that it would work in Require.js and other AMD loaders.  Luckily for us, Backbone contributor, Tim Branyen, created **Use.js** for this exact reason.

Looking at the previous code example, you will notice that I am using the Use.js plugin to include Backbone as a dependency to my View class.  Use.js allows you to use (no pun intended) scripts that are not AMD compatible, by setting a one time configuration (typically in a main.js file or initialization file), and then prefixing dependencies with `use!`.

Here is a typical Use.js configuration with Backbone.js and Underscore.js:
{% codeblock lang:js %}
  // Sets the use.js configuration for your application
  use: {

    backbone: {
      deps: ["use!underscore", "jquery"],
      attach: "Backbone"  //attaches "Backbone" to the window object
    },

    underscore: {
      attach: "_" //attaches "_" to the window object
    }

  } // end Use.js Configuration
{% endcodeblock %}

**Note**: Require.js 2.0 added support for loading non-AMD compatible scripts by using the Shim configuration.  You can find a more detailed explanation of the Shim configuration in my next <a href="http://gregfranko.com/blog/require-dot-js-2-dot-0-shim-configuration/" target="_blank">blog post</a>. If you want more in-depth explanations and examples of how to setup Backbone.js, Require.js, and the Shim configuration, check out the <a href="https://github.com/gfranko/Backbone-Require-Boilerplate" target="_blank">Backbone-Require-Boilerplate</a> I recently created on Github.

An additional benefit of using Require.js is you can **optimize** (concat, minifiy) your entire project or single JavaScript files (depending on your preference) using **r.js**, a library written by James Burke that allows you to use the Require.js Optimizer.  All you need to do is install either node.js or rhino (both can be used as command line tools but node.js is recommended) and create a build file.

Here is an example of an app.build.js configuration file:
{% codeblock lang:js %}
//Install node.js, navigate to the js folder, and then run this command: "node r.js -o app.build.js"
({

  // Creates a js-optimized folder at the same folder level as your "js" folder and places the optimized project there
  dir: "../js-optimized",

  // 3rd party script alias names
  paths: {

    // Core Libraries
    modernizr: "libs/modernizr-2.5.3.min",
    jquery: "libs/jquery-1.7.2.min",
    underscore: "libs/lodash-0.2.0.min",
    backbone: "libs/backbone-0.9.2.min",

    // Require.js Plugins
    use: "plugins/use-0.3.0.min",
    text: "plugins/text-1.0.8.min"

  },

  // Sets the use.js configuration for your application
  use: {

    backbone: {
      deps: ["use!underscore", "jquery"],
      attach: "Backbone"  //attaches "Backbone" to the window object
    },

    underscore: {
      attach: "_" //attaches "_" to the window object
    }

  }, // end Use.js Configuration

  // Modules to be optimized:
  modules: [

    {
      name: "mobile"
    },

    {
      name: "desktop"
    }
  ]

})
{% endcodeblock %}

Obviously your app.build.js file will most likely be different, but hopefully this helps get you on the right track (note: I took this directly from my Backbone-Require-Boilerplate project).

Considering the many advantages of using Backbone.js and Require.js together, I hope I have convinced you to try out these great open source projects.  Happy coding!