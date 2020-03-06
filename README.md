# Standard Fusion Steps

This step allows you to recieve a payload from Standard Fusion.


---------

<kbd>
  <img src="https://github.com/xmatters/xMatters-Labs/raw/master/media/disclaimer.png">
</kbd>

---------

# Files

* [StandardFusionSteps.zip](StandardFusionSteps.zip) - Workflow zip file with the step and example flow
* [standardfusion.png](/standardfusion.png) - Standard Fusion logo

# How it works
This step takes a Standard Fusion payload and creates flow designer outputs.


# Installation

## StandardFusion Setup
1. Navigate to Settings > System > Integrations.
2. Create a new Integration called "Fire to xMatters".
3. Paste in the HTTP Trigger url and paste in the following for the FieldMappingsJson.

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
4. Navigate to the Advanced tab and replace the "EventRules": [] with the following.

Advanced tab content:
```
{
  "WebhookUrl": "https://acme.xmatters.com/api/integration/1/functions/UUID/triggers?apiKey=API_KEY",
  "FieldMappingsJson": "{\n   \"Target.Key\":\"Key\",\n   \"Target.Name\":\"Name\",\n   \"Target.Description\":\"Description\",\n   \"Target.WorkflowState\":\"Status\",\n   \"Target.Notes\":\"Notes\",\n   \"Target.Owner_DisplayName\":\"Owner\",\n   \"Target.IncidentDate\":\"Incident Date\",\n   \"Target.Severity\":\"Severity\",\n   \"Target.Category\":\"Category\",\n   \"Target.ClassificationLevel\":\"Classification\",\n   \"Target.Tags\":\"Tags\",\n   \"Target.Folder\":\"Folder\",\n   \"Target.ID\":\"ID\",\n   \"Event.EventType\":\"EventType\",\n   \"Target.Custom_ClosingDate\": \"Incident Close Date\"\n}",
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
5. Check the appropriate boxes on the Event Rules for when to fire to xMatters.

## xMatters Setup
1. Download the [StandardFusionSteps.zip](StandardFusionSteps.zip) file onto your local computer
2. Navigate to the Developer tab of your xMatters instance
3. Click Import, and select the zip file you just downloaded


## Usage
The **Inbound from Standard Fusion** HTTP Trigger is now available in your custom steps. So navigate to the appropriate canvas so you can add the step there. If you'd like to experiment with it, the **Incident** workflow has a canvas that can be triggered via HTTP call. 

### Inputs
StandardFusion Payload:
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

