---
layout: post
title: "Creating a jQuery Plugin using jqBoilerplate"
date: 2012-04-22 20:04
comments: true
categories: [jQuery, jQuery Plugin, jQueryUI Widget Factory]
description: "Boilerplate code to help build stateful jQuery widgets"
keywords: "Greg Franko, SelectBoxIt, jQuery select box plugin, jQuery select box, jQueryUI select box, jQuery, jQuery form stylingjQuery Boilerplate, boilerplate, Greg Franko, jQuery Plugin, jQueryUI Widget Factory, jQuery Stateful Plugin"
---
##What is a jQuery Plugin?

A typical jQuery plugin is a method that is added to the jQuery prototype object, `fn`, that interacts with one or more DOM elements wrapped in a jQuery object.  Something like this:

{% codeblock lang:js %}
//Adding the tooltip method to the jQuery prototype
$.fn.tooltip = function ( options ) {
    return this.each(function () {
        //Plugin logic
    });
}
{% endcodeblock %}

Then the jQuery plugin can be used like this:

{% codeblock lang:js %}
//Calls the jQuery tooltip plugin on a DOM element
$("input#firstName").tooltip();
{% endcodeblock %}

<!-- more -->

##Creating Advanced jQuery Plugins

Creating simple jQuery plugins like the above example is easy.  Creating more advanced/stateful jQuery plugins is not easy.  This is why projects such as the **jQueryUI widget factory** were created to provide structure for advanced/stateful jQuery plugins.

##jQueryUI Widget Factory

Although the **jQueryUI widget factory** is a great project that helps maintain best practices, code organization, and consistency among jQuery plugins, not every developer wants to use it.  The most common reason for developers not using the widget factory, is that it places another level of abstraction on top of creating a jQuery plugin.  This means another dependency/project that you need to keep upgraded, and another API that you need to learn.

##jqBoilerplate

Enter [jqBoilerplate](http://www.jqBoilerplate.com).  **jqBoilerplate** is a project I created that provides boilerplate jQuery/JavaScript code that will help you start writing stateful jQuery plugins/widgets more efficiently, without having to learn a new API.  All of the code provided is just vanilla jQuery, so if you know jQuery, you can start working right away.

The boilerplate code heavily uses the **Revealing Module Pattern**, which is a Classical JavaScript design pattern to allow private and public properties/methods.  This makes it trivial to provide your plugin users with a public API, while keeping the private parts of your plugin well... private.

##The Revealing Module Pattern Example

Here is a plain JavaScript example of the Revealing Module Pattern:

{% codeblock lang:js %}
//Person Constructor
var Person = function(firstName, lastName, occupation) {
    //Instance variables stored in the self object
    var self = {
        firstName: firstName,
        lastName: lastName,
        occupation: occupation
    },

    //Public method
    getFullName = function() {
        return self.firstName + " " + self.lastName;
    },

    //Private method
    _getOccupation = function() {
        return self.occupation;
    };

    //Public API
    return {
        getFullName: getFullName
    }

};
{% endcodeblock %}

The above example allows you to view greg's first and last name (Click on the JavaScript tab to view the code).  If you wanted to view greg's occupation, you would not be able to, because greg's occupation is private (not returned by the function).

{% jsfiddle kSAZ5 result,js,html,css %}
<br /><br />

##Other benefits of jqBoilerplate

jqBoilerplate not only organizes your jQuery plugin code with the Revealing Module Pattern, but it provides other plugin goodies as well.  Other goodies include a free jQuery pseudo selector, methods to interact with getting and setting plugin options, prevention of multiple instances of your jQuery plugin being called on the same element, callback function and chaining support, and more.

##How to Use jqBoilerplate

Although a lot of the plugin code has already been written for you, certain methods have been left mostly blank so that you can put your custom jQuery logic in them.  These methods include:

`_events()` - This method will handle setting all of your plugin event handlers

`disable()` - This method will disable the DOM element being called by your plugin

`enable()` - This method will enable the DOM element being called by your plugin

`destroy()` - This method will bring the page back to it's initial state (before the plugin was called)

`create()` - This method will create your jQuery plugin.  This is where the magic happens.

Although all of these methods are provided, you are free to remove them if you feel they are not necessary for your plugin.

After you write your custom logic in the above methods, the only thing left to do is update the `pluginName` variable to match your specific jQuery plugin name, and also update the `pluginVersion` variable to the match the actual release version of your jQuery plugin.  And thats it!  Happy jQuery Plugin coding!

##Frequently Asked Questions

Questions will be added here from the <a href="https://github.com/gfranko/jq-boilerplate/issues" target="_blank">Github Issues Page</a> and from the comments below.  Also, more demos/documentation will be provided on request.