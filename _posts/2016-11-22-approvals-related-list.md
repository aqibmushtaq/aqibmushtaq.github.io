---
layout: post
title:  "Approval Related List with a Relationship"
date: 2016-11-22T12:15:00+00:00
tags:
- ServiceNow
- Related List
- Approvals
- Relationship
---

When implementing an approval workflow in ServiceNow you may consider adding a related list showing all the related approval records on approving record.

<!--break-->

Out of the box you cannot add the related list as the approval record (sysapprover_approval) uses a Document ID field rather than a reference. However you can create a relationship between the two table which will allow you to add the related list. The relationship is just a set of queries on the approval table to filter out irrelevant records.

Create a Relationship with the following values

 1. **Name:** [Your table name] Approvals
 2. **Applies to table:** [Your table name]
 3. **Queries from table:** Approval [sysapprover_approval]
 4. **Query with:**

{% highlight javascript %}
(function refineQuery(current, parent) {

	current.addQuery('source_table', parent.getTableName());
	current.addQuery('document_id', parent.getUniqueValue());

})(current, parent);
{% endhighlight %}
