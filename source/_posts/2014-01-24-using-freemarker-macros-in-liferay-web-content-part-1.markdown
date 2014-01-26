---
layout: post
title: "Using FreeMarker Macros in Liferay Web Content - Part 1"
date: 2014-01-24 14:51:58 -0500
comments: true
categories: [Liferay, FreeMarker, Hooks, WCM]
---
Liferay supports using FreeMarker as a web content template language. There are many things I really like about FreeMarker and use it whenever possible. Macros are one of those features, as you can create libraries of routines you can use over and over. But it's not apparent where to place those libraries to get Liferay to recognize them and use them.

<!--more-->

[1]: https://github.com/learn-liferay/blog-files "blog files"
[2]: https://github.com/learn-liferay/blog-files/tree/master/liferay-plugins-sdk-6.1.1/hooks/freemarker-macros-hook "FreeMarker Macros Hook"
[3]: https://github.com/learn-liferay/blog-files/blob/master/liferay-plugins-sdk-6.1.1/hooks/freemarker-macros-hook/docroot/custom_files/WEB-INF/classes/freemarker/macros/mylib.ftl "mylib.ftl"
There is a setting in portal.properties where you can specify macro libraries
[4]: /blog/2014/01/25/using-freemarker-macros-in-liferay-web-content-part-2/ "Part 2"

```
    #
    # Input a list of comma delimited macros that will be loaded. These files
    # must exist in the class path.
    #
    freemarker.engine.macro.library=FTL_liferay.ftl as liferay
```

but, unfortunately if you do, they are not recognized by WCM templates.

So it looks like we need to find a way to put them somewhere that the built in template loaders will find them. The comment above is a provides the hint of where they need to be.

```These files must exist in the class path.```

So even though we can't specify them in that property, we just need to get them into the class path somewhere.

**Hooks to the rescue!**

We can create a hook to deploy our macro library files somewhere in the class path. And while most people talk about using a hook for custom jsps, any file can be deployed by a hook.


{% codeblock lang:xml liferay-hook.xml %}
<?xml version="1.0"?>
<!DOCTYPE hook PUBLIC "-//Liferay//DTD Hook 6.1.0//EN" "http://www.liferay.com/dtd/liferay-hook_6_1_0.dtd">

<hook>
	<custom-jsp-dir>/custom_files</custom-jsp-dir>
</hook>
{% endcodeblock %}

Like a typical jsp hook, I just set the directory name for where the files will be added/replaced. In my case I used **custom_files** instead of custom_jsps because I'm not using jsps.

Where you put them in the class path is up to you, I put them in ``/WEB-INF/classes/freemarker/macros``

{% img /images/freemarker_macros/custom-files-path.jpg 300 %}

I've created a hook in the [blog files][1] project called [freemarker-macros-hook][2] for you to use as a starting point.

Build and deploy your hook and verify that the library got deployed where you expected it to. 

In the next part, we'll actually use the mylib.ftl and see how this all works.

-----

*(make sure you use the **[mylib.ftl][3]** in the project if you plan to move on to [part 2][4])*









