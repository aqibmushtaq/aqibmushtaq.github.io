---
layout: post
title: 'GlideAjax Tutorial 2: Infinite Scrolling of Recent Incidents'
date: '2014-10-22T22:44:04+01:00'
tags: [ServiceNow, GlideAjax, Ajax, Asynchronous, JavaScript, GlideRecord]
tumblr_url: http://aqibnow.com/post/100696263337/glideajax-tutorial-2-infinite-scrolling-of-recent
---

Using the same concepts of Ajax as last time, we’ll code an infinite scrolling list of incidents. This can be useful when avoiding pagination to display a list of results.

<!--break-->

**Components**

#Script include

Write a server side script which returns a list of incidents depending on which page you’re on.

[https://github.com/aqibmushtaq/servicenow-infinite-scrolling/blob/master/sys_script_include_88a1a663c3332100a0791f9c64d3ae1f.xml](https://github.com/aqibmushtaq/servicenow-infinite-scrolling/blob/master/sys_script_include_88a1a663c3332100a0791f9c64d3ae1f.xml
)

#UI Script

Client side script to consume the data from the server and remember the last page.

[https://github.com/aqibmushtaq/servicenow-infinite-scrolling/blob/master/sys_ui_script_6aa36a63c3332100a0791f9c64d3ae3e.xml](https://github.com/aqibmushtaq/servicenow-infinite-scrolling/blob/master/sys_ui_script_6aa36a63c3332100a0791f9c64d3ae3e.xml
)

#UI Page
Render the results

[https://github.com/aqibmushtaq/servicenow-infinite-scrolling/blob/master/sys_ui_page_d765aa63c3332100a0791f9c64d3ae9d.xml](https://github.com/aqibmushtaq/servicenow-infinite-scrolling/blob/master/sys_ui_page_d765aa63c3332100a0791f9c64d3ae9d.xml)
