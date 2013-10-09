---
layout: post
title: "jQuery Best Practices"
date: 2013-07-20 11:24
comments: true
categories: [jQuery]
description: "Register the jQueryUI Widget Factory as a named AMD module so that other jQuery plugins that depend on the Widget Factory can provide AMD support"
keywords: "Greg Franko, jQuery, jQuery Best Practices"
---

<a href="http://gregfranko.com/jquery-best-practices/#/" target="_blank">{% img left /images/jquery.png 300 %}</a>

Back in February 2013, I presented about jQuery best practices at the [Nova Web Development User Group](http://www.meetup.com/NoVA-Web-Develoment-User-Group/events/101712422/) meetup.  Let's take a moment to review my <a href="http://gregfranko.com/jquery-best-practices/#/" target="_blank">presentation</a>.

<!-- more -->

<br><br>
##IIFEs

IIFEs are an ideal solution for locally scoping global variables/properties and protecting your JavaScript codebase from outside interference (e.g. third-party libraries).  If you are writing jQuery code that will be run in many different environments (e.g. jQuery plugins), then it is important to use an IIFE to locally scope jQuery.

> You can't assume everyone is using the $ to alias jQuery.

Here is how you would do it:

{% codeblock lang:js %}
	// IIFE - Immediately Invoked Function Expression
	(function($, window, document) {
		// The $ is now locally scoped

		// The rest of your code goes here!

	}(window.jQuery, window, document));
	// The global jQuery object is passed as a parameter
{% endcodeblock %}

If you don't like having to scroll to the bottom of your source file to see what global variables/properties you are passing to your IIFE, you can do this:

{% codeblock lang:js %}
	// IIFE - Immediately Invoked Function Expression
	(function(yourcode) {

		// The global jQuery object is passed as a parameter
		yourcode(window.jQuery, window, document);

		}(function($, window, document) {

			// The rest of your code goes here!

		}
	}));
{% endcodeblock %}
To read more about IIFEs, you can read my blog post titled, [I Love My IIFE](/blog/i-love-my-iife/).

##Ready Event

Many developers wrap all of their code inside of the jQuery ready event like this:

{% codeblock lang:js %}
	$("document").ready(function() {
		// The DOM is ready!
		// The rest of your code goes here!
	});
{% endcodeblock %}

Or developers use a shorter version like this:

{% codeblock lang:js %}
	$(function() {
		// The DOM is ready!
		// The rest of your code goes here!
	});
{% endcodeblock %}

If you are doing either of the above patterns, then you should consider moving the pieces of your application (e.g. methods), that don't depend on the DOM, outside of the ready event handler.  Like this:

{% codeblock lang:js %}
	// IIFE - Immediately Invoked Function Expression
	(function(yourcode) {

		// The global jQuery object is passed as a parameter
		yourcode(window.jQuery, window, document);

		}(function($, window, document) {

			// The $ is now locally scoped 
			$(function() {

				// The DOM is ready!

			});

			// The rest of your code goes here!

		}
	}));
{% endcodeblock %}

This pattern makes it easier to separate your logic (from a code design perspective) since not everything has to be wrapped inside of a single anonymous function.  It will also improve your application's page load performance, since not everything needs to initialized right away.

##Caching DOM selectors

When manipulating elements in the DOM (e.g. animating, setting attributes, changing CSS properties), it is important to consider performance.

> Every time you select a DOM element using jQuery, you are instantiating a new jQuery object and forcing jQuery to search the DOM.

Instead of creating multiple jQuery instances and forcing jQuery to re-query the DOM for a particular element, you can cache your jQuery object in a variable and reuse it like this:

{% codeblock lang:js %}	
	// Stores the jQuery instance inside of a variable
	var elem = $("#elem");

	// Set's an element's title attribute using it's current text
	elem.attr("title", elem.text());

	// Set's an element's text color to red
	elem.css("color", "red");

	// Makes the element fade out
	elem.fadeOut();
{% endcodeblock %}

If you prefer the previous code snippet to be on one line, you can **chain** multiple jQuery methods together like this:

{% codeblock lang:js %}
	// Stores the live DOM element inside of a variable
	var elem = $("#elem");
	
	// Chaining
	elem.attr("title", elem.text()).css("color", "red").fadeOut();
{% endcodeblock %}

##Adding DOM elements

Adding elements to the DOM is one of the slowest and most common use cases in a web application.  To limit the performance hit, make sure you are not unneccessarily adding elements to the DOM.

A common example, that can cause web applications to feel janky (or slow), is building a dynamic HTML unordered list.  Since there could potentially be thousands of list items inside of the list, you want to make sure to only append to the DOM once (instead of thousands of times).  Like this:

{% codeblock lang:js %}
	// Dynamically building an unordered list from an array
	var localArr = ["Greg", "Peter", "Kyle", "Danny", "Mark"],
		list = $("ul.people"),
		dynamicItems = "";

	$.each(localArr, function(index, value) {

		dynamicItems += "<li id=" + index + ">" + value + "</li>";

	});

	list.append(dynamicItems);
{% endcodeblock %}

The previous example stores the entire HTML of the dynamic list inside of a local string variable.  Once all the HTML has been created, the list is appended to the page **ONE TIME**.  Many developers mistakingly append to the DOM each time they create a new list item.  **DO NOT DO BE THAT DEVELOPER**

##Event Delegation

DOM event handlers take up a lot of memory and can very easily cause **memory leaks** (and poor performance) in web applications.  We can use an HTML unordered list example to demonstrate how to use **event delegation** to reduce the number of event handlers that we have and the amount of memory our application uses.

{% codeblock lang:js %}
	var list = $("#longlist");

	list.on("mouseenter", "li", function(){

		$(this).text("Click me!");

	});

	list.on("click", "li", function() {

		$(this).text("Why did you click me?!");

	});
{% endcodeblock %}

The previous example uses the jQuery `on()` method to add _mouseenter_ and _click_ event handlers to an unordered list.  The second argument provided to the `on()` method is the **event handler context**.  This tells jQuery to trigger the associated unordered list event handler if a list item child element has been the targeted DOM element.

To understand event delegation, we have to understand **event bubbling**.  Event Bubbling is a behavior associated with DOM elements where parent DOM elements (all the way up to the `document`) are notified of child DOM element events.  Not all DOM events natively _bubble_, so jQuery has done the heavy lifting and made the event bubbling behavior consistent for every event and browser.

Because of event bubbling, we only have to add one additional event handler to the unordered list (instead of a new event handler for each child list item).  This saves system memory, which allows more memory to be allocated for other intensive operations, such as animations (improving our app's perceived performance).

##Ajax

Since jQuery version 1.5+, the jQuery `ajax()` method has returned a Promise object.  Using Promises with Ajax allows us to bind multiple callback functions to our request, write flexible code where our ajax handling logic is in a different place than our actual request, and wait for multiple requests to complete before starting an action.

Here is an example of separating the handling logic from the actual request:

{% codeblock lang:js %}
	function getName(personid) {
		var dynamicData = {};
		dynamicData["id"] = personID;
		// Returns the jQuery ajax method
		return $.ajax({
			url: "getName.php",
			type: "get",
			data: dynamicData
		});
	}

	getName("2342342").done(function(data) {
		// Updates the UI based the ajax result
		$(".person-name").text(data.name); 
	});
{% endcodeblock %}

Our example demonstrates a `getName()` function that accepts a unique id, sends an ajax request with that unique id, and returns the jQuery `ajax()` method.  Since our function returns the jQuery `ajax()` method (which returns a Promise by default), we are able to call the `done()` method and pass a callback function (which will execute once the request has completed).

If we had wanted to wait for two separate ajax requests to our endpoint, we could have done something like this:

{% codeblock lang:js %}
	function getName(personid) {
		var dynamicData = {};
		dynamicData["id"] = personID;
		// Returns the jQuery ajax method
		return $.ajax({
			url: "getName.php",
			type: "get",
			data: dynamicData
		});
	}

	var person1 = getName("2342342"),
		person2 = getName("3712968"),
		people = [person1, person2];

	$.when.apply(this, people).then(function() {
		// Both the ajax requests have completed
	});
{% endcodeblock %}

We are using the JavaScript `apply()` method in the previous example to demonstrate how we can pass an array to the `$.when` method.  If you do not want to use an array, then you can very easily pass each promise object (stored in a variable) as regular parameters.

Happy jQuerying!
