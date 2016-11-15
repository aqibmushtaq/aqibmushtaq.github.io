---
layout: post
title: "Writing Reusable ServiceNow Client Scripts"
date: 2016-11-15T19:20:00+00:00
tags:
- ServiceNow
- Client Script
- JavaScript
- UI Script
---

Writing Client Scripts in ServiceNow can a repititive task as there is not easy way to reuse code. In this post I'll tell you how to create a reusable client side script.

<!--break-->

Lets is this as an example - writing a Catalog Client Script across multiple Catalog Items in order to hyphenate and lowercase a string field when it is changed. Normally this would involve writing the same code across mutliple Catalog Client Scipts for each Catalog Items. This approach will lead to technical debt. When you need to alter the script, you'll end up changing multiple scripts.

The reusable approach is to create a UI Script and import it in any ServiceNow client script, here are a few examples:

  - Client Scripts
  - UI Policies
  - Client UI Actions
  - Catalog Client Scripts
  - Catalog UI Policies

First you'll need to create a UI Script:

{% highlight javascript %}
var CatalogUtils = Class.create();
CatalogUtils.prototype = {
  initialize: function() {
  },

  hyphenateVariable: function(variableName) {
    var newValue = this.hyphenateString(g_form.getValue(variableName));
    g_form.setValue(variableName, newValue);
  },

  hyphenateString: function(string) {
    return string.toLowerCase().replace(/ /g, '-');
  },
}
{% endhighlight%}

We'll now need to create a Catalog Client Script which is triggered when the string field value changes. In the script field you'll need to write this:

{% highlight javascript %}
ScriptLoader.getScripts('CatalogUtils.jsdbx', function() {
  new CatalogUtils().hyphenateVariable('[VARIABLE NAME]');
});
{% endhighlight%}

Things to note:

- ScriptLoader.getScripts accepts two parameters
  - Either a string of the script you are loading or an array of script names as strings
    1. ScriptLoader.getScripts('ScriptName.jsdbx', function() {});
    2. ScriptLoader.getScripts(['ScriptName.jsdbx', 'ScriptName2.jsdbx'], function() {});
- You won't load the same scripts again by calling getScripts with the same script names.
