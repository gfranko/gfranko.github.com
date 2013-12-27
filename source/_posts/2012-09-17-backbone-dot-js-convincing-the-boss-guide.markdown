---
layout: post
title: "Backbone.js: Convincing the Boss Guide"
date: 2012-09-17 10:00
comments: true
categories: [JavaScript, Backbone.js]
description: "Creating Backbone.js Custom Builds to help convince the boss to use Backbone.js"
keywords: "Greg Franko, Backbone.js, Backbone.js Custom Builds, Backbone.js Convincing the Boss Guide" 
---

I am officially pronouncing **2012 as the year of the JavaScript MV\* frameworks**.  Although many people complain that the JavaScript MV\* frameworks boom has created too many frameworks to choose from, which further divides the developer community, let's look at the glass half full.

<!-- more -->

The influx of JavaScript MV\* frameworks has created new design patterns that have promoted **JavaScript as a language that you should care about**.  JavaScript codebase consistency, maintainability, and performance have never been scrutinized more.  This intensified focus is helping the web become an even better place to spend your time.

Although 2012 has been a big win for the JavaScript community, there is still much work to be done.  Much of that work consists of educating developers, who may not be JavaScript wizards (seriously not everyone can be [Paul Irish](http://paulirish.com/), [Addy Osmani](http://addyosmani.com/), etc), that organizing their front-end codebases deserves their full time and attention.

A common argument against using JavaScript MV* frameworks by non-JavaScript wizards is that most "apps" are not complex enough to warrent using these "bloated MV Whatever frameworks" (their words not mine).  Before you start screaming at your computer, let's look at this argument in more detail using [Backbone.js](http://backbonejs.org), one of the most popular JavaScript MV\* frameworks, and [custom Backbone.js builds](http://gregfranko.com/backbone/customBuild/).

###Backbone.js

{% img center /images/backbone.png %}

**Backbone.js** is a great client-side MV* JavaScript framework that provides structure to JavaScript applications by providing View, Model, Collection, Router, and Event class objects.  Although your application may not need all of the functionality that Backbone.js provides you, 99% of all applications could use the functionality provided by one or more Backbone.js class objects.  **Let's take a look at each Backbone.js class object and determine if a "simple" application needs the functionality provided.**

####Backbone.js View
Backbone **Views** allow you to organize all of your JavaScript event handlers while also providing a mechanism for adding dynamic HTML to your page through the optional use of JavaScript templates.

Here is an example Backbone View class:
{% codeblock lang:js %}
	var View = Backbone.View.extend({

        // Represents the actual DOM element that corresponds to your View
        el: 'body',

        // Constructor
        initialize: function() {

        },

        // Event Handlers
        events: {

            "click #example": "doSomething"

        },

        render: function() {

            // Updates the text of the element with an ID attribute of example            
            this.$el.find("#example").text("This is an example");

        },

        doSomething: function() {

            // Do something

        }

    });
{% endcodeblock %}

#####Backbone.js View Conclusion
The basic functionality that a Backbone View class object provides is organizing your JavaScript event handlers and organizing how you update your applications UI.  99.9% of all applications (simple or complex) will include JavaScript event handlers that respond to user interactions and then add content to a page.

A common problem with most applications is that their JavaScript code is filled with inconsistent event handlers.  You could very well see a "legacy" application with **DOM Level 0** (event handlers inside HTML code), **DOM Level 1** (event handlers inside JavaScript code that limit the amount of event handlers that can be bound to each HTML element) , and **DOM Level 2** event handlers (event handlers inside JavaScript code that allows an unlimited amount of event handlers to be bound to each HTML element).  This inconsistency makes it extremely difficult to maintain an app, especially if a new team member is added to the project.

If you are using a library, such as [jQuery](http://www.jquery.com), you most likely have many DOM Level 2 events in your codebase (ie. $("body").click(function() {})).  Although jQuery helps with the consistency of your event handlers, it does not help you structure how to organize all of your event handlers.  With just jQuery, an application could consist of spaghetti code that binds event handlers all over the place.  I have worked in this environment, and have found that more bugs are created because of this inconsistency.

No matter the complexity of your application, Backbone Views could very easily help promote best practices and improve maintainability of your JavaScript event handlers.

####Backbone Model
Backbone **Models** store all of your applications business logic and data. This allows you to organize all of your application's data validation inside of your Models.

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

#####Backbone.js Model Conclusion
The basic functionality that a Backbone Model class object provides is a place for your application to store and validate client-side information.  99.9% of applications will include validation of data.

A common problem with most applications is that they use the DOM (Document Object Model) as a database (Backbone.js creator, Jeremy Ashkenas, has talked about this at length).  Any time an application needs to find the current "state" of an application, that data is retrieved from the DOM.  This can severely hurt performance, since DOM related activities are one of the most memory intensive actions your JavaScript can take.

A common misconception is that the DOM is a part of the JavaScript language.  It isn't.  In fact, it is a completely separate API that JavaScript interacts with.  Having the ability to query the current "state" of your application without always resorting to querying the DOM is a HUGE WIN.

No matter the complexity of your application, Backbone Models could very easily promote decoupling your code and standardizing where your application performs business logic validation.

####Backbone.js Collection
If your application is complex, it is also helpful to use Backbone **Collections**, which provides a mechanism to interact with many different Model instances. Since Backbone has a hard dependency on Underscore.js, you are able to utilize many Underscore.js methods to interact with Collections.

Here is an example Backbone Collection class:
{% codeblock lang:js %}
var Collection = Backbone.Collection.extend({

    //Allows the Collection to work with User models
    model: User

});
{% endcodeblock %}

#####Backbone.js Collection Conclusion
The basic functionality that a Backbone Collection class object provides is a mechanism for your application to interact with one or more Backbone Models.

The Backbone Collection class object may not be necessary for all applications, since the true benefit of collections shines through when your application contains more than one Backbone Model instance, and very simple applications may only contain one model instance.

####Backbone.js Router
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

#####Backbone.js Router Conclusion
The basic functionality that a Backbone Router class object provides is a mechanism for your application to utlize a "single page" structure.  This is particularly useful if you do not want page refreshes in your application when a user navigates between "pages" or different sections of your app's functionality.

The Backbone Router class object may not be necessary for all applications, since not all applications may want the "single page" feel or care about refreshing the page.

####Backbone.js Event
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

#####Backbone.js Event Conclusion
The basic functionality that a Backbone Event class object provides is a mechanism for using the **pub/sub** pattern to decouple an application's codebase.  99.9% of applications could benefit from a design pattern that promotes decouplization (not sure if that is a real word).

A common jQuery design pattern is triggering and binding to **jQuery special events** on the HTML body element.  This pattern is extremely useful, but Backbone improves on this pattern, since it does not rely on the DOM for its pub/sub implementation.  This improves performance and flexibility, since it allows you to bind to custom events on a generic JavaScript object instead of just a jQuery object.

No matter the complexity of your application, the Backbone Events class object could very easily promote decoupling your code and consistent special event binding.

###Backbone.js Custom Builds

Backbone.js does not support custom builds by default, so I looked at the Backbone codebase and split up all of the Backbone Class Objects into their own files. Since Backbone.js is not set up in a modular way and instead heavily uses local variables all scoped under one Immediately Invoked Function Expression (IIFE) like most libraries, I had to make a few changes to remove these local property dependencies so that they could be split into several files.

With Backbone.js custom builds, you can now use the Backbone Events class object as a standalone pub/sub solution, or only use Backbone Views to organize all of your app's event handlers, or use Backbone Models to store all of your applications data client-side, etc (all while minimizing your file size).

Backbone.js is incredible, so I wanted to make it even easier for people who aren't completely sold on using it, to only take what they want/need.  Start making [Backbone.js Custom Builds](http://gregfranko.com/backbone/customBuild/) now!


