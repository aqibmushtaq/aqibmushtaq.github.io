---
title:  "Take ESS User Directly to the Service Portal"
date: '2018-12-14T09:00:00+01:00'
tags:
- serviceportal
- portal
- ess
- login
- redirect
- SPEntryPage
---

Users without roles are usually only require access to the self service view of ServiceNow, which is now the Service Portal.

They do not need access to the backend forms and lists. For most new ServiceNow projects you'll be required to direct role-less users to the ServicePortal.

<!--break-->

In order to achieve this, you'll just need to create a System Property.

**Name:** glide.entry.first.page.script

**Value:** new SPEntryPage().getFirstPageURL();

**Optional**

In addition to this, if you want to further restrict a particular role such as ITIL to only have access to the backend then amend the `SPEntryPage` Script Include.

Replace this line in Script Include (line 69 at the time of writing this):

```javascript
if (user.hasRoles() && !redirectURL && !isServicePortalURL)
```

With the line

```javascript
if (user.hasRole('itil') && !redirectURL && !isServicePortalURL)
```
