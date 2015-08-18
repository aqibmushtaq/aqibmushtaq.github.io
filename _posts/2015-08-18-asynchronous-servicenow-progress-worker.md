---
layout: post
title: Asynchronous ServiceNow Progress Worker
date: '2015-08-18T14:22:07+01:00'
tags:
- ServiceNow
- ProgressWorker
- Async
tumblr_url: http://aqibnow.com/post/126992819112/asynchronous-servicenow-progress-worker
---
This post will explain how you can show the progress of a time consuming process in ServiceNow. We’ll achieve this via a Progress Worker.

<!--break-->

A progress worker is a async task that runs on a separate thread so you can continue your other tasks while the task executes. An example is you may continue working while a plugin is being installed.

As you can see in the image below, the progress is shown with a live status of the plugin installation.The following components are required for the progress worker:GlideModal of “simple\_progress\_viewer” to show the progress

A Script Include that does the work and updates the progress

#Example

In this post I will create a progress viewer which shows the progress of creating incidents in bulk.

**Code: [https://github.com/aqibmushtaq/snc-progress-worker](https://github.com/aqibmushtaq/snc-progress-worker)**

#1) Input fields

Let’s start of by creating the a UI Page which sets up the page with 2 input fields and a button:

**Fields:**

- Name: create\_incidents

{% highlight xml %}
<?xml version="1.0" encoding="utf-8" ?>
<j:jelly trim="false" xmlns:j="jelly:core" xmlns:g="glide" xmlns:j2="null" xmlns:g2="null">


    <label for="total" class="col-sm-3 control-label">Number of incidents to create</label>
    <input type="text" id="total" />

    <label for="prefix" class="col-sm-3 control-label">Short description prefix</label>
    <input type="text" id="prefix" />

    <button id="create_incidents" class="btn btn-primary">Create incidents</button>

    var gr = new GlideRecord(''sys_ui_script'');  
    gr.orderByDesc(''sys_updated_on'');  
    gr.query();  
    if(gr.next())
        gr.getValue(''sys_updated_on'');  
</g2:evaluate>  

</j:jelly>
{% endhighlight %}

In the above code you’ll notice the following:

- Input field for the number of incidents to create
- A button to trigger the creation

#2) UI Script to Create GlideModal

The aim of the JS UI Script is to initiate the GlideModal and call the Ajax Processor with the value of the two fields above. It will also create the required buttons on completion.

Create a **UI Script** with the following values:

- Name: create\_incident\_progress
- Global: false

{% highlight javascript %}
addLoadEvent(function() {
    event.stop();
    
    // (1) A GlideModal is created referring to simple_progress_viewer
    var dd = new GlideModal("simple_progress_viewer", false, "40em", "10.5em");
    
    var map = getMessages(["Creating incidents", "Close", "List new incidents"]);
    dd.setTitle(map["Creating incidents"]);
    
    var totalIncidents = $("total").value;
    var prefix = $("prefix").value;
    
    // (2) We set the ajax script with sysparm_ajax_processor and the parameters 
    //     all start with sysparm_ajax_processor following their name
    dd.setPreference("sysparm_ajax_processor_total_incidents", totalIncidents);
    dd.setPreference("sysparm_ajax_processor_prefix", prefix);
    dd.setPreference("sysparm_ajax_processor", ''CreateIncidentAJAXProcessor'');
    
    dd.setPreference("sysparm_renderer_progress_title", map["Creating incidents"]);
    
    // (3) Specify the buttons to display on completion by, all buttons are prefixed 
    //     with sysparm_button
    dd.setPreference("sysparm_button_new_incidents", map["List new incidents"]);
    dd.setPreference("sysparm_button_close", map["Close"]);
    
    // (4) An event listener is added on executionComplete which is called 
    //     when the progress worker is completed. A tracker object is 
    //     passed to the event listener which contains a map of values  
    //     which you will see in the CreateIncidentWorker Script Include below.
    dd.on("executionComplete", function(trackerObj) {
        // (5) Event listeners are attached to the buttons
        var newIncidentsBtn = $("sysparm_button_new_incidents");
        if (newIncidentsBtn) {
            newIncidentsBtn.on(''click'', function() {
                location.href = trackerObj.result.url_new_incidents;
            });
        }
        var closeBtn = $("sysparm_button_close");
        if (closeBtn) {
            closeBtn.on(''click'', function() {
                dd.destroy();
            });
        }
        
    });
    
    dd.render();
});
{% endhighlight %}


In the above code you’ll notice the following:

- A GlideModal is created referring to simple\_progress\_viewer
- We set the ajax script with sysparm\_ajax\_processor and the parameters all start with sysparm\_ajax\_processor following their name
- Specify the buttons to display on completion by, all buttons are prefixed with sysparm\_button
- An event listener is added on executionComplete which is called when the progress worker is completed. A tracker object is passed to the event listener which contains a map of values which you will see in the CreateIncidentWorker Script Include below.
- Event listeners are attached to the buttons

#3) Create an Ajax Processor

We now need to create an ajax processor which will start the worker code when called.

Create a Script Include with the following values:

- **Name: CreateIncidentAJAXProcessor**
- **Client callable: true**

{% highlight javascript %}
var CreateIncidentAJAXProcessor = Class.create();

CreateIncidentAJAXProcessor.prototype = Object.extendsObject(AbstractAjaxProcessor, {
check: function() {
    var trackerGr = new GlideRecord("sys_execution_tracker");
    trackerGr.addQuery("source_table", "incident");
    trackerGr.addQuery("state", "IN", "0,1");
    trackerGr.query();
    if (trackerGr.next())
        return trackerGr.getUniqueValue();
    return "";
},

/**
 * Start the Scripted Hierarchical Worker if one does not already exist
 */
start: function() {
    var trackerId = this.check();
    if (trackerId)
        return trackerId;
    
    var totalIncidents = this.getParameter("sysparm_ajax_processor_total_incidents");
    var prefix = this.getParameter("sysparm_ajax_processor_prefix");
    var worker = new GlideScriptedHierarchicalWorker();
    worker.setProgressName("Creating incidents");
    worker.setScriptIncludeName("CreateIncidentWorker");
    worker.setScriptIncludeMethod("start");
    worker.putMethodArg("totalIncidents", totalIncidents);
    worker.putMethodArg("prefix", prefix);
    worker.setBackground(true);
    worker.start();
    
    return worker.getProgressID();
},

type: 'CreateIncidentAJAXProcessor'

});
{% endhighlight %}

In the above code you’ll notice the following:

1. The start method is the entry point into the processor
2. A check is made to ensure a worker isn’t already running for the job
3. Create a GlideScriptedHierarchicalWorker with the CreateIncidentWorker Script Include that we’ll create below in **step 4**


#4) Create the Worker

Finally we need to create the worker Script Include which will do the nessacary work to complete the task. In this case it will create the specified number of incidents while updating the progress with a messageCreate a Script Include with the following values:

- **Name: CreateIncidentAJAXProcessor**
- **Client callable: true**

{% highlight javascript %}
var CreateIncidentWorker = Class.create();
CreateIncidentWorker.prototype = {
},

start: function(totalIncidents, prefix) {
    // (1) Get the execution tracker and start it with the correct max progress
    var tracker = SNC.GlideExecutionTracker.getLastRunning();
    tracker.setSourceTable("incident");
    tracker.run();
    tracker.setMaxProgressValue(totalIncidents);
    
    // (2) Create the incidents and update status
    var ids = [];
    for (var i = 0; i < totalIncidents; i++) {
        var gr = new GlideRecord("incident");
        var incidentNumber = i + 1;
        var shortDescription = prefix + " " + incidentNumber;
        gr.setValue("short_description", shortDescription);
        var id = gr.insert();
        if (id)
            ids.push(id);
        tracker.incrementProgressValue();
        var progressMsg = gs.getMessage("Created incident {0} of {1}", [incidentNumber+"", totalIncidents]);
        tracker.updateDetailMessage(progressMsg);
    }
    
    // (3) Update execution tracker result
    var completeMsg = gs.getMessage("Completed incident creation, {0} incidents created out of {1}", [ids.length+"", totalIncidents]);
    tracker.updateDetailMessage(completeMsg);
    if (ids.length == totalIncidents)
        tracker.success(gs.getMessage("The incident creation is complete"));
    else
        tracker.fail(gs.getMessage("The incident creation is complete with failures"));
    
    // (4) Generate a URL to take the user to the newly created incidents
    var gu = new GlideURL("incident_list.do");
    gu.set(''sysparm_query'', ''sys_idIN'' + ids);
		// Post URL in tracker object which is passed to the client
    tracker.updateResult(result);
  },

  type: 'CreateIncidentWorker'
};
{% endhighlight %}

In the above code you’ll notice the following:

1. We get the last running tracker and set the appropriate details on the tracker, such as source table and maximum progress
2. Start the actual work by creating incidents and updating the progress on the tracker object
3. Update the final result on the tracker
4. Create a result object (aka tracker object) which will be passed to the client in **step 2** in the **executionComplete** event handler.
