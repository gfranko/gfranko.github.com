---
layout: post
title: "Registering the jQueryUI Widget Factory as an AMD Module"
date: 2013-02-10 09:39
comments: true
categories: [jQuery, jQueryUI Widget Factory, Require.js]
description: "Register the jQueryUI Widget Factory as a named AMD module so that other jQuery plugins that depend on the Widget Factory can provide AMD support"
keywords: "Greg Franko, jQueryUI, jQueryUI Widget Factory, Require.js, jQuery"
---

**AMD** (Asynchronous Module Definition) script loaders are fast becoming one of the most popular and important tools for organizing and maintaining JavaScript applications.  Many front-end developers, like myself, are becoming educated on the benefits of using a module system that promotes code reusability/decoupling, freeing the global namespace, dependency script management, and more.

<!-- more -->

Unfortunately, AMD script loaders, such as [Require.js](http://requirejs.org/), are not drop-in solutions.  To use an AMD module system, you must make sure every other JavaScript library used in your application is _AMD Compatible_.

###AMD Compatible

For a JavaScript file to be _AMD Compatible_, the JavaScript content must be wrapped in a `define()` method and all module (file) dependencies must be listed.

A possible solution is to change the source of each JavaScript file you are using and make sure the content is wrapped inside of a `define()` method.  This is both time-intensive and error-prone, since you will even have to change the source of third-party libraries that your application is using, such as jQuery.

At this point you may be scratching your head and asking yourself a few questions...

<!-- more -->

###Common Questions

**Why don't third-party libraries automatically wrap their code in a define method?**

Third-party libraries, such as jQuery, are often used in many different development environments, and the jQuery team cannot assume that jQuery is being used in an AMD environment.  If jQuery was not being used in an AMD environment, then a syntax error would be thrown complaining that `the define method is undefined`.

Luckily for us, jQuery and many other popular libraries have found a way to support the AMD API (with the help of the always awesome [James Burke](https://github.com/jrburke))!  When jQuery is executed, it checks to see if an AMD script loader is being used, and then conditionally wraps itself in a `define()` method and declares itself as a **named AMD module**. 

**Is there a better way to load a non-AMD Compatible third-party script with Require.js then upgrading the source of a file?**
Yes, with the Require.js v2.0 release, the `shim` configuration option was added to _shim_ third-party libraries that were not AMD compatible and exported a global object.  You can read more about the `shim` configuration [here](http://gregfranko.com/blog/require-dot-js-2-dot-0-shim-configuration/).

**Does the shim configuration solve all of the AMD Compatibility problems?**
No!  Although the `shim` configuration works for the most common uses cases, it is still not the ideal approach.  Shimmed third-party scripts cannot be loaded from a CDN after a build, since shimmed files are inlined by the builder and may be executed by Require.js before a CDN asset dependency.  The best approach is making sure a file is wrapped in a `define()` method (this works with CDN assets).

###jQuery AMD Support
The first FAQ above mentioned that jQuery supports the AMD API by conditionally wrapping itself in a `define()` method and declaring itself as **named AMD module**.  Let's take a look at the jQuery source to see how they are doing it:

{% codeblock lang:js %}
if ( typeof define === "function" && define.amd && define.amd.jQuery ) {
    define( "jquery", [], function () { return jQuery; } );
}
{% endcodeblock %}

The jQuery code is checking to make sure that a global `define()` method is on the page, that there is an `amd` property (object) set on the `define` method (Remember this is JavaScript and functions act as first-class objects and can have properties), and that the `amd` object contains a `jQuery` property that is set to true.  jQuery is essentially checking that an AMD script loader is on the page and that the AMD loader handles the special case when more than one jQuery version is included on the page (this is unfortunately very common).

If an AMD loader satisfies all of the jQuery AMD checks, then jQuery will wrap itself in the global `define()` method, register itself as a named module ("jquery"), and return itself (the jQuery object) within the `define()` method callback function.

**Note:**  Registering **named** AMD modules is usually not recommended, since it hard codes a module id and does not allow you to change the module name, making it inflexible.  The reason most popular third-party libraries register as **named** AMD modules is because many other libraries depend on them, and desire a common name that they can list as a dependency.

###Using jQuery with Require.js
Now that we know jQuery is AMD compatible, we can easily use jQuery with a Require.js configuration like this:

{% codeblock lang:js %}
// Sample Configuration File
// -------------------------
require.config({

  // Sets the js folder as the base directory for all future relative paths
  baseUrl: "./js",

  // 3rd party script alias names (Easier to type "jquery" than "libs/jquery, etc")
  paths: {

      // Core Libraries
      // --------------
      "jquery": "libs/jquery"

  }

});

// Includes Desktop Specific JavaScript files here (or inside of your Desktop router)
require(["jquery"],

  function($) {

    console.log("Hey look, I'm using jQuery with Require.js!");

  }

);
{% endcodeblock %}

###Using jQuery Plugins with Require.js
Although jQuery supports the AMD API, that does not mean that jQuery plugins are also AMD compatible.  Here is how we could load a jQuery plugin that does not support the AMD API:

{% codeblock lang:js %}
// Sample Configuration File
// -------------------------
require.config({

  // Sets the js folder as the base directory for all future relative paths
  baseUrl: "./js",

  // 3rd party script alias names (Easier to type "jquery" than "libs/jquery, etc")
  paths: {

      // Core Libraries
      // --------------
      "jquery": "libs/jquery",

      "example": "libs/plugins/example"

  },

  shim: {

    "example": ["jquery"]

  }

});

// Includes Desktop Specific JavaScript files here (or inside of your Desktop router)
require(["jquery", "example"],

  function($) {

    console.log("Hey look, I'm using a jQuery plugin that does not support the AMD API with Require.js!");

  }

);
{% endcodeblock %}

The above code example uses the `shim` configuration to tell Require.js that the plugin depends on jQuery.  Internally, Require.js waits until jQuery is loaded before it loads the jQuery plugin.

Luckily for plugin authors, a jQuery plugin can internally support the AMD API:

{% codeblock lang:js %}
// Example plugin
(function (factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD. Register as an anonymous module.
        define(['jquery'], factory);
    } else {
        // Browser globals
        factory(jQuery);
    }
}(function ($) {
    $.fn.jqueryPlugin = function () {};
}));
{% endcodeblock %}

This jQuery plugin example checks to make sure an AMD loader is on the page and then registers itself as an **anonymous** AMD module (while also passing the "jquery" module as a dependency).  Since jQuery registers itself as a named AMD module, the plugin is able to pass that exact name as a dependency.

**Note:** You can find other AMD patterns [here](https://github.com/umdjs/umd);

Here is how you would load the AMD Compatible jQuery plugin with Require.js:

{% codeblock lang:js %}
// Sample Configuration File
// -------------------------
require.config({

  // Sets the js folder as the base directory for all future relative paths
  baseUrl: "./js",

  // 3rd party script alias names (Easier to type "jquery" than "libs/jquery, etc")
  paths: {

      // Core Libraries
      // --------------
      "jquery": "libs/jquery",

      "example": "libs/plugins/example"

  }

});

// Includes Desktop Specific JavaScript files here (or inside of your Desktop router)
require(["jquery", "example"],

  function($) {

    console.log("Hey look, I'm using a jQuery plugin with Require.js!");

  }

);
{% endcodeblock %}

###jQueryUI Widget Factory Plugins
The above code example is a great solution for jQuery plugins to register themselves as anonymous AMD modules, but let's next look at jQuery plugins that depend on both jQuery and the **jQueryUI Widget Factory**.

<a href="http://api.jqueryui.com/jQuery.widget/" target="_blank">{% img center /images/jqueryui.png %}</a>

In case you are not familiar, the jQueryUI Widget Factory _provides a flexible base for building complex, stateful plugins with a consistent API._ - per the jQueryUI website.

The Widget Factory is designed not only for plugins that are part of jQuery UI, but for general consumption by developers who want to create object-oriented components without reinventing common infrastructure.

Since more and more jQuery plugin authors, including myself, are using the jQueryUI Widget Factory, it is necessary to understand how to use these jQuery plugins with AMD loaders such as Require.js.

Since the jQueryUI Widget Factory does not support the AMD API, here is how you currently have to load jQuery plugins that depend on the jQueryUI Widget Factory:

{% codeblock lang:js %}
// Sample Configuration File
// -------------------------
require.config({

  // Sets the js folder as the base directory for all future relative paths
  baseUrl: "./js",

  // 3rd party script alias names (Easier to type "jquery" than "libs/jquery, etc")
  paths: {

      // Core Libraries
      // --------------
      "jquery": "libs/jquery",

      "jquery.ui.widget": "libs/jquery.ui.widget",

      "jquery.selectBoxIt": "libs/plugins/jquery.selectBoxIt"

  },

  shim: {

    "jquery.ui.widget": ["jquery"],

    "jquery.selectBoxIt": ["jquery.ui.widget"]

  }

});

// Includes Desktop Specific JavaScript files here (or inside of your Desktop router)
require(["jquery", "jquery.ui.widget", "jquery.selectBoxIt"],

  function($) {

    $("select").selectBoxIt();

    console.log("Hey look, I'm using a jQueryUI Widget Factory plugin with Require.js!");

  }

);
{% endcodeblock %}

The above code uses the `shim` configuration to shim both the jQueryUI Widget Factory and [SelectBoxIt](https://github.com/gfranko/jquery.selectBoxIt.js), a jQuery plugin based on the jQueryUI Widget Factory.  This works, but requires additional configuration setup and the `shim` configuration, which prevents the loading of CDN assets after building using the Require.js optimizer.

After experiencing this first hand, I tweeted my desire that the jQueryUI Widget Factory should declare itself as a **named** AMD module, so that plugins (like SelectBoxIt) could list the Widget factory as a dependency and support the AMD API.

<blockquote class="twitter-tweet"><p>@<a href="https://twitter.com/jrburke">jrburke</a> You should convince the jQueryUI devs to declare the Widget Factory as a named module, like jQuery.Lot's of plugins depend on it.</p>&mdash; Greg Franko (@GregFranko) <a href="https://twitter.com/GregFranko/status/299184644887306240">February 6, 2013</a></blockquote>

Dave Methvin, the President of the jQuery Foundation, and Scott Gonzalez, the jQueryUI Project lead, (both all around good guys), responded by asking me if James Burke's [jqueryui-amd](https://github.com/jrburke/jqueryui-amd) project would suit my needs.

<blockquote class="twitter-tweet"><p>@<a href="https://twitter.com/gregfranko">gregfranko</a> Yes, @<a href="https://twitter.com/scott_gonzalez">scott_gonzalez</a> says you're taking me on a mandate? Let's gobowling. Does <a href="https://t.co/oE5mSOTg" title="https://github.com/jrburke/jqueryui-amd">github.com/jrburke/jquery…</a> work for you? @<a href="https://twitter.com/jrburke">jrburke</a></p>&mdash; Dave Methvin (@davemethvin) <a href="https://twitter.com/davemethvin/status/299595913150726145">February 7, 2013</a></blockquote>

<blockquote class="twitter-tweet"><p>@<a href="https://twitter.com/scott_gonzalez">scott_gonzalez</a> @<a href="https://twitter.com/davemethvin">davemethvin</a> @<a href="https://twitter.com/jrburke">jrburke</a> I'll write a blog post and let you know when it is done.</p>&mdash; Greg Franko (@GregFranko) <a href="https://twitter.com/GregFranko/status/299613588459495424">February 7, 2013</a></blockquote>

If you are not familiar with James Burke's [jqueryui-amd](https://github.com/jrburke/jqueryui-amd) project, it is a Node.js command line project that will automatically wrap jQueryUI modules within `define()` methods and pass jQuery as a dependency, thus making jQueryUI _AMD Compatible_:

{% codeblock lang:js %}
define(['jquery'], function (jQuery) {
    // jQueryUI code goes here
});
{% endcodeblock %}

This is a great solution if you are only using jQueryUI plugins (i.e. Calendar widget, etc), but not such a great solution for third-party plugin authors, like myself, who are using the jQueryUI Widget Factory as the base for our plugin structures.

Here is how you would load a jQueryUI Widget Factory plugin with Require.js, when using the **jqueryui-amd** project:

{% codeblock lang:js %}
// Sample Configuration File
// -------------------------
require.config({

  // Sets the js folder as the base directory for all future relative paths
  baseUrl: "./js",

  // 3rd party script alias names (Easier to type "jquery" than "libs/jquery, etc")
  paths: {

      // Core Libraries
      // --------------
      "jquery": "libs/jquery",

      "jquery.ui.widget": "libs/jquery.ui.widget",

      "jquery.selectBoxIt": "libs/plugins/jquery.selectBoxIt"

  },

  shim: {

    // Shimming SelectBoxIt by telling Require.js that the plugin depends on the jQueryUI Widget Factory
    "jquery.selectBoxIt": ["jquery.ui.widget"]

  }

});

// Includes Desktop Specific JavaScript files here (or inside of your Desktop router)
require(["jquery", "jquery.ui.widget", "jquery.selectBoxIt"],

  function($) {

    $("select").selectBoxIt();

    console.log("Hey look I'm using a jQueryUI Widget Factory plugin with Require.js!");

  }

);
{% endcodeblock %}

In the above code example, you'll notice that jQueryUI Widget Factory no longer needs to use the shim configuration, since the **jqueryui-amd** project has wrapped it within a `define()` method.  Unfortunately, the SelectBoxIt plugin still needs to be shimmed, since it is not AMD compatible.

By now you may be thinking, couldn't you just support the AMD API within SelectBoxIt by doing this:

{% codeblock lang:js %}
;(function (selectBoxIt) {

    if (typeof define === "function" && define.amd) {
        // AMD. Register as an anonymous module.
        define(["jquery", "jquery.ui.widget"], function() {

            selectBoxIt(window.jQuery, window, document);

        });
    } else {
        // Browser globals
        selectBoxIt(window.jQuery, window, document);
    }

}
(function ($, window, document, undefined) {

    // SelectBoxIt code goes here

})); // End of all modules
{% endcodeblock %}

Technically yes, I could conditionally support the AMD API by using the above code.  The problem is that it would be a very confusing process for plugin users, since I would have to point them to the **jqueryui-amd** project, which is not trivial to set up.  Or I could maintain an up-to-date AMD Compatible version of the jQueryUI Widget Factory. Not an ideal process.

It would be much easier to tell users to create a Require.js path called, "jquery.ui.widget", and be done with it!

##Upgrading the jQueryUI Widget Factory
Following the lead of jQuery, I believe the jQueryUI Widget Factory should support the AMD API by registering as a named AMD module that Widget Factory plugins can declare as a dependency:

{% codeblock lang:js %}
;(function (widgetFactory) {

    if (typeof define === "function" && define.amd) {
        // AMD. Register as an anonymous module.
        define( "jquery.ui.widget", ["jquery"], function() {

            widgetFactory( window.jQuery );

        } );
    } else {
        // Browser globals
        widgetFactory( window.jQuery );
    }
}
(function( $, undefined ) {

    // jQuery Widget Factory code goes here

}));
{% endcodeblock %}

I have listed "jquery.ui.widget" to be the default name of the Widget Factory, but that is only my personal opinion and may be subject to change.  If this change is put into the jQueryUI Widget Factory, then the thousands of plugins that are based on it (probably more), could easily support the AMD API.  This means that no Require.js configurations/extra steps would be necessary for you to use a jQuery plugin based on the Widget Factory.

**Note:** Check out this [gist](https://gist.github.com/gfranko/4750778) to see the full source of the AMD Compatible jQueryUI Widget Factory that I am proposing.

##Final Thoughts
It only takes a couple lines of code to support the AMD API, so I feel that the jQueryUI Widget Factory should follow in the footsteps of its parent's favorite child, jQuery, and just do it (I am not sponsored by Nike).  I'd love to hear your thoughts!  Happy AMD'ing!
