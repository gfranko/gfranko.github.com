---
layout: post
title: "Backbone.validateAll plugin"
date: 2012-09-18 14:56
comments: true
categories: [JavaScript, Backbone.js]
description: "Backbone.js plugin that provides an option to only validate Model properties that are currently being set or saved"
keywords: "Greg Franko, Backbone.js, Backbone.js validateAll" 
---

**[Backbone.validateAll](https://github.com/gfranko/Backbone.validateAll)** is a small Backbone.js plugin that provides an option to only validate Model properties that are currently being set or saved.

<!-- more -->

{% img center /images/backbone.png %}

###Background
_Backbone.validateAll_ originated from a failed Backbone.js [pull request](https://github.com/documentcloud/backbone/pull/1595).  The original pull request was created because of frustration with using the Backbone.js Model validate method when validating HTML forms.

###Backbone.js v0.9.1 Changes
Since Backbone.js v0.9.1 and greater, Backbone Model validation was not made to elegantly handle form validation, since the default validate method will validate all Model attributes, regardless of what particular attribute is being set or saved. For certain use cases, it is necessary to only validate a certain Model property (or form field) without worrying about the validation of any other Model property (or form field).

###Performance!
Backbone core contributor, [@braddunbar](https://github.com/braddunbar), presented a possible solution for this use case in the above mentioned [pull request](https://github.com/documentcloud/backbone/pull/1595), but it still involved calling all of the validation methods within the `validate()` method, which can negatively affect performance.

Here is a [jsPerf Test](http://jsperf.com/backbone-validateall) showing the performance benefits when setting Backbone Model attributes with and without _Backbone.validateAll_.

<!-- more -->

###Who Should Use This
Anyone who wants an **option** to validate only Model properties that are currently being set/saved instead of the entire Model.  An obvious use case for this is form validation.

**Note**: The plugin does not override the default Backbone.js Model validation behavior (all Model attributes are validated whenever an attribute is saved/set on a Model) by default.  You need to pass the **validateAll** option when setting/saving Model attributes.

###Is it easy to use the plugin?
Yes!  You just have to pass the validateAll option when setting/saving a Model attribute.

{% codeblock lang:js %}

    // The validateAll option makes sure that only the Model attributes that you are setting get passed to the validate method
    user.set({ "firstname": "Greg" }, {validateAll: false});

{% endcodeblock %}

###Summary
If you have found yourself frustrated about Backbone's default Model validation behavior, then this plugin is a perfect fit.  You can also read more about this plugin in Addy Osmani's [Backbone Fundamentals](http://addyosmani.github.com/backbone-fundamentals/) book under the section titled **Better Model Property Validation**.  Happy Model validating!
