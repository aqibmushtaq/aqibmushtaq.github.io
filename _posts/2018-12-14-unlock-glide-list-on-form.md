---
title:  "Load a GlideList Field Unlocked"
date: '2018-12-14T09:00:00+01:00'
tags:
- form
- glidelist
- unlocked
- unlock
---

List fields on forms can be clunky when they're used for heavily used fields. Sometimes it may make it easier to load the field in it's unlocked state to improve usability.

<!--break-->

This requires a dictionary attribute on the list field you want to keep unlocked.

**1)** Right click the list field's label.

<img src="/assets/images/start_locked_1.png" style="width:400px;height:200px;" />


**2)** Append your new attribute `start_locked=false`

<img src="/assets/images/start_locked_2.png" style="width:500px;height:170px;" />


**3)** Result

<img src="/assets/images/start_locked_3.png" style="width:519px;height:300px;" />
