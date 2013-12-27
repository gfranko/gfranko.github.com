---
layout: post
title: "Documenting Your Open Source Projects"
date: 2012-09-18 10:14
comments: true
categories: [JavaScript]
description: "DownloadBuilder.js helps you create custom project builds for your open source projects"
keywords: "Open Source, Documentation, Documenting Open Source, OS, Tocify, Document-Bootstrap" 
---

Popular Open Source (OS) projects are self promoted by their easy to read, organized, and exhaustive documentation.  Great examples include Backbone.js, Can.js, jQuery++, Underscore.js, jQuery, HTML5 Boilerplate, Require.js, Twitter Bootstrap, and many more.

<!-- more -->

{% img center /images/opensource.png %}

These projects realize that users expect their documentation to follow a common format that includes a table of contents that houses, at the very least, a _Getting Started_ guide, a _Demos/Examples_ section, and a _forum_ section to ask questions.  Additional sections (ie. _annotated source code_, _Contributor Guide_) targeting potential core project contributors may also be included.

All OS developers should take note of these documentation practices and follow them when promoting their own projects.  When patrolling Github to find new and interesting projects, I often find myself dismissing a project right away if there is a lack of documentation.

>The project could have the most readable, brilliant, and performant code known to man, yet I (and many other developers) would never know

###But Documentation Takes Too Long to Write!
It's true, documenting your project may take longer than writing the project code itself.  But there are open source tools that can help expediate the process...

<!-- more -->

####[Tocify.js](http://gregfranko.com/jquery.tocify.js/) - a jQuery plugin

Tocify is "a jQuery plugin that dynamically generates a table of contents (TOC). Tocify can be optionally styled with Twitter Bootstrap or jQueryUI Themeroller, and optionally animated with jQuery show/hide effects".

Tocify also optionally provides support for smooth scrolling, scroll highlighting, scroll page extending, and the HTML5 pushstate API via History.js.

Tocify works by looking at all of the header elements on an HTML page and constructing a TOC based on the page structure.  Tocify was inspired by the Can.js and jQuery++ documentation created by [Bitovi](http://bitovi.com/), so it allows you to have complex nested TOC structures along with slick show/hide effects.

####[Document-Bootstrap](http://gregfranko.com/Document-Bootstrap/) - a Boilerplate Project

Document-Bootstrap is "a boilerplate project that incorporates JavaScript libraries, jQuery plugins, and Twitter Bootstrap to save developers time when creating project pages and providing users with custom project builds".

Document-Bootstrap serves as a great starting point for creating a OS project page.  Document-Bootstrap allows you to stop worrying about the design of your project page and instead focus on the content.

**Note:** Document-Bootstrap includes Tocify

####[Docco](http://jashkenas.github.com/docco/) - an Annotated Source Code Generator

Docco "produces HTML that displays your comments alongside your code. Comments are passed through Markdown, and code is passed through Pygments syntax highlighting."

As long as your codebase adheres to [Markdown](http://daringfireball.net/projects/markdown/) standards, Docco will automatically create an annotated source code HTML page for your project.  Annotated source code is much cleaner and easier to read than regular source code, in my opinion.

**Note:** Docco can be used to process CoffeeScript, JavaScript, Ruby, Python, or TeX files

####Summary
I find that as a developer I read more documentation than I write code.  Please help my eyes by creating awesome documentation.  Happy documenting!




