---
layout: post
title: "Introducing the jQuery Plugin SelectBoxIt"
date: 2012-04-14 00:11
comments: true
categories: [JavaScript, jQuery, Forms, jQueryUI]
description: "A jQuery single option select box plugin that can be optionally styled using ThemeRoller and optionally animated with jQueryUI show/hide effects"
keywords: "Greg Franko, SelectBoxIt, jQuery select box plugin, jQuery select box, jQueryUI select box, jQuery, jQuery form styling"
---

<link href="{{ root_url }}/stylesheets/jQuery.selectBoxIt.css" rel="stylesheet" type="text/css">
<script src="{{ root_url }}/javascripts/jQuery.selectBoxIt.min.js" type="text/javascript"></script>
<script src="http://jqueryui.com/themeroller/themeswitchertool/"></script>
<script src="{{ root_url }}/javascripts/jquery.tableofcontents.min.js"></script>

<script type="text/javascript">
    $(function() {
        $("#toc").tableOfContents(null, { startLevel: "2", depth: "2" }); 
        var selectBox = $("select#test, select#icons").selectBoxIt({ showEffect: "fadeIn", showEffectSpeed: 300, hideEffect: "fadeOut", hideEffectSpeed: 300 }).data("selectBoxIt");
        $("#switcher").themeswitcher();
        var effectsSelectBox = $("select#effectsDropDown").selectBoxIt().data("selectBoxIt");
        $("#effectsButton").click(function() {
            var effect = $("select#effectsDropDown").val();
            selectBox.setOptions({ showEffect: effect, hideEffect: effect });
            if(!selectBox.list.is(":visible")) {
                selectBox.open();
            }
            else {
                selectBox.close();   
            }
        });
        $(".ui-button").mouseenter(function() {
            $(this).addClass("ui-state-hover");
        });
        $(".ui-button").mouseleave(function() {
            $(this).removeClass("ui-state-hover");
        });
        var upArrowSelectBox = $("select#upArrow").selectBoxIt().data("selectBoxIt");

        $("select#upArrow").bind({
	        "open": function() {
                upArrowSelectBox.setOption("downArrowIcon", "ui-icon ui-icon-triangle-1-n");
            },
            "close": function() {
                upArrowSelectBox.setOption("downArrowIcon", "ui-icon ui-icon-triangle-1-s");
            }
        });

        var optgroupsSelectBox = $("select#optgroups").selectBoxIt().data("selectBoxIt");
    });
</script>
##What is SelectBoxIt?
<select id="test" name="test">
<option value="SelectBoxIt is:">SelectBoxIt is:</option>
<option value="a jQuery Plugin">a jQuery Plugin</option>
<option value="a Select Box Replacement">a Select Box Replacement</option>
<option value="a Stateful UI Dropdown Widget">a Stateful UI Dropdown Widget</option>
</select>
<br /><br />
A jQuery plugin that progressively enhances an HTML Select Box into a **single option** dropdown list.  The dropdown list can be optionally styled with **ThemeRoller** (_Preview a Theme Below_) and **jQueryUI/custom image icons**, and optionally animated with **jQueryUI show/hide effects** (_Preview a jQueryUI Effect Below_).

<div style="float:left;padding-right:100px;"><em>Preview a Theme</em>  <div id="switcher"></div></div>
<div style="float:left;"><em>Preview a jQueryUI Effect</em>
<br />
<select class="effects" name="effects" id="effectsDropDown">
<option value="blind">blind</option>
<option value="bounce">bounce</option>
<option value="clip">clip</option>
<option value="drop">drop</option>
<option value="explode">explode</option>
<option value="fold">fold</option>
<option value="highlight">highlight</option>
<option value="puff">puff</option>
<option value="scale">scale</option>  
<option value="shake">shake</option>
<option value="size">size</option>
<option value="slide">slide</option>
</select>

<button id="effectsButton" class="ui-button ui-state-default ui-corner-all ui-button-text-only" role="button" aria-disabled="false">
	<span class="ui-button-text">Run Effect</span>
</button>
</div>
<br /><br /><br /><br />
The project is hosted on <a href="https://github.com/gfranko/jQuery.selectBoxIt.js" target="_blank" class="projectLinks">Github</a>, has a <a href="http://www.selectboxit.com" target="_blank">project page</a> the <a href="{{ root_url }}/docs/jQuery.selectBoxIt.html" target="_blank" class="projectLinks">annotated source code</a> is available, and an <a href="{{ root_url }}/test/SpecRunner.html" target="_blank" class="projectLinks">online test suite</a> is also available.  SelectBoxIt is available for use under the <a href="https://github.com/gfranko/jQuery.selectBoxIt.js/blob/master/MIT-LICENSE.txt" target="_blank" class="projectLinks">MIT software license</a>.  You can report bugs and discuss features on the <a href="https://github.com/gfranko/jQuery.selectBoxIt.js/issues?sort=created&direction=desc&state=open" target="_blank">GitHub issues page</a>, or send tweets to <a href="http://www.twitter.com/gregfranko" target="_blank">@gregfranko</a>.
<br />
<!-- more -->

##Table of Contents

<ul id="toc" style="list-style:none;background:#F0F0F0;"></ul>

##Downloads and Dependencies

SelectBoxIt depends on jQuery 1.6.1+ (It is always recommended to use the latest version of jQuery) and the latest version of the jQueryUI Widget Factory.  If you want to style your dropdown list with jQueryUI, download a default <a href="http://jqueryui.com/themeroller/" target="_blank">jQueryUI CSS Theme</a> or create your own theme.  If you want to use jQueryUI for advanced show/hide effects, download the latest version of the <a href="http://jqueryui.com/download" target="_blank">jQueryUI effects</a> (only the custom **Effects** you use are required).

<a href="https://raw.github.com/gfranko/jQuery.selectBoxIt.js/master/src/javascripts/jQuery.selectBoxIt.js" target="_blank" style="display:inline-block;">
<button id="effectsButton" class="ui-button ui-state-default ui-corner-all ui-button-text-only" role="button" aria-disabled="false" style="margin:0;width:200px;font-size:16px;padding:0px;">
<span class="ui-button-text">Development Version</span>
</button>
</a>
<span class="ui-button-text">72kb, Full source, lots of comments</span>

<a href="https://raw.github.com/gfranko/jQuery.selectBoxIt.js/master/src/javascripts/jquery.selectBoxIt.min.js" target="_blank" style="display:inline-block;">
<button id="effectsButton" class="ui-button ui-state-default ui-corner-all ui-button-text-only" role="button" aria-disabled="false" style="margin:0;width:200px;font-size:16px;padding:0px;">
<span class="ui-button-text">Production Version</span>
</button>
</a>
<span class="ui-button-text">3.5kb, Packed and gzipped</span>
<br />

##Why Use SelectBoxIt?
<a href="http://jqueryui.com/themeroller/" target="_blank">{% img images /images/themeroller.png 120 130 jQueryUI Themeroller %}</a> <a href="http://docs.jquery.com/UI/Effects/" target="_blank">{% img images /images/jqueryui.png 120 130 jQueryUI Effects %}</a>
**SelectBoxIt** gives you full control to style and customize your HTML Select Box to fit the look and feel of your site.  Integration with **jQueryUI** is built-in to provide advanced stying and custom effects.

SelectBoxIt was built to make a developers life easier, not harder.  All of your current JavaScript validation code will continue to work.  If you want to enhance your validation/UI logic, you can listen to the over **20 events** and call the **14 methods** provided by SelectBoxIt.  
<br />

##Notable Features
-Built-in **ARIA support** (Accessible Rich Internet Applications)<br /><br />
-Full **keyboard search and navigation support**<br /><br />
-Handful of **customizable options**<br /><br />
-An **event API** triggered on the original select box element that calls the plugin<br /><br />
-A **method API** providing developers with methods to interact with the dropdown list (i.e. Search, Open, Disable, Set Options).<br /><br />
-**Selected**, **disabled**, and **optgroup** support<br /><br />
-Easily **extendable** to allow developers to create new widgets
<br />

##Browser Support 
Tested in IE7+, Firefox 4+, Chrome, Safari 4+, and Opera 11+.

Accessibility Testing was done in IE7+ and Firefox 4+ using JAWS 12, a proprietary screen reader.
<br />

##Getting Started

**1.**  Download the latest <a href="https://github.com/gfranko/jQuery.selectBoxIt.js/zipball/master">SelectBoxIt Project Folder</a>.  All of the plugin files are in the `src` directory (this includes both the JavaScript and CSS files).  If you want to see an example page with all of the files included, look in the `demos` directory. 

**2.**  Go to the <a href="http://jqueryui.com/download" target="_blank">jQueryUI Custom Download Page</a>.  This is where we will be downloading the jQueryUI Widget Factory and potentially a jQueryUI CSS Theme and jQueryUI show/hide animations.

-Once you are at the jQueryUI custom download page, check the box to download the <a href="http://jqueryui.com/download" target="_blank">jQueryUI Widget Factory</a>.

<a href="http://jqueryui.com/download" target="_blank">
{% img /images/widgetFactory.PNG 800 530 jQueryUI Widget Factory %}
</a>

-Next, If you are going to use jQueryUI to style your dropdown list (you should), then make sure to pick a jQueryUI CSS theme to download.  Alternatively, you can create your own theme or look at all of the jQueryUI default themes with <a href="http://jqueryui.com/themeroller/" target="_blank">jQueryUI ThemeRoller</a>.  

<a href="http://jqueryui.com/download" target="_blank">
{% img /images/theme.PNG 250 230 jQueryUI Theme %}
</a>  

You may also use a Google or Microsoft CDN hosted version of a default jQueryUI CSS theme instead of downloading a copy locally to your computer.  Replace the `[UI.VERSION]` with the jQueryUI version you are using (i.e. _1.8.18_) and also replace the `[THEME-NAME]` with the default theme name you are using (i.e. _smoothness_).

**Google**: `http://ajax.googleapis.com/ajax/libs/jqueryui/[UI.VERSION]/themes/[THEME-NAME]/jquery-ui.css`
<br />
**Microsoft**
`http://ajax.aspnetcdn.com/ajax/jquery.ui/[UI.VERSION]/themes/[THEME-NAME]/jquery-ui.css`

Next, if you are using jQueryUI for custom show/hide effects, then make sure to check the `Effects Core` box, and any of the custom animations you would like to use.

Finally, click the `Download` button to download your custom jQueryUI files!

<a href="http://jqueryui.com/download" target="_blank">
{% img /images/jqueryUIEffects.png 800 530 jQueryUI Effects %}
</a>

**3.**  
###HTML

Create an HTML select box with `id` and `name` attributes.  The `name` attribute from the HTML select box will be copied to the new dropdown list that the plugin creates (this allows you to easily interact with the new dropdown list without having to know a new `id` or `class` attribute).  Each select box option should have a `value` attribute.
{% codeblock lang:html %}
<select id="test" name="test">
<option value="SelectBoxIt is:">SelectBoxIt is:</option>
<option value="a jQuery Plugin">a jQuery Plugin</option>
<option value="a Select Box Replacement">a Select Box Replacement</option>
<option value="a Stateful UI Dropdown Widget">a Stateful UI Dropdown Widget</option>
</select>
{% endcodeblock %}

Your select box option `value` attributes and text also do not have to be the same.  This would also work:
{% codeblock lang:html %}
<select id="test" name="test">
<option value="someValue">SelectBoxIt is:</option>
<option value="jQuery">a jQuery Plugin</option>
<option value="select box">a Select Box Replacement</option>
<option value="widget">a Stateful UI Dropdown Widget</option>
</select>
{% endcodeblock %}

**SelectBoxIt** also supports the `selected` and `disabled` HTML properties.  Keep in mind that the last select box option to contain the `selected` property will be the select box option that the dropdown list uses as it's default option.  Also, the `disabled` property can be used to disable the entire dropdown or specific dropdown options.

Here is an example of setting a select box option as the `selected` option:
{% codeblock lang:html %}
<select id="test" name="test">
<option value="someValue">SelectBoxIt is:</option>
<option value="jQuery">a jQuery Plugin</option>
<option value="select box">a Select Box Replacement</option>
<option value="widget" selected>a Stateful UI Dropdown Widget</option>
</select>
{% endcodeblock %}

Here is an example of setting the `disabled` property for multiple select box options:
{% codeblock lang:html %}
<select id="test" name="test">
<option value="someValue">SelectBoxIt is:</option>
<option value="jQuery" disabled>a jQuery Plugin</option>
<option value="select box">a Select Box Replacement</option>
<option value="widget" disabled>a Stateful UI Dropdown Widget</option>
</select>
{% endcodeblock %}


**4.** 
###CSS


-Customize **jquery.selectBoxIt.css** (Downloaded in Step 1) and then add it to the _head_ section of your HTML page.  The CSS file by default expects a select box element with an `id` attribute of _test_.  Replace the word _test_ in all of the CSS selectors with the unique `id` attribute of your HTML select box (if you don't use _test_ as your `id` attribute).

-If you are using jQueryUI to style your dropdown list, then add the latest **jQueryUI CSS file** (i.e. _jquery-ui-1.8.18.custom.css_) to the _head_ section of your HTML page.  This is downloaded in Step 2.  If you are using a Google or Microsoft CDN hosted version of a default jQueryUI theme, then you do not need to download anything, just include the hosted css file in the _head_ section of your HTML page.

-If you are using jQueryUI to style your dropdown list, then add all of the jQueryUI images to your images folder.  Keep in mind that depending on your folder structure, you may need to update the image paths inside of the jQueryUI CSS file.

**Note:**  This assumes that you have downloaded both of the css files to the same folder as your HTML page.  Modify the `href` attribute if you do not store the CSS files in the same folder as your HTML page.  Also, if you are **not** using jQueryUI to style your dropdown list, then you will most likely want to add additional CSS attributes to **jquery.selectBoxIt.css** to compensate.

{% codeblock lang:html %}
<head>
  <link type="text/css" rel="stylesheet" href="jquery.selectBoxIt.css" />
  <link type="text/css" rel="stylesheet" href="jquery-ui-1.8.18.custom.css" />
</head>
{% endcodeblock %}

**6.**  
###JavaScript

-If you are not already including **jQuery** in your HTML page, download <a href="http://jquery.com/" target="_blank">jQuery</a>, and add it immediately before the closing `body` tag.

-Next, add your custom **jQueryUI** JavaScript file (i.e. _jquery-ui-1.8.19.custom.min.js_) after **jQuery**.  This was downloaded in Step 2.

-Add **jquery.selectBoxIt.min.js** (downloaded in Step 1) after your custom **jQueryUI** JavaScript file.

**Note:**  Order Matters.  Include these files in the same order as shown.  The jQueryUI Widget Factory and jQueryUI show/hide effects were included in the same file (jquery-ui-1.8.19).  You can find this file in the `SelectBoxIt` Github Repository under the `libs/jqueryUI` directory.  Also, this assumes that you have downloaded all of the JavaScript files to the same folder as your HTML page.  Modify the `src` attribute if you do not store the JavaScript files in the same folder as your HTML page.

{% codeblock lang:html %}
<body>
//Your other markup...

<script type="text/javascript" src="jquery-1.7.2.min.js"></script>
<script type="text/javascript" src="jquery-ui-1.8.19.custom.min.js"></script>
<script type="text/javascript" src="jquery.selectBoxIt.min.js"></script>
</body>
{% endcodeblock %}

**Call the plugin**

In your JavaScript code, call the selectBoxIt method on your HTML select box.

{% codeblock lang:js %}
//Executes your code when the DOM is ready.  Acts the same as $(document).ready().
$(function() {
    //Calls the selectBoxIt method on your HTML select box.
    var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
});
{% endcodeblock %}

##Options API

The Options API allows you to customize the dropdown list by setting custom options.  All options can be set when the plugin is called, or after the plugin is called, using the `setOption()` or `setOptions()` methods.

Here is an example of setting the options when the plugin is called:
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt({ showEffect: "fadeIn", showEffectSpeed: "medium", hideEffect: "fadeOut", hideEffectSpeed: "medium" }).data("selectBoxIt");
{% endcodeblock %}

Here is an example of setting the options after the plugin is called using the `setOptions()` method:
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
selectBox.setOptions({ showEffect: "fadeIn", showEffectSpeed: "medium", hideEffect: "fadeOut", hideEffectSpeed: "medium" });
{% endcodeblock %}

Here is an example of setting a single option after the plugin is called using the `setOption()` method:
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
selectBox.setOption("showEffect", "fadeIn");
{% endcodeblock %}

All of the custom options are described below:

**showEffect**: Accepts a String: `none`, `fadeIn`, `show`, `slideDown`, or any of the <a href="http://jqueryui.com/demos/show/" target="_blank"> jQueryUI show effects</a> (i.e. `bounce`).  Default is `none`

**showEffectOptions**: Accepts an object literal.  All of the available properties are based on the <a href="http://docs.jquery.com/UI/Effects/" target="_blank">jqueryUI effect options</a>(i.e. {direction: `down`}).  Default is `{}`

**showEffectSpeed**: Accepts a Number (milliseconds) or a String: `slow`, `medium`, or `fast`.  Default is `medium`

**hideEffect**: Accepts String: `none`, `fadeOut`, `hide`, `slideUp`, or any of the <a href="http://jqueryui.com/demos/hide/" target="_blank"> jQueryUI hide effects</a> (i.e. `bounce`). Default is `none`

**hideEffectOptions**: Accepts an object literal.  All of the available properties are based on the <a href="http://docs.jquery.com/UI/Effects/" target="_blank">jqueryUI effect options</a>(i.e. {direction: `up`}).  Default is `{}`

**hideEffectSpeed**: Accepts a Number (milliseconds) or a String: `slow`, `medium`, or `fast`.  Default is `medium`

**keyboardSearch**: Allows users to search for dropdown options.  Accepts a Boolean: true or false.  Default is `true`

**keyboardNavigation**: Allows keyboard navigation using the `up` and `down` keyboard arrow keys.  Accepts a Boolean: true or false.  Default is `true`

**showFirstOption**: Shows the first select box option within the dropdown options list. Accepts a Boolean: true or false.  Default is `true`

**defaultText**: Overrides the text, used by the dropdown list, to allow a user to specify custom text.  Accepts a String.  Default is `""`.

**defaultIcon**: Overrides the icon, used by the dropdown list selected option, to allow a user to specify a custom icon.
Accepts a String.  Default is `""`.

**downArrowIcon**: Overrides the default down arrow, used by the dropdown list, to allow a user to specify a custom image.
Accepts a String.  Default is `"ui-icon ui-icon-triangle-1-s"`, which are the jQueryUI class names for the down arrow icon.

##Events API

All custom/default events are **triggered on the original select box element** (not the new dropdown list).  The original select box `value` attribute is also synced with the new dropdown list, so if a user selects a new value from the dropdown list, the original select box value will also be updated.  This allows your existing code to continue working inside of forms.

You can catch **Default Events** by using the jQuery `bind()` or `on()` methods, or by using jQuery convenience methods such as `click`, `change`, etc.  You **must** use the jQuery `bind()` or `on()` methods to catch **Custom Events**.

Here is an example of catching a **Default Event** by using the jQuery `bind()` method:
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
$("select#test").bind({
    "focus": function() {
        //Do something here
    }
});
{% endcodeblock %}

Here is an example of catching a **Default Event** by using a jQuery convenience method:
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
$("select#test").focus(function() {
    //Do something here
});
{% endcodeblock %}

Here is an example of catching a **Custom Event** by using the jQuery `bind()` method:
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
$("select#test").bind({
    "open": function() {
        //Do something here
    }
});
{% endcodeblock %}

**Note:**  _SelectBoxIt_ also provides `$(":selectBox-selectBoxIt")`, a custom jQuery pseudo selector, that returns all select box elements that use the SelectBoxIt plugin (useful if you are using the plugin to enhance multiple select box elements and want to catch events on all select box elements).

###Default Events

`focus` - A focus event will be triggered when a user either clicks or tabs to the dropdown list.

`focusin` - A focusin event will be triggered when a user either clicks or tabs to the dropdown list.  Focus and focusin events are closely related, but the focusin event bubbles up the DOM tree and the focus event does not bubble.  If you are using a library such as **Backbone.js**, which uses event delegation, use the focusin event to determine when the select box element gains focus.

`click` - A click event will be triggered when a user clicks on the dropdown list.

`blur` - A blur event will be triggered when the dropdown list loses focus.

`focusout` - A focusout event will be triggered when the dropdown list loses focus.  Blur and focusout events are closely related, but the focusout event bubbles up the DOM tree and the blur event does not bubble.  If you are using a library such as **Backbone.js**, which uses event delegation, use the focusout event to determine when the select box element loses focus.

`change` - A change event will be triggered when a user selects a new dropdown list option.

`mouseenter` - A mouseenter event will be triggered when a user's mouse enters an element.  jQuery uses both mouseenter and mouseleave to simulate a hover event.

`mouseleave` - A mouseleave event will be triggered when a user's mouse leaves an element.  jQuery uses both mouseenter and mouseleave to simulate a hover event.

###Custom Events

`open` - An open event will be triggered when a user opens the dropdown list.

`close` - A close event will be triggered when a user closes the dropdown list.

`moveDown` - A moveDown event will be triggered when a user presses the down arrow key to select a dropdown list option directly beneath the currently selected option.

`moveUp` - A moveUp event will be triggered when a user presses the up arrow key to select a dropdown list option directly above the currently selected option.

`search` - A search event will be triggered when a user does a text search that matches a dropdown list option.  Keep in mind that this event will be fired only when a search match is found.

`enter` - An enter event will be triggered when a user presses the enter key while the dropdown list is focused.

`tabFocus` - A tabFocus event will be triggered when a user presses the tab key to focus the dropdown list.

`tabBlur` - A tabBlur event will be triggered when a user presses the tab key to blur the dropdown list.

`backspace` - A backspace event will be triggered when a user presses the backspace key while the dropdown list is focused.

`disable` - A disable event will be triggered if a dropdown list becomes disabled.

`enable` - An enable event will be triggered if a dropdown list becomes enabled, or in other words, no longer disabled.

`destroy` - A destroy event will be triggered if a dropdown list is destroyed.

`create` - A create event will be triggered if a dropdown list is created.

##Method API

The Method API allows you to programmatically interact with the dropdown list after it is created.  All methods can be called individually or chained.  Here is an example of chaining: 
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
selectBox.open().close().moveDown().disable();
{% endcodeblock %}

If you want to provide a delay (in milleseconds) before your methods are called, use the **wait()** method.  Here is an example of the **wait()** method.

{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
//Opens the dropdown list for two seconds before closing the dropdown list
selectBox.open().wait(2000, function() { this.close() });
{% endcodeblock %}

**Note:** You can pass a callback function to all of the methods.  Inside of the callback function, the `this` keyword refers to the plugin object, which allows you to call another plugin method like so:
{% codeblock lang:js %}
var selectBox = $("select#test").selectBoxIt().data("selectBoxIt");
selectBox.open(function() {
    this.moveDown();
}); 
{% endcodeblock %}

**open()**: Opens the dropdown options list.

**close()**: Closes the dropdown options list.

**moveDown()**: Selects the dropdown option directly beneath the currently selected option.

**moveUp()**: Selects the dropdown option directly above the currently selected option.

**search(String searchTerm)**: Selects the dropdown option that most closely matches the text passed into the method.  If a pattern match is found, the dropdown text value changes.  If a pattern match is not found, then the dropdown text value does not change.

**setOption(String key, String value)**: Sets a single plugin option.

**setOptions(Object newOptions)**: Sets or adds new plugin option settings.

**disable()**: Disables the dropdown/select box.

**enable()**: Enables the dropdown/select box.

**destroy()**: Removes the dropdown from the DOM and makes the original select box element visible.

**wait(Number time, Function callback)**: Delays execution of the callback function by the amount of time (milleseconds) specified by the `time` parameter.

##Getting Options

You can get the current options used by `SelectBoxIt` two different ways:

**Example 1:** Retrieving a single plugin option

{% codeblock lang:js %}
var selectBoxIt = $("select#test").selectBoxIt().data("selectBoxIt");
console.log(selectBoxIt.option("showFirstOption"));
{% endcodeblock %}

**Example 2:** Retrieving all of the plugin options
{% codeblock lang:js %}
var selectBoxIt = $("select#test").selectBoxIt().data("selectBoxIt");
console.log(selectBoxIt.options);
{% endcodeblock %}

##Optgroup Support
**SelectBoxIt** supports optgroups.  You have full control to style the optgroups by using the `optgroupHeader` and `optgroupOption` CSS classes.  

Here is an example of a dropdown list that uses optgroups:

<select id="optgroups" name="optgroups">
<option value="SelectBoxIt is:">SelectBoxIt is:</option>
<optgroup label="Optgroup 1">
  <option value="a jQuery Plugin">a jQuery Plugin</option>
</optgroup>
<optgroup label="Optgroup 2">
  <option value="a Select Box Replacement">a Select Box Replacement</option>
</optgroup>
<optgroup label="Optgroup 3">
  <option value="a Stateful UI Dropdown Widget">a Stateful UI Dropdown Widget
  </option>
</optgroup>
</select>  
<br /><br />

The HTML code from the above example looks like this:
{% codeblock lang:html %}
<select id="optgroups" name="optgroups">
<option value="SelectBoxIt is:">SelectBoxIt is:</option>
<optgroup label="Optgroup 1">
  <option value="a jQuery Plugin">a jQuery Plugin</option>
</optgroup>
<optgroup label="Optgroup 2">
  <option value="a Select Box Replacement">a Select Box Replacement</option>
</optgroup>
<optgroup label="Optgroup 3">
  <option value="a Stateful UI Dropdown Widget">a Stateful UI Dropdown Widget
  </option>
</optgroup>
</select>  
{% endcodeblock %}


##Using Icons

**SelectBoxIt** supports jQueryUI/custom image icons.  You can specify the class names that you want to use to show the appropriate icon(s) (set the `background` property inside of your CSS).  There are two ways to specify CSS class names (adding HTML5 data attributes or SelectBoxIt options).  View the **Options API** and **HTML5 Data Attributes** Sections for more documentation.

Here is an example of a dropdown list that uses jQueryUI icons:
<br />
<select id="icons" name="test">
<option value="SelectBoxIt is:" data-icon="ui-icon ui-icon-search">SelectBoxIt is:</option>
<option value="a jQuery Plugin" data-icon="ui-icon ui-icon-check">a jQuery Plugin</option>
<option value="a Select Box Replacement" data-icon="ui-icon ui-icon-check">a Select Box Replacement</option>
<option value="a Stateful UI Dropdown Widget" data-icon="ui-icon ui-icon-check">a Stateful UI Dropdown Widget</option>
</select>
<br />

The HTML code from the above example looks like this:
{% codeblock lang:html %}
<select id="icons" name="test">
<option value="SelectBoxIt is:" data-icon="ui-icon ui-icon-search">SelectBoxIt is:</option>
<option value="a jQuery Plugin" data-icon="ui-icon ui-icon-check">a jQuery Plugin</option>
<option value="a Select Box Replacement" data-icon="ui-icon ui-icon-check">a Select Box Replacement</option>
<option value="a Stateful UI Dropdown Widget" data-icon="ui-icon ui-icon-check">a Stateful UI Dropdown Widget</option>
</select>
{% endcodeblock %}

By specifying a `data-icon` HTML5 data attribute for each select box option, I am able to tell SelectBoxIt which CSS classes to use to show a custom image for each of my individual options.  The `ui-icon` and `ui-icon-search` CSS classes are both given by the jQueryUI CSS framework (these image icons are included in the **ThemeRoller** download folder).

**Note:** If you had just specified a `data-icon` attribute on the select box, and not on any of the individual options, the dropdown list will use the `data-icon` attribute as a default icon image, and will not change it's icon when options are selected.

If you want to use custom icons, then you need to create custom CSS properties in **jquery.selectBoxIt.css** similar to this:
{% codeblock lang:css %}
.someImage {
  /* Width and height of your icon */
  width: 16px; 
  height: 16px;
  /* File path to your icon */
  background: url(../someimage.png) no-repeat top left;
}
{% endcodeblock %}


There are a few other goodies provided, including a way for you to change the default icon image using the `defaultIcon` option, and also a way for you to customize the dropdown down arrow to be a different icon.

Here is an example of a dropdown list that uses jQueryUI icons like the above example, but also updates the dropdown down arrow to be an up arrow when the dropdown list is opened.

<select id="upArrow" name="test">
<option value="SelectBoxIt is:" data-icon="ui-icon ui-icon-search">SelectBoxIt is:</option>
<option value="a jQuery Plugin" data-icon="ui-icon ui-icon-check">a jQuery Plugin</option>
<option value="a Select Box Replacement" data-icon="ui-icon ui-icon-check">a Select Box Replacement</option>
<option value="a Stateful UI Dropdown Widget" data-icon="ui-icon ui-icon-check">a Stateful UI Dropdown Widget</option>
</select>
<br /><br />

Here is the jQuery snippet from the above example:
{% codeblock lang:js %}
var upArrowSelectBox = $("select#upArrow").selectBoxIt().data("selectBoxIt");

$("select#upArrow").bind({
    "open": function() {
        upArrowSelectBox.setOption("downArrowIcon", "ui-icon ui-icon-triangle-1-n");
    },
    "close": function() {
        upArrowSelectBox.setOption("downArrowIcon", "ui-icon ui-icon-triangle-1-s");
    }
});
}
{% endcodeblock %}

If you are interested in other jQueryUI icon images, check out the <a href="http://jquery-ui.googlecode.com/svn/tags/1.6rc5/tests/static/icons.html" target="_blank">jQueryUI Icons Test Page</a>


##HTML5 Data Attributes

**data-icon** - Specifies the custom or jQueryUI CSS classes you want to use to show icon images for the dropdown list and/or dropdown list individual options

**data-downarrow** - Specifies the custom or jQueryUI CSS classes you want to use to replace the default down arrow icon image

**data-text** - Specifies the custom text that you want to use for the dropdown list


##CSS Guide

**SelectBoxIt** dynamically creates new HTML elements to replace your original select box.  Each new HTML element has a dynamic `id` attribute generated for it.  Each CSS selector in the default CSS file expects a select box with an `id` of **test**, but if you are using a different `id` attribute for your select box (which you probably are), just replace the word **test** with the `id` attribute of your select box.

**Note:** It is highly recommended to use a jQueryUI theme to help style your **SelectBoxIt** dropdown list, but it is not required.  Also, keep in mind that the CSS file is heavily commented to help you easily understand where to place your custom CSS attributes.

**Element Overview**

`#testSelectBoxItContainer` - This selector is a `div` element that holds all of the HTML elements generated by **SelectBoxIt**.  If you want to horizontally or vertically position your dropdown list on the page (i.e. float: left;), do that using this element.

`#testSelectBoxIt` - This selector is a `div` element that acts as the new dropdown list.  It contains the dropdown list selected text, and also the down arrow image (provided by jQueryUI).  If you want to increase the height of the dropdown list, use this element.

`#testSelectBoxItDefaultIcon` - If you are using an image with your dropdown list, this selector allows you to adjust the positioning of the image.

`#testSelectBoxItText` - This selector is a `span` element that holds the dropdown list selected text.  If you want to change the font or text positioning of the dropdown list selected text, use this element.

`#testSelectBoxItArrowContainer` - This selector is a `span` element that holds the dropdown list down arrow image.  If you want to alter the positioning of the down arrow or enclose the down arrow inside of a box (by adding a `border-left` CSS attribute), use this element.

`#testSelectBoxItOptions .optgroupHeader` - This selector is an `li` element that styles all of the dropdown lists optgroup header text (if you are using optgroups).

`#testSelectBoxItOptions .optgroupOption` - This selector is an `li` element that styles all of the dropdown lists optgroup options text (if you are using optgroups).

`#testSelectBoxItOptions` - This selector is a `ul` element that holds all of the dropdown list options.  If you want to limit how many dropdown list options are visible when the dropdown list is opened, change the `max-height` attribute of this element.

`#testSelectBoxItOptions li` - This selector is all of the `li` elements that each hold one of the dropdown list options.  If you want to change anything (i.e. background-color, font) about the dropdown list options, use this element.

`#testSelectBoxItOptions li span` - If you are using image icons with your individual dropdown list options, this selector allows you to adjust the positioning of each icon.


##Demos

**Note:** Edit any of these demos by clicking the <span style="display:inline-block;*display:inline;zoom:1;" class="ui-icon ui-icon-plusthick"></span> button.  Re-run each demo by clicking the <span style="display:inline-block;*display:inline;zoom:1;" class="ui-icon ui-icon-play"></span>  button.
 

**Basic**- Uses the default options

{% jsfiddle mxvgx result,js,html,css %}

<br /><br />

**Basic with Disabled Options** - Uses the default options with multiple disabled dropdown options

{% jsfiddle qPfz3 result,js,html,css %}

<br /><br />

**jQuery Show/Hide Effects**- Uses the jQuery `slide` effect.  All jQuery effects can be used (i.e. fade, show, etc)

{% jsfiddle w4kph result,js,html,css %}
<br /><br />

**jQueryUI Show/Hide Effects with Special Show Effect Option**- Pick a jQueryUI effect from the select box and then click on the **Run Effect** button to view the effect.

{% jsfiddle rXuRR result,js,html,css %}

<br /><br />

**Using the Method API to open, close, move up, move down, and search**- Click each button to see what happens

{% jsfiddle ghHEq result,js,html,css %}

<br /><br />

**Using the Method API to disable, enable, and destroy the dropdown list**- Click each button to see what happens

{% jsfiddle AqGJJ result,js,html,css %}

<br /><br />

**Using the Events and Options API's**- Hides the first dropdown list option (by changing the `showFirstOption` option to `false`) when a change event is triggered on the original select box.

{% jsfiddle c3rFP result,js,html,css %}

<br /><br />

**Using the Events API with custom events**- Tracks a user's interaction with the dropdown list by printing out some of the custom events that are triggered by the plugin

{% jsfiddle gSt2G result,js,html,css %}

<br /><br />

##Contributing to SelectBoxIt

Take care to maintain the existing coding style. Add Jasmine unit tests for any new or changed functionality. Lint and test your code using [grunt](https://github.com/cowboy/grunt).

If you plan to contribute to `SelectBoxIt` in the future, keep in mind that you should make sure your code passes the Grunt checks.  If you are on Windows (like me) remember you need to run the grunt command using `grunt.cmd`.  Also, if you have trouble getting the Jasmine Unit Tests to work with PhantomJS 1.5 (the current release), install PhantomJS 1.3.

After you have verified your code, send a pull request to the `SelectBoxIt` dev branch.  After you send a pull request, you will hear back from me shortly after I review your code.

You'll find source code in the "src" subdirectory!

##Extending SelectBoxIt

If you find that you need a feature that SelectBoxIt does not currently support, either let me know via the <a href="https://github.com/gfranko/jQuery.selectBoxIt.js/issues" target="_blank">Github issue tracker</a>, or <a href="https://github.com/gfranko/jQuery.selectBoxIt.js" target="_blank">fork</a> the project and and easily extend SelectBoxIt to create your own widget!

**Note:** Remember that you need to include **jQuery**, the **jQueryUI Widget Factory**, and **jquery.selectBoxIt.min.js** before you include your new plugin file, since your plugin will depend on SelectBoxIt and all of its dependencies.

**Example: Extending SelectBoxIt**

{% codeblock lang:js %}
// Plugin setup
(function ($) {

    $.widget('a.newPlugin', $.selectBox.selectBoxIt, {

        // Changing SelectBoxIt's default showEffect from 'none' to 'slide'
        options: {
            showEffect: "slide", 
        },

        //Overwriting the SelectBoxIt open method
        open: function() {

            //Calling the default SelectBoxIt open method
            $.selectBox.selectBoxIt.prototype.open.call(this);

            // Adding new logic
            console.log("Just opened my new plugin!");

        }

    });

    //Then call your new plugin like this
    var selectBox = $("select#test").newPlugin().data("newPlugin");

}(jQuery));
{% endcodeblock %}


##Change Log

`0.9.0` - May 21, 2012 *Approaching a stable 1.0 release*

	- IE7 and IE8 bug fix: A special thanks to lhwparis

`0.8.0` - May 15, 2012 *Approaching a stable 1.0 release*

	- Bug fixes for the `disabled` use cases

`0.7.0` - May 10, 2012

	- Added optgroup support to allow dropdown list options to be put in subgroups.

	- Bug fixes to the `change` and `focus` Event API handlers

`0.6.0` - May 3, 2012

	- Added jQueryUI and custom icon support to allow icons to be used for the dropdown list and also alongside individual dropdown options.  You can specify the class names that you want to use to show the appropriate icon (set the background-image property inside of your CSS).  There are two new ways to do this (HTML5 data attributes or plugin options)

		* Added support for three new HTML5 data attributes to be used with the original select box element.  Use cases for each are described below.
			* data-icon - Specifies the custom or jQueryUI CSS classes you want to use to show icon images for the dropdown list and/or dropdown list individual options
			* data-downarrow - Specifies the custom or jQueryUI CSS classes you want to use to replace the default down arrow icon image
			* data-text - Specifies the custom text that you want to use for the dropdown list

		* Added support for two new options.  Use cases for each are described below.
			* defaultIcon - An alternative to the `data-icon` HTML5 data attribute
			* downArrowIcon - An alternative to the `data-downarrow` HTML5 data attribute

`0.5.0` - April 29, 2012 **MAJOR REWRITE**

	- SelectBoxIt has been rewritten using the jQueryUI Widget Factory!  This means that SelectBoxIt now depends on both jQuery and the jQueryUI Widget Factory.  This also means that there are a few API changes that are not backwards compatible...
		* getOption(), getOptions(), and create() were all removed from the Method API
		* To use the custom pseudo selector, you must now use $(":selectBox-selectBoxIt")

	- SelectBoxIt now uses [grunt](https://github.com/cowboy/grunt) to run jsHint for code quality checking, Jasmine for unit testing, and UglifyJS for minification.

	- Removed AMD Support

`0.4.0` - April 28, 2012

	- `AMD Support`.  If `AMD` support is found, SelectBoxIt is wrapped in a define `module`.
	  
	  [UMD Patterns](https://github.com/umdjs/umd/blob/master/jqueryPlugin.js)

	- `Bug fixes for supporting the `disabled` HTML property for individual select box options


`0.3.0` - April 25, 2012

	- A new option, defaultText, was added to allow users to specify the default text of the dropdown list that is not linked to a specific select box option

	- The disabled HTML property is now supported for individual select box options

	- When a user presses the esc keyboard key, the dropdown options list will now close (become hidden)


`0.2.0` - April 24, 2012

	- This release requires you to use jQuery 1.6.1+.

	- You are no longer required to have select box option values be the same as the select box option text.

	- IE bug fix to prevent default dropdown text from being selectable


`0.1.0` - April 14, 2012

	- Initial SelectBoxIt release.  Added annotated source code, unit tests, and documentation

##Frequently Asked Questions

Questions will be added here from the <a href="https://github.com/gfranko/jQuery.selectBoxIt.js/issues" target="_blank">Github Issues Page</a> and from the comments below.  Also, more demos/documentation will be provided on request.