# ServiceNow code cheat sheet
### Add the data record to the current update set 
```javascript
var gr_rec = new GlideRecord('sn_customerservice_case_subcategory_information');
if (gr_rec.get('a5c04839dba4ac509fe1df0bd3961921')) {
	var update = new GlideUpdateManager2();
	update.saveRecord(gr_rec);
}

```
### Schedule Migration
```javascript
var gr_rec = new GlideRecord('cmn_schedule');
if (gr_rec.get('5e45b3c1dbae24145a7c5cbed39619b8')) {
	var update = new GlideUpdateManager2();
	update.saveRecord(gr_rec);
}

var gr_scheduleSpan = new GlideRecord('cmn_schedule_span');
if (gr_scheduleSpan.get('6b95fb45dbae24145a7c5cbed3961954')) {
	var update = new GlideUpdateManager2();
	update.saveRecord(gr_scheduleSpan);
}

var gr_scheduleSpan2 = new GlideRecord('cmn_schedule_span');
if (gr_scheduleSpan2.get('3e4573c1dbae24145a7c5cbed396198f')) {
	var update = new GlideUpdateManager2();
	update.saveRecord(gr_scheduleSpan2);
}

var gr_otherSchedule = new GlideRecord('cmn_other_schedule');
if (gr_otherSchedule.get('689a7741dbee24145a7c5cbed3961968')) {
	var update = new GlideUpdateManager2();
	update.saveRecord(gr_otherSchedule);
}
```
### Get Group Count and Value
```javascript
var getUniqueValues = function (table, groupingAttribute) {
	var uniqueValues = [];
	var ga = new GlideAggregate(table);
	ga.addAggregate('COUNT');
	ga.groupBy(groupingAttribute);
	ga.addHaving('COUNT', '>', '0'); // get only values where count is more than 1
	ga.query();
	while (ga.next()) {
		// check if the row is for a unique value and not for an overall count
		if (ga.getDisplayValue(groupingAttribute)) {
			uniqueValues.push(ga.getDisplayValue(groupingAttribute) + ' || ' + ga.getAggregate('COUNT')); // add the value to the array
		}
	}
	return uniqueValues;
}

var uniqueValues = getUniqueValues('sn_customerservice_frontrangecmdb', 'u_companyname');

for (l1 = 0; l1 < uniqueValues.length; l1++) {
	gs.print(uniqueValues[l1]);
}

```
### Set all the editable fields to be read-only
```javascript

// Client Script
function onLoad() {
	if (g_form.getValue('state') == '5') {
		var fields = g_form.getEditableFields();
		fields.forEach(function (field) {
			g_form.setReadOnly(field, true);
		});
	}
}

```
### Datediff on two fields
```javascript
// Datediff on two fields
var functionBuilder = new GlideDBFunctionBuilder();
var myDateDiffFunction = functionBuilder.datediff();
myDateDiffFunction = functionBuilder.field('sys_updated_on');
myDateDiffFunction = functionBuilder.field('opened_at');
myDateDiffFunction = functionBuilder.build();

var now_GR = new GlideRecord('incident');
now_GR.addFunction(myDateDiffFunction);
// now_GR.addQuery(myDateDiffFunction, '<', 5);
now_GR.query();
while (now_GR.next()) {
	gs.log(now_GR.getValue(myDateDiffFunction));
	gs.log(now_GR.number);
	gs.log(now_GR.number.getDisplayValue());
}

```
### Element getRefRecord()
- Returns a GlideRecord object for a given reference element.
- **Warning: If the reference element does not contain a value, it returns an empty GlideRecord object, not a NULL object.**
```javascript
var grINC = new GlideRecord('incident');
grINC.addNotNullQuery('caller_id');
grINC.query();
if (grINC.next()) {

	// Get a GlideRecord object for the referenced sys_user record 
	var gr_request = grINC.caller_id.getRefRecord();
	if (gr_request.isValidRecord())
		gs.info(gr_request.getValue('name'));
}

```
### Element nil()
```javascript
var glideRecord = new GlideRecord('incident');
glideRecord.query('priority', '1');
glideRecord.next();
gs.info(glideRecord.state.nil());

```
### Element toString()
```javascript
var glideRecord = new GlideRecord('incident');
glideRecord.query('priority', '1');
glideRecord.next();
gs.info(glideRecord.opened_at.toString());

```
### Full text search 
```javascript
var gr = new GlideRecord('sc_cat_item');
gr.addQuery('IR_AND_OR_QUERY', 'Application');
gr.query();

while (gr.next()) {
	gs.print(gr.name + ': ' + gr.ir_query_score);
}

```
### applyEncodedQuery(String queryString)
- Sets the values of the specified encoded query terms and applies them to the current GlideRecord.
```javascript
function createAcl(table, role) {
	gs.info("Checking security on table " + table);
	var now_GR = new GlideRecord("sys_security_acl");
	now_GR.addQuery("name", table);
	now_GR.addQuery("operation", "read");
	now_GR.query();
	var encQuery = now_GR.getEncodedQuery();
	if (now_GR.next()) {
		// existing acl found so use it
		createAclRole(now_GR.getValue('sys_id'), role);
		return;
	} else {
		now_GR.initialize();
		now_GR.applyEncodedQuery(encQuery); // <-- the new record contains the query value, name = table, operation = read
		var acl = now_GR.insert();
		gs.info("Added read access control on " + table);
		createAclRole(acl, role);
	}
}

```
### deleteMultiple()
- Deletes multiple records that satisfy the query condition.
```javascript
var now_GR = new GlideRecord('incident');
now_GR.addQuery('active', 'false'); //to delete all inactive incidents
now_GR.query();
now_GR.deleteMultiple();

```
### getLink(Boolean noStack)
- Retrieves the link to the current record.
```javascript
var now_GR = new GlideRecord('incident');
now_GR.addActiveQuery();
now_GR.addQuery("priority", 1);
now_GR.query();
now_GR.next();
gs.info(gs.getProperty('glide.servlet.uri') + now_GR.getLink(false));

```
### getAllUserCriteria
```javascript
// Gets ids of all user criteria records available to the current logged in user
SNC.UserCriteriaLoader.getAllUserCriteria()

// Check if the target user criteria exists in this object through
SNC.UserCriteriaLoader.getAllUserCriteria().contains(user_criteria_sys_id)

// Get ids of all user criteria records available to the specificed user
SNC.UserCriteriaLoader.getAllUserCriteria(userID)

```
### Pop up a windows on UI Action
```javascript
//Get the values to pass into the dialog
var groupName = g_form.getValue("name");

//Initialize and open the Dialog Window
var dialog = new GlideDialogWindow("copy_group_with_member_dialog");
dialog.setTitle("Copy to a new group with its members");
dialog.setPreference("groupName", groupName);
dialog.setSize(500, 200);
dialog.render(); //Open the dialog

```
### Print the variables from the requested item
```javascript
var gr_assignmentRule = new GlideRecord('sysapproval_approver');
gs.info(gr_assignmentRule.sys_class_name);

if (gr_assignmentRule.get('fc04db3adb2b20185a7c5cbed396193f')) {
	var grRequestedItem = gr_assignmentRule.sysapproval;
	// for (var prop in grRequestedItem.variables) {
	//     if (grRequestedItem.variables.hasOwnProperty(prop)) {
	//         var variable = grRequestedItem.variables[prop];
	//         gs.print(typeof(variable));
	//         gs.print(prop + ':' + variable.getDisplayValue());
	//     }
	// }
	var variables = grRequestedItem.variables.getElements();
	for (var l1 = 0; l1 < variables.length; l1++) {
		var v = variables[l1].getQuestion();
		gs.print(v.getLabel() + ': ' + v.getDisplayValue());
	}
	gs.print(grRequestedItem.variables['location']);
}

```
### Create an event by code
```javascript
gs.eventQueue('event.name', GlideRecord, parm1, parm2); // standard form
gs.eventQueue('jos.new.active.announcement', grA, '8ee74252dbd01010a51b3a2b7c961983', '');

```
### getMessage with parameters
```javascript
parentcase.u_activity_note = gs.getMessage("State for case task {0} changed to {1}", [task, currentState]);

```
### Delete the work item time-out action, force the work assign to the agent again
```javascript
var gr_work_item_reject = new GlideRecord('awa_work_item_rejection');
gr_work_item_reject.addEncodedQuery('reason=237c67ac53202300afffddeeff7b1208^work_item.state=queued^sys_created_on<javascript:gs.beginningOfLastMinute()');
gr_work_item_reject.query();
gr_work_item_reject.deleteMultiple();

```
### Make the Catalog item variable (List Collector) to show ref_ac_columns
- Set the variable attributes to 
```javascript
ref_auto_completer = AJAXTableCompleter, ref_ac_columns = email; department, ref_ac_columns_search = true

```
### Send a workflow event
```javascript
// Send a workflow event
var grWWE = new GlideRecord('wf_workflow_execution');
if (grWWE.get('a5ee5a7cdb7030104ada026dd39619e1')) {
	fireWorkflowEvent(grWWE, 'jos.csm.state.control.approval');
}

function fireWorkflowEvent(targetRecord, evt) {
	var wf = new global.Workflow().getRunningFlows(targetRecord);
	while (wf.next()) {
		// gs.info('fireWorkflowEvent {1} wf.sys_id: {0}', wf.sys_id, targetRecord.number.toString());
		new global.Workflow().broadcastEvent(wf.sys_id, evt);
	}
}

```
### Check the ServiceNow instance Node that the user is using
```javascript
function findUserNode(userID) {
	if (userID) {
		var logs = new GlideRecord('syslog_transaction');
		logs.addQuery('sys_created_by', userID);
		logs.addQuery('sys_created_on', '>', gs.minutesAgo(15)); // This is arbitrary

		// You should add an order by here -- use sys_created_on    
		logs.setLimit(1);
		logs.query();
		if (logs.next())   // change to a loop if you want nodes for multiple sessions.
		{
			var node = logs.system_id.toString();
			var num = node.indexOf(':');
			var nodeName = node.substring(parseInt(num + 1), parseInt(node.length));
			gs.info("User " + userID + " is currently logged into node: " + nodeName);
		}
		else {
			gs.info("User " + userID + "is not currently logged in");
		}
	}
}

findUserNode('abc.abc@abc.com.hk');

```
### Stop the SLA and its workflow
```javascript
var grTaskSla = new GlideRecord('task_sla');
if (grTaskSla.get('c2ef77c2dbd1b4104631d90ed396192b')) {

	gs.info('stage: ' + grTaskSla.getValue('stage'));
	gs.info('active: ' + grTaskSla.getValue('active'));

	grTaskSla.setValue('active', false);
	grTaskSla.setValue('stage', 'completed');
	grTaskSla.setValue('end_time', new GlideDateTime());

	var workflow = new Workflow();
	workflow.cancel(grTaskSla);
	grTaskSla.update();
}

```
### Determine has role of a user
```javascript
gs.getUser().getUserByID('the record sys_id').hasRole('snc_external');

```
### User Role Determine 
```javascript
g_user.hasRole('role_name1') // client
gs.hasRole('role_name') // server

```
### Client Script setting reference field
- Set the display value as well to prevent browser fetching the display value from the server
- better performance
```javascript
g_form.setValue('assigned_to', managerSysID, managerName);

```
### UI Action + UI Script
3 methods
1. UI Macro
   1. Create a UI Macro referencing to that UI Script 
   2. Create a formatter referencing to that UI Macro
   3. put the formatter on the form
2. onLoad client script
   1. Create onLoad client script
   2. Use ScriptLoader.getScripts to load the UI Script.jsdbx
   3. use the imported function in other client scripts
3. ScriptLoader.getScripts directly
   1. Use ScriptLoader.getScripts directly in the client script and makes call in the callback function
```javascript
function testing() {
	ScriptLoader.getScripts('sn_customerservice.test_lib.jsdbx', function () {
		sn_customerservice.test_lib.sayhello();
	});
}

```
### Update the Journal Entry History
1. update sys_audit table value if found
2. update sys_journal_field table value
3. run the history refresh
```javascript
new GlideHistorySet("sn_customerservice_case", "1c016629dbb134504631d90ed3961954").refresh();

```
### Check the queue missing setups
```javascript
// Check the queue missing setups
var grAwaQueue = new GlideRecord('awa_queue');
grAwaQueue.orderBy('name');
grAwaQueue.query();
while (grAwaQueue.next()) {
    var grAEP = new GlideRecord('awa_eligibility_pool');
    grAEP.addQuery('queue', grAwaQueue.getUniqueValue());
    grAEP.setLimit(1);
    grAEP.query();
    if (!grAEP.hasNext()) {
        gs.log('', grAwaQueue.getDisplayValue('name'));
    }
}

```
