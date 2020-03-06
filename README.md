# StandardFusion

This integration recieves and parses the payload from StandardFusion and generates an xMatters event to notify the designated recipients. This is a starting point for building out simple and complex workflows.


---------

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

---------

# Files

* [StandardFusion.zip](StandardFusion.zip) - Workflow zip file
* [standardfusion.png](standardfusion.png) - Standard Fusion logo

# How it works
This step takes a Standard Fusion payload and creates flow designer outputs.


# Installation

## xMatters Setup
1. Download the [StandardFusion.zip](StandardFusion.zip) file onto your local computer
2. Navigate to the Workflows section of your xMatters instance
3. Click Import, and select the zip file you just downloaded and when finished importing, click Open Workflow. 
4. Click the Incident canvas in the Flows tab, then double click the *Incident - Inbound from Standard Fusion* http trigger. 
5. Copy the Initiation URL and store for later, then close the dialog. 
6. Double click the Create xMatters event step and populate the Recipients field. Alternatively, [subscriptions](https://help.xmatters.com/ondemand/userguide/receivingalerts/subscriptions/sharingsubscriptions.htm) can be set up to target the appropriate parties based on components in the event properties. 

<kbd>
	<img src="/media/recipients.png" width="400">
</kbd>

7. Save the step and repeat as necessary for the other xMatters Event Steps.
8. Save the canvas. 

## StandardFusion Setup
1. Navigate to Settings > System > Integrations.
<kbd>
	<img src="/media/integrations.png" width="400">
</kbd>

2. Create a new Integration called "Fire to xMatters".
3. Paste in the HTTP Trigger url from above and paste in the following for the FieldMappingsJson.

FieldMappingsJson:
```
{
   "Target.Key":"Key",
   "Target.Name":"Name",
   "Target.Description":"Description",
   "Target.WorkflowState":"Status",
   "Target.Notes":"Notes",
   "Target.Owner_DisplayName":"Owner",
   "Target.IncidentDate":"Incident Date",
   "Target.Severity":"Severity",
   "Target.Category":"Category",
   "Target.ClassificationLevel":"Classification",
   "Target.Tags":"Tags",
   "Target.Folder":"Folder",
   "Target.ID":"ID",
   "Event.EventType":"EventType"
}
```

<kbd>
	<img src="/media/firetoxm.png" width="400">
</kbd>

4. Navigate to the Advanced tab and replace the `"EventRules": []` with the following.
```
{
   "EventRules": [
    {
      "Name": "Send on Incident Creation",
      "IsEnabled": true,
      "EventType": "CoreRecordCreated",
      "IsAsync": true,
      "ActionName": "Fire to xMatters.PostToWebhook",
      "RecordTypeName": "SF.Incidents"
    },
    {
      "Name": "Send on Incident Workflow State Changed",
      "IsEnabled": true,
      "EventType": "CoreWorkflowTransition",
      "IsAsync": true,
      "ActionName": "Fire to xMatters.PostToWebhook",
      "RecordTypeName": "SF.Incidents"
    },
    {
      "Name": "Send on Incident Record Modified",
      "IsEnabled": false,
      "EventType": "CoreRecordModified",
      "IsAsync": true,
      "ActionName": "Fire to xMatters.PostToWebhook",
      "RecordTypeName": "SF.Incidents"
    }
  ]
}
```
<kbd>
	<img src="/media/eventrules.png" width="400">
</kbd>

5. Check the appropriate boxes on the Event Rules for when to fire to xMatters.




## Usage
The **Inbound from Standard Fusion** HTTP Trigger is now available in your custom steps. So navigate to the appropriate canvas so you can add the step there. If you'd like to experiment with it, the **Incident** workflow has a canvas that can be triggered via HTTP call. 

### Inputs
Example StandardFusion Payload:
```
{
  "Key": "IC-38",
  "Name": "Installation of software",
  "Description": "The office cat's laptop needs new meoware.",
  "Status": "Open",
  "Notes": "",
  "Owner": "IT Support Dog",
  "Incident Date": "2020-02-11T00:00:00Z",
  "Severity": "High",
  "Category": "Internal Audit Finding",
  "Classification": null,
  "Tags": "",
  "Folder": null,
  "ID": "171870",
  "EventType": "CoreWorkflowTransition"
}
```


### Outputs

| Name |
| ---- |
| ID |
| Description |
| Status |
| Notes |
| Owner |
| Key |
| Name |
| Severity |
| Incident Date |
| Folder |
| Category |
| Tags |
| Classification |


## Example
This is an example taking in a Standard Fusion payload and creating an xMatters event based on the Status.

<kbd>
	<img src="/media/ExampleFlow.png">
</kbd>

