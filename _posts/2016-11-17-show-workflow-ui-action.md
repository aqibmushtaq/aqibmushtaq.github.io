---
layout: post
title:  "Show Workflow UI Action"
date: 2016-11-17T17:35:00+00:00
tags:
- ServiceNow
- UI Action
- Workflows
---

When working with workflows it is really useful to view the context of workflows through the target record. I'll show how to create a UI Action to view the workflow context from the target record form.

<!--break-->

The alternative is to navigate to the Workflow Context (wf_context) table and find the correct context for your record.

Create the following UI Action:

 - Name: Show Workflow
 - Table: [Your table name]
 - Onclick: {% highlight javascript %} showWorkflowContext(){% endhighlight %}
 - Code: 
{% highlight javascript %}
function showWorkflowContext() {
	var url = new GlideURL('context_workflow.do');
	url.addParam('sysparm_stack', 'no');
	url.addParam('sysparm_table', 'x_tori2_tsm_timesheet');
	url.addParam('sysparm_document', g_form.getValue('sys_uniqueValue'));
	getTopWindow().popupOpenFocus(url.getURL(), 'show_workflow_context', 950, 700, '', false, false);
}
{% endhighlight %}
