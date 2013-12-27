---
layout: post
title: "JavaScript Performance Tips"
date: 2013-09-17 16:25
comments: true
categories: [JavaScript, Performance, AddThis]
description: "jQuery Plugin Architecture"
keywords: "Greg Franko, Performance, AddThis"
cover: "http://farm4.staticflickr.com/3742/9776421873_0d5795a966_z.jpg"
---

It is a competitive advantage for websites to be fast and responsive.  When building the <a href="https://www.addthis.com/get/smart-layers#.UjjAS2TXhTs" target="_blank">AddThis Smart Layers</a> tool suite, performance was a priority and first-class feature. Let’s take a look at some of the mobile and desktop performance best practices that I (and the rest of the AddThis team) used to make Smart Layers blazing fast.

<!-- more -->

<br><br>
##Dynamic Loading

With the increasing number of mobile web users using a 3g/4g connection, it’s more important than ever to only download what you need. To save unnecessary bytes from being downloaded, we dynamically load assets (JavaScript, CSS, etc).

<a href="http://www.addthis.com/blog/2013/09/17/performance-optimizing-for-smart-layers/" target="_blank" title="JavaScript Performance Best Practices">{% img center http://farm4.staticflickr.com/3742/9776421873_0d5795a966_z.jpg %}</a>

For example, Smart Layers uses the great <a href="https://github.com/douglascrockford/JSON-js" target="_blank">JSON2.js</a> JavaScript library, created by <a href="https://github.com/douglascrockford" target="_blank">Douglas Crockford</a>, to provide native JSON support for older browsers (IE6 and IE7). Instead of including JSON2.js for browsers that already support the native JSON methods, we make sure to only include JSON2.js for browsers that need the polyfill.

Dynamic asset loading is becoming popular in the front-end community, and is often a benefit of using a client-side script loader, such as <a href="http://requirejs.org">Require.js</a> (which we don’t use).

##Animations

The unofficial performance goal for web animations is to perform at around 60 frames per second (fps). If your app is running animations at 60fps, then your users will see smooth and jank-free animations. Smooth animations = happy users.

To achieve ~60fps performance, Smart Layer animations use CSS transitions and transforms. A major performance benefit of using CSS transitions and transforms is **hardware acceleration**. At a high level, this means that all of the animation hard work can be offloaded to the browser and GPU. Performance benchmarks have shown massive speed and resource consumption improvements for browsers that utilize these techniques (specifically mobile browsers).

Instead of creating our own custom CSS transforms and transitions, we decided to use a few of the CSS-based animations from the popular project, <a href="http://daneden.me/animate/" target="_blank">Animate.css</a>. We love open-source technologies and want to thank <a href="https://twitter.com/_dte" target="_blank">Dan Eden</a> for all of his hard work on the project.

##Memory Management

Proper memory management is crucial for the performance of web applications. A common reason for a web app feeling sluggish and non-responsive to a user is a memory leak.

{% blockquote %}
I thought the browser does automatic JavaScript garbage collection and memory deallocation, so why do I have to worry about this?
{% endblockquote %}

You’re right, your browser does automatically clean up and deallocate memory. Unfortunately, the browser does not know your application, and you must make it clear what code is no longer being used and can be freed up for other things.

**Memory leaks are so common that your web application most likely has one** that you may not have noticed; only the memory leaks which cause performance issues easily draw attention. One of the most performance-intensive operations in JavaScript is interacting with the DOM. If you suspect that your application has a memory leak, then you should first investigate if it is DOM-related.

Since so many web applications these days are **single page applications** (I know, I’m tired of that buzz word too), and long-lived (meaning they go for a long time without a page refresh), we wanted to make sure to support memory deallocation for Smart Layers. To support this, Smart Layers provides a `destroy()` method that allows you to remove all DOM nodes and DOM event listeners it creates. This method will trigger the browser to garbage collect and deallocate the memory used by Smart Layers to other parts of your application.

This brings us to another important topic: **memory consumption**. Smart Layers uses a variety of techniques to make sure that it is taking up as little memory as possible, leaving more for your application. Here are a couple techniques.

###Event Delegation

Event delegation promotes binding as few DOM event handlers as possible, since each event handler requires memory. For example, let’s say that we have an HTML unordered list we want to bind event handlers to. Instead of binding a click event handler for each list item (which may be hundreds for all we know), we bind one click handler to the parent unordered list itself.

But how does the unordered list click handler get triggered when a child list item is clicked? That’s where event bubbling comes in.

**Event bubbling** is the concept that a DOM event is propagated (sent to) to the outer-most elements (parents) all the way up to the root element of the document (e.g. the HTML element). By checking the target property of a JavaScript Event object, we can see what DOM element triggered the current event.

Smart Layers heavily uses event delegation and event bubbling to minimize the number of event handlers that are bound. While testing Smart Layers on mobile devices, there was a noticeable performance improvement when using event delegation instead of direct element binding, since less memory was being used.

###DOM Performance

Let’s reiterate; **DOM interactions are one of the most memory-expensive actions** a web application can complete.

A common performance pitfall is repeatedly appending DOM elements (causing multiple browser reflows) to the page instead of appending once. With Smart Layers, we only append elements to the DOM when absolutely necessary. For each Layer, we construct all of the HTML that we want as a string, and then append that string to the DOM. This prevents unnecessary browser reflows, which force the browser to re-calculate the layout of the page.

Another best practice that Smart Layers uses to improve performance is **simplified DOM selectors and DOM caching**. If we can select a DOM element using an id, then we do. We also save those elements as instance properties internally, so that we do not need to re-query the DOM again.

For any complicated selectors (not too complicated, mind you), we use <a href="https://github.com/ded/qwery" target="_blank">qwery</a>, a compact, blazing fast CSS selector engine. We are not using <a href="http://sizzlejs.com/" target="_blank">Sizzle</a> (the selector engine used by jQuery) since we are not using jQuery, and we do not need all of the advanced selectors Sizzle provides. A special shout out goes to <a href="https://twitter.com/ded" target="_blank">Dustin Diaz</a>, <a href="https://twitter.com/fat" target="_blank">Jacob Thornton</a>, and <a href="https://twitter.com/rvagg" target="_blank">Rod Vagg</a> for their amazing work on qwery.

##Debouncing High-Rate Events

Certain JavaScript events are notorious for being triggered at a high rate per second. Examples include the **scroll and resize events**. If you are listening for these DOM events and your event handlers contain performance-intensive actions, you should be **debouncing your event handlers**.

Debouncing an event handler allows you to specify a time (e.g. 500 milleseconds) where you would only like one call to your event handler to execute. For example, if the window scroll event triggers 100 times every 500 milleseconds, your debounced event handler will only get called once (probably the desired behavior). Remember that JavaScript is single threaded (unless you are using web workers, which do not handle DOM-related activities), so if your event handler had executed 100 times repeatedly, it will most likely prevent other logic from executing.

Smart Layers listens to both the resize and scroll events for different reasons, and we make sure to debounce each event to prevent from blocking the UI thread.

One of the cooler features of Smart Layers is our responsive toolset. If you are on a desktop browser and the viewport is less than the size of a tablet browser, we automatically provide our mobile tools (which look better in a small viewport) using the resize event.

{% blockquote %}
Why don’t you just use CSS3 media queries instead of listening to the JavaScript resize event?
{% endblockquote %}

We use CSS3 media queries, in conjunction with the debounced resize event, so that we can allow users to specify which viewport sizes they would like our mobile tools to show up on for desktop browsers. Configurable + good performance = happy users.

If you would like to see example implementations of a debounce method, then check out either <a href="http://underscorejs.org/#debounce" target="_blank">Underscore.js</a> or <a href="http://lodash.com/docs#debounce" target="_blank">Lodash.js</a>.

##Infinite Scrolling

On mobile browsers, we allow you to scroll through all the possible sharing services that we support. There are hundreds of services, and we are using beautiful, retina-ready, **SVG icons** for each service. To prevent any performance issues, we do not load all of our services at once.

If a user searches for a service, or scrolls to a position on the page where that service should be, then we load the service (and it’s associated icon). This technique allows our users to enjoy beautiful icons, and not get bogged down by any performance issues.

##Responsive Mobile Scrolling

When testing Smart Layers on mobile browsers, we noticed that scrolling through a large unordered list with a flick of the finger did not send the webpage scrolling. Since we did not feel this was a great user experience, we started to investigate how we could incorporate this **momentum** effect.

We found out that you can add this default behavior on iOS 5 using the `-webkit-overflow-scrolling: touch;` CSS property. This is great, but what about other platforms/browsers? To support other browsers, we incorporated the popular library, <a href="https://github.com/cubiq/iscroll" target="_blank">iScroll</a>. A big thank you to <a href="https://twitter.com/cubiq" target="_blank">Matteo Spinelli</a> for all of his hard work on iScroll.

We hope that all of these performance best practices baked into Smart Layers pay dividends for you. If you haven’t already tried <a href="https://www.addthis.com/get/smart-layers#.UjjAS2TXhTs" target="_blank">Smart Layers</a>, grab and code and let us know what you think!