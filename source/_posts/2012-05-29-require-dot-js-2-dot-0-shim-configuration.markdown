---
layout: post
title: "Require.js 2.0 Shim Configuration"
date: 2012-05-29 15:40
comments: true
categories: [JavaScript, Backbone.js, Require.js]
description: "Require.js 2.0 Shim Configuration Now Supports Loading Non-AMD Compatible Scripts"
keywords: "Greg Franko, Require.js, Require.js 2.0, Loading Non-AMD Compatible Scripts, Greg Franko"
cover: "/images/requirejs.png"
---

Require.js 2.0 was recently released by James Burke, and with it comes a bunch of bug fixes and enhancements.  The major enhancement that I wanted to shed light on includes the new `Shim` configuration.

<!-- more -->

{% img center /images/requirejs.png %}

The `Shim` configuration is a much needed upgrade to the Require.js core that allows Require.js to load non-AMD compatible scripts.  In my previous post [Using Backbone.js with Require.js](http://gregfranko.com/blog/using-backbone-dot-js-with-require-dot-js/), I covered how Use.js, a Require.js plugin created by Backbone.js core contributor <a href="http://tbranyen.com/" target="_blank">Tim Branyen</a>, was necessary to load Backbone.js and Underscore.js (both AMD incompatible) with Require.js.  This was a good solution for integrating these two wonderful projects together, but it was still another project dependency that you needed to keep track of.

James Burke mentions Use.js as one of his inspirations for including the new Shim configuration.  Now, instead of including the Use.js plugin to load Backbone and Underscore, you can have a shim configuration in your main file like this:

{% codeblock lang:js %}
  // Sets the configuration for your third party scripts that are not AMD compatible
  shim: {

      "backbone": {
          deps: ["underscore", "jquery"],
          exports: "Backbone"  //attaches "Backbone" to the window object
      }

  } // end Shim Configuration
{% endcodeblock %}

The `Shim` configuration is also a replacement for the **order** plugin, which made sure certain scripts executed in a particular order.  Example use cases for this include loading jQuery and Backbone plugins, since Backbone must be executed before Backbone plugins are executed, and jQuery must be executed before jQuery plugins are executed.  Below is how you can load jQuery and Backbone plugins using Require.js 2.0 or greater:

{% codeblock lang:js %}
  // Sets the configuration for your third party scripts that are not AMD compatible
  shim: {

        'jquery.selectBoxIt': ['jquery'],
        'backbone.Validation': ['backbone']

  } // end Shim Configuration
{% endcodeblock %}


For more documentation on all the new changes in Require.js 2.0, head over to the [Require.js Github Wiki](https://github.com/jrburke/requirejs/wiki/Upgrading-to-RequireJS-2.0).  Happy AMD'ing!
