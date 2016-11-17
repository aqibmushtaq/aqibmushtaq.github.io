---
layout: post
title:  "Strip out HTML tags"
date: 2016-11-17T12:15:00+00:00
tags:
- ServiceNow
- Strip HTML tags
- Server Script
---

In cases where you're getting information from external sources, you may need to strip out HTML tags.

<!--break-->

The code below does the following:

 1. Removes all HTML tags over multiple lines
 2. Splits the string into an array of strings, each string represents a line
 3. All ``&nbsp;`` codes are replaced with their text alternatives
 4. Empty lines are removed
 5. Turn the array back into a string and return the result

{% highlight javascript %}
function stripHtml(html) {
    return html.replace(/<(?:[^>=]|='[^']*'|="[^"]*"|=[^'"][^\s>]*)*>/g, '')
        .split(/\n/)
        .map(function(line) {
    	       return line.replace(/(&nbsp;)/g, ' ').trim();
        }).filter(function(line) {
    	       return line != '' && line != '&nbsp;';
        })
        .join('\n');
}

{% endhighlight %}

Regular expression is taken from this [StackOverflow post](http://stackoverflow.com/a/17668453/568050){:target="_blank"}
