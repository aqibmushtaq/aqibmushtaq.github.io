---
layout: post
title:  "Force a Record into an Update Set"
date: 2018-04-18T15:00:00+00:00
tags:
- ServiceNow
- Update Sets
---

Whilst developing in ServiceNow you may come across things you'd like in your update set but they aren't tracked.

You can achieve this by adding a global UI Action which will allow admins to force updates into their current Update Set.

<!--break-->

Create a UI Action with the following values

 1. **Name:** Force to Update Set
 2. **Table:** Global [global]
 3. **Active:** true
 4. **Show update:** true
 5. **Form link:** true
 6. **Condition:** gs.hasRole('admin')
 7. **Script:**

{% highlight javascript %}
var um = new GlideUpdateManager2();
um.saveRecord(gr)
{% endhighlight %}
