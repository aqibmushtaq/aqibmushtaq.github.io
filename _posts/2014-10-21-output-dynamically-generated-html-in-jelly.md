---
layout: post
title: Output dynamically generated HTML in Jelly
date: '2014-10-21T21:08:36+01:00'
tags: [ServiceNow, Jelly, HTML]
tumblr_url: http://aqibnow.com/post/100606790422/output-dynamically-generated-html-in-jelly
---
It can be a pain trying to output some HTML stored within a variable using Jelly. When printing an expression with Jelly, it is automatically escaped.

<!--break-->

For example

{% highlight xml %}
<g:evaluate var="jvar_some_content">
  var content = "<p>foo</p><p>bar</p>";
  content;
</g:evaluate>
<div>${jvar_some_content}</div>
{% endhighlight %}

Will output:

{% highlight html %}
<p>foo</p><p>bar</p>
{% endhighlight %}

To prevent Jelly from escaping the output, using no_escape as follows:

{% highlight xml %}
<g:evaluate var="jvar_some_content">
  var content = "<p>foo</p><p>bar</p>";
  content;
</g:evaluate>
<g2:no_escape>${HTML:jvar_some_content}</g2:no_escape>
{% endhighlight %}

Will output:

{% highlight xml %}
foo
bar
{% endhighlight %}
