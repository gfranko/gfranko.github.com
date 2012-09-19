---
layout: post
title: "Creating Custom Project Builds with DownloadBuilder.js"
date: 2012-09-17 14:52
comments: true
categories: [JavaScript]
description: "DownloadBuilder.js helps you create custom project builds for your open source projects"
keywords: "Greg Franko, DownloadBuilder.js, Custom Project Builds" 
---

{% img left http://gregfranko.com/DownloadBuilder.js/img/html5.png 205 %}
<br /><br /><br /><br /><br /><br />

###Please, Just Give Me What I Want
Many of the most popular Open Source (OS) projects understand that flexibility and performance is very important for users.  Since it is nearly impossible to create a project that is a perfect fit for all users, many OS projects provide a custom build option.  This allows users to specify which parts of a project he/she wants and then download only those parts (thus minimizing file size).

_Examples of popular OS projects that are providing custom build options now include jQuery (recent addition in version 1.8), jQueryUI, Modernizr, Twitter Bootstrap, Can.js, etc._

After reviewing the custom build processes of the above OS projects, it seemed that each project had developed its own custom back-end build process from scratch.  This architecture seemed to work well for projects with many contributors/organizational backing, but I wanted an easy solution that I could use for my own OS projects where I was the only main contributor.  Since I could not find any existing OS projects that fit the bill, I created **DownloadBuilder.js**.

###DownloadBuilder.js

####Description
[DownloadBuilder.js](http://gregfranko.com/DownloadBuilder.js/) is a JavaScript library that uses the HTML5 Filesystem API to create concatenated single file custom builds for front-end projects.  Since the project uses the HTML5 Filesystem API, all of its features only currently work in Chrome 21+, although it can still be used in all other modern browsers(Firefox, IE8+) to create a generated concatenated project file.

<!-- more -->

####Getting Started

**Note:** DownloadBuilder.js expects your project to be decoupled and split into separate files (modular approach).

Download the [entire project](https://github.com/gfranko/DownloadBuilder.js/zipball/master) from Github and look at the examples in the *demos* folder.

####Include DownloadBuilder.js

{% codeblock lang:html %}
    // DownloadBuilder.js
    <script src="DownloadBuilder.js" />
{% endcodeblock %}

#####HTML
Create one checkbox for each file in your project that you want to allow users to download. Each checkbox value should be the full file path. 

**Note:** You can use Github or local relative file paths. This example uses Github to download your project files.

{% codeblock lang:html %}
    // A checkbox with the correct Github filepath of testfile.js
    <label class="checkbox"><input checked="checked" type="checkbox" value="src/javascripts/testfile.js">Test File</label>
{% endcodeblock %}

#####JavaScript
Using a library (eg. jQuery) or regular JavaScript, create a new DownloadBuilder object instance with custom options when the DOM is ready.

**Note:** You do not need to use a library, such as jQuery. I am using it here for convenience. Also, all options are optional and more documentation about all of the options can be found in the documentation.

{% codeblock lang:js %}
    //Executes your code when the DOM is ready.  Acts the same as $(document).ready().
    $(function() {

        // All options passed to the DownloadBuilder object constructor are optional
        // This example is a sample Github setup
        var builder = new DownloadBuilder({ "location": "github", "author": "gfranko", "repo": "jquery.selectboxit.js", "branch": "dev" });

    });
{% endcodeblock %}

Next, create an event handler for when you want to create your custom built file, call the `buildURL()` method to construct a file url (if supported), and then create custom logic once your custom built file has been created (the file is passed into the buildURL callback function).

**Note:** More documentation about the buildURL method and other methods can be found in the documentation.

{% codeblock lang:js %}
    // Whenever an element with an id of javascript-generate is clicked, the buildURL() method is called
    $("#javascript-generate").on("click", function() {

        // Pass in all of the checkboxes that should be used to download files, the desired name of the file to be created,
        // the type of file to be created, and a callback function
        builder.buildURL($("#javascript-downloads input[type='checkbox']:checked"), "example.js", "javascript", function(data) {

            // An object is passed to the callback function which contains a file url
            // (if the current browser supports the HTML5 Filesystem API), file content (the text of the file),
            // the name of the file that was created, and the type of file that was created.

            // Put any custom logic here

        });

    });
{% endcodeblock %}

Below is the full **DownloadBuilder.js** example:

{% codeblock lang:js %}
    //Executes your code when the DOM is ready.  Acts the same as $(document).ready().
    $(function() {

        // All options passed to the DownloadBuilder object constructor are optional
        // This example is a sample Github setup
        var builder = new DownloadBuilder({ "location": "github", "author": "gfranko", "repo": "jquery.selectboxit.js", "branch": "dev" });

        // Whenever an element with an id of javascript-generate is clicked, the buildURL() method is called
        $("#javascript-generate").on("click", function() {

            // Pass in all of the checkboxes that should be used to download files, the desired name of the file to be created,
            // the type of file to be created, and a callback function
            builder.buildURL($("#javascript-downloads input[type='checkbox']:checked"), "example.js", "javascript", function(data) {

                // An object is passed to the callback function which contains a file url
                // (if the current browser supports the HTML5 Filesystem API), file content (the text of the file),
                // the name of the file that was created, and the type of file that was created.

                // Put any custom logic here

            });

        });

    });
{% endcodeblock %}

###Summary

[DownloadBuilder.js](https://github.com/gfranko/DownloadBuilder.js) is a great fit for small OS projects that are only worried about creating a single file custom build.  If there is a feature that you would like to see added to DownloadBuilder.js, feel free to create an issue or fork the project the project on Github.  Happy custom building!





