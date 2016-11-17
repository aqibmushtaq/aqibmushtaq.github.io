---
layout: post
title: String interpolation
date: '2014-07-26T14:49:20+01:00'
tags: [ServiceNow, Interpolation]
tumblr_url: http://aqibnow.com/post/92919417162/string-interpolation
---
String interpolation can be achieved using gs.getMessage() in ServiceNow; it’s very similar to Java’s MessageFormat class.

<!--break-->

{% highlight javascript %}
gs.getMessage("Hello, my name is {0} and I live in {1}.", ["Aqib", "London"]);
{% endhighlight %}

The first argument is a pattern taken as a string. The second argument are the values to be inserted into the pattern.
