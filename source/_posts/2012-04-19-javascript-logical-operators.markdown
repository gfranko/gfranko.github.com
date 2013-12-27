---
layout: post
title: "JavaScript Logical Operators"
date: 2012-04-19 23:03
comments: true
categories: [JavaScript]
description: "Understanding the JavaScript OR and AND (Logical) Operators"
keywords: "Greg Franko, JavaScript Operators, JavaScript logical operators, JavaScript or operator, JavaScript and operator, "
---

Have you ever written code like this?

{% codeblock lang:js %}
var something;

if(somethingElse === true) {
    something = true;
}
else if(somethingElse === false) {
    something = false;
}
{% endcodeblock %}

<!-- more -->

If you have, then you need to be using **JavaScript logical operators**.  The previous code can be rewritten like this:

{% codeblock lang:js %}
var something = somethingElse || false;
{% endcodeblock %}
<br />

##Logical OR Operator
The logical OR operator **returns the first true value** it finds (looking from left to right).  If nothing is true in the statement, then it defaults to the last value (false in the previous code example).  This makes it **perfect for setting default values**.

It is also important to understand that JavaScript evaluates other variable types to boolean (true or false).  Below are examples of other JavaScript variable types that are evaluated to true or false:

`""` - An empty string in JavaScript returns false.  All other string values return true.

`undefined` - Returns false.

`null` - Returns false

`0` - Returns false.  All other numbers return true.

Here is a more advanced (and more common) use case for the logical OR operator.  You want to check if a certain string value is empty, and if it is, you want to set a variable to a default value of "test"

{% codeblock lang:js %}
var something = (typeof somethingElse === "string" && somethingElse) || "test";
{% endcodeblock %}

Here is the breakdown of how the above code works:

**1.**  If the somethingElse value is both a string and is true (is not empty), then set the something variable equal to the somethingElse value.  It is important to check that the somethingElse value `typeof` evaluates to `string`, because we want to make sure we are dealing with a string (and not an integer such as 0 which will evaluate to true).

**2.**  If the somethingElse value is not a string and is false (is empty), then set the something variable equal to "test".
<br />

##Logical AND Operator
The **JavaScript logical AND operator** can also be used to set default values.  Instead of returning the first true value, the logical AND operator **returns the first false value** it finds (looking from left to right).  If there is no false value, then the last value is returned.  Let's look at the first code example again, but instead with the logical AND operator.

{% codeblock lang:js %}
var something = somethingElse && "something";
{% endcodeblock %}

If the somethingElse value evaluates to false, then the something variable gets set to the somethingElse value.  If the somethingElse variable evaluates to true, then the something variable gets set to "something".  The logical AND operator uses the exact opposite logic of the logical OR operator.

JavaScript logical operators are an often misunderstood but powerful feature of the language.  Start using them today!

