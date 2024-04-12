# skuidCustom.js
This is a custom library allowing for function calls to the skuid.custom (eg. skuid.custom.modelLoader(model,params)). This library can be embedded as an In-line in the SKUID Editor, or as a Static Resource from Salesforce. Also adds functions helpful to working with Dates and Objects.

This library has been tested and is working with SKUID 15.3.7 and API v1.

## skuidCustom.js Functions

### skuid.custom.isBlank(v)
* v: a variable

Returns true if the variable v is blank (undefined, null, or '').

### skuid.custom.blockUI(obj)
* obj: object passed to the standard blockUI skuid function

Blocks the UI as the standard SKUID blockUI does, except if just the message is changing, it will update the message instead of re-initializing the UI block. This will prevent the UI from momentarily unblocking when it is already blocked.

### Array.prototype.includesAny(arr)
* arr: a variable, or an array

Returns true if any items in the current array are also in the passed array or are equal to the passed variable.

### Array.prototype.includesAll(arr)
* arr: a variable, or an array

Returns true of all items in the passed array (or variable) are also in the current array.

### Date.isLeapYear(year)
* year: a year

Takes a year and returns true if it is a leap year, otherwise returns false. Can also be used on a javascript date directly and will return true if the year of that date is a leap year. (eg. d.isLeapYear();)

### Date.getDaysInMonth(year,month)
* year:  a year
* month:  a month

Takes a  year and a month and returns the number of days in that month. Can also be used on a javascript date directly and will return the number of days in the month of that date. (eg. d.getDaysInMonth();)

### Date.addMonths(months)
* months:  months to add to the given date.

Adds months to the given javascript date and returns a new javascript date with the months added (eg. d.addMonths(3);)

### Date.addDays(days)
* days: days to add to the given date.

Adds days to the given javascript date and returns a new javascript date with the days added (eg. d.addDays(3);)

### skuid.custom.isValidSFDate(dateString)
* dateString: date in string format

Returns true if the dateString is a valid SF date in the format YYYY-MM-DD, eg. 2000-01-30

### skuid.custom.fixCurrency(number)
* number: any number

Javascript mathematical operations on floating point numbers can return your result but then also append an additional small value to the number due to "fuzzy math", eg. (1.020000000000000004 when the number should have been 1.02); this function will remove anything past 2 decimal places for the number and return the fixed number.

### skuid.custom.fixCurrencyStr(str)
* str: a string representing a currency value (eg. $1,000.00)

Returns the passed str stripped of any special characters (eg. $1,000.01 -> 1000.01)

### skuid.custom.formatPercentDone(percentDone)
* percentDone: a percentage value from 0-100

Returns the percentage rounded to the nearest 5%. This is used along with creating a picklist based progress indicator that scales in increments of 5 from 0-100.

### skuid.custom.clearArray(array)
* array: an array

Force clears data from an array by popping any items that may still be on it.

### skuid.custom.operatorSOQL(operatorSafe)
* operatorSafe: String of an operator in an HTML/XML safe format (eg. "gte") to be converted

Converts a string for an operator from XML/HTML safe format to SOQL format (eg. "gte" converts to ">=") and returns the converted operator string.

### skuid.custom.operatorSafe(operatorSOQL)
**operatorSOQL:** String of an operator in SOQL format (eg. ">=") to be converted

Converts a string for an operator in SOQL format to HTML/XML safe format (eg. ">=" converts to "gte") and returns the converted operator string.

### skuid.custom.htmlEscape(string)
**string:** string to escape

HTML escapes a string. For use when creating XML to define template contents when creating a custom component via XML. Returns the HTML escaped string to be placed in the `<contents>` area of the XML defining the template field.

### skuid.custom.reRenderComponent(componentId)
* componentId: Id of a component on the page

Try to re-render a component, catches the error if the component cannot be re-rendered at this time.

### skuid.custom.waitForElement(fparams,callback)
* fparams = {
	* initEle: function to initialize the element / check if the element is initialized, should return the element. Takes context as a variable.
	* context:passed context or defaults to window
	* chunkTime: time between chunks, defaults to 250
 }
 * callback: function to run when the element is determined to be ready

Wait for an element to be ready on the screen before performing the callback function. Useful if needing to make a rendering update to an item on the screen but the item may not yet be fully rendered on the screen. This function will keep trying until the item is ready and then will call the callback function.

### skuid.custom.createUpdateExistingRow(model,row)
* model: passed model
* row: object of field value pairs. Must include a Salesforce Id field

Create a row for an already existing Salesforce object with any specified fields acting as updates to the row when saving. createRow will normally assume the newly created row needs to be inserted, but if this is an existing row we will instead be updating the row when saving the model.

### skuid.custom.createUpdateExistingRows(ourModel,rows)
* ourModel: passed model
* row: array of objects of field value pairs that must include a salesforce Id field.

Create rows for already existing Salesforce objects with any specified fields acting as updates to those rows when saving. createRow will normally assume the newly created row needs to be inserted, but if this is an existing row we will instead be updating the row when saving the model

### Object.byString(o,s)
* o: an object
* s: string for accessing sub-objects

Access sub-objects using a single string. eg. "MainObject.SubObject.SubSubObject.Field"

### skuid.custom.iterateAsync(data, fn, chunkEndFn, maxTimePerChunk, context)
* data: data to iterate over in an array, map, or object
* fn: function to call on each iteration
* chunkEndFn: function to call whenever an asynchronous chunk completes (optional); parameters: row,length,percentDone
* endFn: function to call when the loop completes (optional)
* maxTimePerChunk: time between asynchronous chunks. (optional, default: 200)
* context: passed context (optional, default: window)

Iterate over an array, map, or object asynchronously allowing for long loops to not freeze the browser tab. chunkEndFn can be used to update a loading message on the SKUID page.

If iterating over Model Rows, first get the rows into an array using model.getRows(), use that as the array variable.

**Usage**
```
iterateAsync(ourArrayMapOrObject,function(value, index, array){
	//runs each iteration of the loop
},
function(row,length,percentDone){
	//runs after every chunk completes, this is optional, use undefined if not using this
},
function(){
	//runs after completing the loop, this is optional, use undefined if not using this
});
```

### skuid.custom.iterateArrayAsync(array, fn, chunkEndFn, endFn, maxTimePerChunk, context)
* array: array to iterate over
* fn: function to call on each iteration
* chunkEndFn: function to call whenever an asynchronous chunk completes (optional); parameters: row,length,percentDone
* endFn: function to call when the loop completes (optional)
* maxTimePerChunk: time between asynchronous chunks. (optional, default: 200)
* context: passed context (optional, default: window)

Iterate over an array asynchronously allowing for long loops to not freeze the browser tab. chunkEndFn can be used to update a loading message on the SKUID page.

If iterating over Model Rows, first get the rows into an array using model.getRows(), use that as the array variable.

**Usage**
```
iterateArrayAsync(ourArray,function(value, index, array){
	//runs each iteration of the loop
},
function(row,length,percentDone){
	//runs after every chunk completes, this is optional, use undefined if not using this
},
function(){
	//runs after completing the loop, this is optional, use undefined if not using this
});
```

### skuid.custom.iterateMapAsync(map, fn, chunkEndFn, endFn, maxTimePerChunk, context)
* map: map to iterate over
* fn: function to call on each iteration
* chunkEndFn: function to call whenever an asynchronous chunk completes (optional); parameters: row,length,percentDone
* endFn: function to call when the loop completes (optional)
* maxTimePerChunk: time between asynchronous chunks. (optional, default: 200)
* context: passed context (optional, default: window)

Iterate over a map (javascript map defined as Map()) asynchronously allowing for long loops to not freeze the browser tab. chunkEndFn can be used to update a loading message on the SKUID page.

**Usage**
```
iterateMapAsync(ourMap,function(value, key, map){
	//runs each iteration of the loop
},
function(index,length,percentDone){
	//runs after every chunk completes, this is optional, use undefined if not using this
},
function(){
	//runs after completing the loop, this is optional, use undefined if not using this
	
});
```

### skuid.custom.iterateObjAsync(obj, fn, chunkEndFn, endFn, maxTimePerChunk, context)
* obj: object to iterate over
* fn: function to call on each iteration
* chunkEndFn: function to call whenever an asynchronous chunk completes (optional); parameters: row,length,percentDone
* endFn: function to call when the loop completes (optional)
* maxTimePerChunk: time between asynchronous chunks. (optional, default: 200)
* context: passed context (optional, default: window)

Iterate over an object asynchronously allowing for long loops to not freeze the browser tab. chunkEndFn can be used to update a loading message on the SKUID page.

**Usage**
```
iterateObjAsync(ourObj,function(value, key, map){
	//runs each iteration of the loop
},
function(index,length,percentDone){
	//runs after every chunk completes, this is optional, use undefined if not using this
},
function(){
	//runs after completing the loop, this is optional, use undefined if not using this
	
});
```

### skuid.custom.modelLoader(model[],fparams)
* model: model or array of models to load
* fparams = {
    * limit: Number of rows to limit by. If unspecified will choose the model's recordsLimit property	or if that is unspecified defaults to 200
    * progressCallback: function to call before running each individual query in the format progressCallback(fparams), fparams is an object
        progressCallback fparams = {
            * count: count of rows queried so far
            * limit: our limit for how many rows to query per query run
            * nextStart: the row # of the next row to be queried,
            * nextEnd: the last row # to be queried (based on limit)
        }
    * exportWhenDone: true or false for whether or not to export when done loading, default false
    * exportOptions: options object to pass to the export function for customized options
}

Clear and Load all records in a model (or array of models) in chunks limited by the model's limit propery, a passed limit, or otherwise if both are undefined defaults to 200.

This will load all rows from the model's query in chunks, preventing APEX Heap Errors when querying too much data at once.

Model can be Basic or Aggregate, both will load in chunks limited by the limit.

Runs Asynchronously and works with $.when();

Will take into account any currently active/inactive filters and search-box specified searches on the model as well as sort order.

**NOTE:** Cannot use subquerying on aggregate models as we need to use OR to run more than one query and OR is not allowed along with subqueries in SOQL

**NOTE:** Uses direct SOQL queries. Operates faster than normal model.load(), but will use system permissions instead of user permissions to accomplish the querying, so the query will ignore the user's access to the object and assume system level access.

**Usage**
```
//Progress Callback function
let callback = function(fparams){
	var vm = skuid.$M('VariableModel');
	var vr = vm.getFirstRow();
	
    //Update variable model LoaderMsg with the progress of the loading
	vm.updateRow(vr,{
		LoaderMsg: 'Loading: '+fparams['nextStart']+'-'+fparams['nextEnd']
	});
}

//Call the model loader specifying our progress callback, a limit of 700 rows at a time, and we are specifying to export when done
$.when(skuid.custom.modelLoader(skuid.$M('OurModel'),{progressCallback: callback,limit:700,exportWhenDone: true})).done(function(){
	console.log('LOADING DONE');
	var vm = skuid.$M('Vars');
	var vr = vm.getFirstRow();

    //Update our variables model when done to indicate we've completed the query
	vm.updateRow(vr,{
		LoaderMsg: '',
		Done: true
	});
	
	console.log('MODELLOADER DONE');
}).fail(function(){
    //fail logic here
});
```

### skuid.custom.modelSaver(model[],fparams)
* model: model or array of models to save
* fparams = {
    * limit: Number of rows to limit by. If unspecified will choose the model's recordsLimit property or if that is unspecified defaults to 100
    * progressCallback: function to call before running each individual query in the format progressCallback(fparams), fparams is an object
        progressCallback fparams = {
            * count: count of rows queried so far
            * limit: our limit for how many rows to query per query run
            * nextStart: the row # of the next row to be queried,
            * nextEnd: the last row # to be queried (based on limit)
	    * model: the model currently being saved
        }
}

Saves a model (or array of models) incrementally as to not overload the save process with too many saves at once

### skuid.custom.sheetJSFromTable(c, options)
* c: table's component or component's ID
* options: 
* {
*	showHidden: true //defaults to true
* }

Takes a table component or a table component's ID and returns array of arrays to use for the XLSX.utils.aoa_to_sheet function in the SheetJS library. This is for exporting a table to an Excel file using SheetJS.

Table MUST have "Show Export Button" selected

This currently does not work with template fields. As a workaround, create your "template" fields using formula fields on the model instead.

### skuid.custom.sheetJSData(d, options)
* d: model.data array
* options:
* {
*		// fields to include in export
*		fields: [
*			{id: 'FIELDID', name: 'RENAMEFIELDCOLUMNHEADERTOTHIS'}
*		]
*		includeId: false //defaults to false
* }

Takes model.data and options object and returns an array of arrays to use for the XLSX.utils.aoa_to_sheet function in the SheetJS library. This is for exporting model data directly to an Excel file using SheetJS.

### skuid.custom.setHidden(f)
* options:
*{
*	table: table component to update (optional, use either this or tableId)
*	tableId: id of table component to update (optional, use either this or table)
*	key: the key to use to match table column, can either use the column's label, or fieldId
*	by: "label" or "fieldId" depending on how you want to select the column; defaults to "label"
*	value: true to hide, false to unhide; defaults to true
*	wrapper: optional wrapper to re-render in order to refresh the display so the changes are visible - NOTE: if making more than 1 change at a time do not use this as it will force several re-renders and can slow/freeze the UI
*}

Takes an object consisting of table and properties and sets an item in the table to display or not display based on properties


### skuid.custom.reRenderModelComponents(model)
* model: the model to reRender components for

Rerenders all components that are tied to the model.

NOTE: This may or may not work in all circumstances

### skuid.custom.isFieldUIOnly(model,fieldKey)
* model: the model to check
* fieldKey: the field to check

Returns true if the field is a UI Only field

### skuid.custom.removeUIOnlyChanges(model)
* model: the model to remove UI Only changes from

Removes any UI Only changes from the model, affecting the display of components

NOTE: This may not work in all circumstances.

### skuid.custom.getConditionByName(model,conditionName)
* model: the model to search
* conditionName: the name of the condition to search for

Get any condition by name, including subconditions (provided condition names are unique within the model)

Operates like model.getConditionByName, but also searches subconditions

Returns the condition object if found, otherwise returns false

### skuid.custom.escapeStringForTemplate(str)
* str: the string to escape

Escape string for use in a template with `` (backticks)

### skuid.custom.overrideModelLoad(model)
* model: the model to override load function for

Override model.load function to use custom modelLoader.
