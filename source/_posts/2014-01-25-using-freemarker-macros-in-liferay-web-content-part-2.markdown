---
layout: post
title: "Using FreeMarker Macros in Liferay Web Content - Part 2"
date: 2014-01-25 20:05:12 -0500
comments: true
categories: [Liferay, FreeMarker, WCM]
---

[1]: http://www.liferay.com/documentation/liferay-portal/6.1/user-guide/-/ai/lp-6-1-ugen03-advanced-content-creation-6 "6.1 WCM Docs"
[2]: http://freemarker.org/docs/ref_directive_macro.html "FreeMarker Manual on macros"
[3]: https://github.com/learn-liferay/blog-files/blob/master/liferay-plugins-sdk-6.1.1/hooks/freemarker-macros-hook/docroot/custom_files/WEB-INF/classes/freemarker/macros/mylib.ftl "mylib.ftl"
[4]: /blog/2014/01/24/using-freemarker-macros-in-liferay-web-content-part-1/ "Part 1"

Now that we have our FreeMarker macro library deployed ([*from part 1*][4]), let's set up the web content structure, template and article to test our macro library. If you are unfamiliar with web content structures and templates you will want to read the Liferay User Guide on [advanced content with structures and templates][1] first.

<!--more-->

The end result will create a table with alternating color rows by calling a macro which does all the work and can be reused. It will look like this.

{% img /images/freemarker_macros/web-content-display.jpg %}

To start, create a structure. The structure will be a simple repeatable field.

{% img /images/freemarker_macros/structure.jpg %}


For the template, we'll include the styles in the template (just for the purpose of making this example simple to do). 

Make sure you uncheck the **Cachable** checkbox and select FTL as the **Language Type**. Make sure you pick the structure, I sometimes forget to do that, when you create your template.

{% img /images/freemarker_macros/template-options.jpg %}

Then you can paste the template code into the editor box when you click on **Launch Editor**.

{% include_code The WCM Template lang:css freemarker_macros/template.txt %}


So looking at the template, we see the styles definitions and this...


**<@mylib.createTable items=state.siblings />**

This is the macro call. It's calling the createTable from the mylib file. You can have multiple macro files and the name of the file (without file extension) is used to namespace the macro. In our case the file name was mylib.ftl, so the namespace is myftl.

It's calling the createTable macro and passing a parameter **items** with the value of **states.siblings**. This passes the repeating field **states** values to the macro.

You can read more about FreeMarker macros in the [FreeMarker manual - macros section][2].

Looking at the template used below, there are actually two macros. One builds the table and calls the setRowColorClass macro in the same file. As it's in the same file we don't need to use the **mylib** namespace.

{% include_code The macro file lang:java freemarker_macros/mylib.ftl %}

Now create the content article and fill in some of the states,

{% img /images/freemarker_macros/web-content-filled-in.jpg 450 %}

At this point you should have the structure, template and article done. If you put the article on a page, you should see...

{% img /images/freemarker_macros/web-content-display.jpg %}

Now you can create and use your own FreeMarker macros and be more productive.

