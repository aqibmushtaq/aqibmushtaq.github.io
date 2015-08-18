---
layout: post
title: Jelly “and” condition
date: '2015-08-17T10:34:32+01:00'
tags: []
tumblr_url: http://aqibnow.com/post/126901148702/jelly-and-condition
---
We’ve all been through the pain of finding how to properly escape and conditions in UI Macros and UI Pages in ServiceNow.

<!--break-->

The convention is to escape ampersands like this:

{% highlight xml %}
<j:if test="${sysparm_some_var == ''foo'' &amp;&amp; sysparm_another_var == ''bar''}">
<h1>Foo bar</h1>
</j:if>
{% endhighlight %}

However you can just use the and keyword to achieve the same thing:

{% highlight xml %}
<j:if test="${sysparm_some_var == ''foo'' and sysparm_another_var == ''bar''}">
<h1>Foo bar</h1>
</j:if>
{% endhighlight %}
