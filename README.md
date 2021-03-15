# Move-Blueprint-Transition-via-Deluge
A Deluge script that allows you to push a blueprint transition through for a record.

## Core Idea
[Blueprints](https://www.zoho.com/crm/blueprint.html) are designed to streamline work processes where a user can simply click through blueprint transitions to advance the state of a record. In a more complex blueprint, there are situations where you may need to advance a record state as part of a Deluge automation. Zoho gives developers the flexibility to do so with the [Blueprint API functions](https://www.zoho.com/crm/developer/docs/api/v2/blueprint-details.html).

## Configuration
The following scope is needed
* ZohoCRM.modules.ALL

## Tutorial

### 1. Get the Blueprint Transition ID
In order to push a blueprint transition via Deluge, you first need to get the blueprint transition ID. This can be done with the API call below:
* To get the desired transition IDs, the CRM record needs to be in a state where the transitions are available as the next transition.
* If you're doing this in Sandbox, the transition IDs change after deployment. You will need to get the transition IDs again when deployed to production, and change your script(s) accordingly.

```javascript
response = invokeurl
[
	url: "https://www.zohoapis.com/crm/v2/CRM_Module_Name/CRM_Record_ID/actions/blueprint"
	type: GET
	connection:"crm_oauth_connection"
];
info response;
```
*Note: Replace "CRM_Module_Name", "CRM_Record_ID" and "crm_oauth_connection" accordingly.*


### 2. Push the Blueprint Transition

Once you have the transition ID that you need, stick the ID in the map based on the format below.
* Similar to getting bluprint transitions, pushing transitions also requires the CRM record to be in a state where the desired transition is available as the next transition.
* You also need to make sure that all transition criteria are met: e.g. if a transition has certain mandatory fields & validation.
* An error is thrown if the record is not in transition, transition_id is wrong, field_value data type mismatches, or field validation fails.

```javascript
dataMap = Map();
dataMap.put("Notes", "Updated via blueprint");
blueprint1 = Map();
blueprint1.put("transition_id", "692969000000981130");
blueprint1.put("data", dataMap);
blueprintList = List();
blueprintList.add(blueprint1);
param = Map();
param.put("blueprint", blueprintList);
response = invokeurl
[
	url: "https://www.zohoapis.com/crm/v2/CRM_Module_Name/CRM_Record_ID/actions/blueprint"
	type: PUT
	parameters: param.toString()
	connection:"crm_oauth_connection"
];
info response;
```
*Note:*
* *Replace "CRM_Module_Name", "CRM_Record_ID", "crm_oauth_connection" accordingly.*
* *Replace "692969000000981130" with the correct transition ID*
