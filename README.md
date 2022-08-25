# _Kissflow Lowcode JavaScript SDK_

JavaScript SDK for developing over the Kissflow lowcode platform

### Use as an `npm` module

Install the SDK as a module: `npm i @kissflow/lowcode-client-sdk` Then import
into your project: Note: Initializing Kf SDK in custom components returns a
promise.

```js
import KFSDK from "@kissflow/lowcode-client-sdk";
let kf;
(async function () {
	// Immediately invoked function.
	kf = await KFSDK.initialise();
})();
```

### Use as a `<script>` tag directly in HTML

SDK can also be loaded directly into HTML by adding:

```html
<script src="https://cdn.jsdelivr.net/npm/@kissflow/lowcode-client-sdk@1/dist/kfsdk.umd.js"></script>
```

> Then SDK can be initialized anywhere by declaring:

```js
let kf;
(async function () {
	kf = await window.kf.initialise();
})();
```


## 1) Context Functions

Context functions are polymorphic, it has different classes based on execution
context.

### Custom component functions

`kf.context` returns a `CustomComponent` class while using inside custom
component. Custom component supported functions:

##### a) Watch Params

Listens for changes in parameter given to custom components in lowcode
application.

```js
kf.context.watchParams(function (data) {
	console.log(data);
});
```

### Main form functions

`kf.context` returns a `Form` class when it is used inside a kissflow's form
that could be either Process, case or Dataform & it has following supported
functions

##### a) getField()

###### Description:

Use this function to get the current value of a form field

###### Syntax:

```js
kf.context.getField(fieldId).then((res) => {...})
// or
let value = await kf.context.getField(fieldId)
```

##### b) updateField()

###### Description:

Use this function to get update any field in the form

###### Syntax:

```js
kf.context.updateField({ fieldId_1: fieldValue, fieldId_2: fieldValue });
```

##### c) toJSON()

###### Description:

Use this function to get the JSON data of the current form

###### Syntax:

```js
const json = await kf.context.toJSON();
```

###### Output:

```
{
    "Untitled_Field": "testing",
    "_created_at": "2022-03-01T03:04:09Z",
    "_flow_name": "form events",
    "_id": "Pk4_T1WGWuMe",
    "_modified_at": "2022-03-01T03:04:09Z"
}
```

---

### Table functions

`kf.currentForm.getTable(tableId)` returns a `Table` class which has the
following functions

#### a) addRow()

###### Description:

Appends row details to the end of table.

###### Syntax:

```js
const table = kf.context.getTable(tableId);
table.addRow({ columnId1: value, columnId2: value });
```

#### b) deleteRow()

###### Description:

Deletes a row from the table based on the row id

###### Syntax:

```js
const table = kf.context.getTable(tableId);
table.deleteRow(rowId);
```

#### c) getRow()

###### Description:

Use this function to perform form actions on any row inside a child table

###### Syntax:

```js
const table = kf.context.getTable(tableId);
const row = table.getRow(rowId);
```

###### Output:

Returns an instance of `TableForm` class

#### d) getRows()

###### Description:

Gets all the rows of the table

###### Syntax:

```js
const rows = await kf.context.getTable(tableId).getRows();
```

###### Output:

Returns an array of `TableForm` instances

##### e) toJSON()

###### Description:

Use this function to get the JSON data of the child table

###### Syntax:

```js
const json = await kf.context.getTable(tableId).toJSON();
```

###### Output:

```
[{
    "Untitled_Field": "row 1",
    "_created_at": "2022-03-01T03:04:09Z",
    "_flow_name": "form events",
    "_id": "Pk4_T1WGWuMe",
    "_modified_at": "2022-03-01T03:04:09Z"
},{
    "Untitled_Field": "row 2",
    "_created_at": "2022-03-01T03:04:09Z",
    "_flow_name": "form events",
    "_id": "Pk4_T1WGWuMe",
    "_modified_at": "2022-03-01T03:04:09Z"
}]
```

---

### Table Row functions

`kf.currentForm` returns a `TableForm` class which has the following functions

##### a) getField()

###### Description:

Use this function to get the value of the table row

###### Syntax:

```js
kf.context.getField(fieldId).then((res) => {...})
// or
let value = await kf.context.getField(fieldId)
```

##### b) updateField()

###### Description:

Use this function to get update any field in the table row

###### Syntax:

```js
kf.currentForm.updateField({ fieldId_1: fieldValue, fieldId_2: fieldValue });
```

##### c) getParent()

###### Description:

Use this function to perform form actions on the main form

###### Syntax:

```js
const mainForm = kf.context.getParent();
mainForm.updateField({ fieldId_1: fieldValue, fieldId_2: fieldValue });
```

###### Output:

Returns an instance of `Form` class using which we can perform any action on the
main form

##### d) toJson()

##### Description:

Get JSON output of table row

##### Syntax:

```js
const json = await kf.context.toJSON();
```

###### Output:

```
{
    "Untitled_Field": "testing",
    "_created_at": "2022-03-01T03:04:09Z",
    "_flow_name": "form events",
    "_id": "Pk4_T1WGWuMe",
    "_modified_at": "2022-03-01T03:04:09Z"
}
```

---

### 2) Client Functions

##### Show Toast

```js
kf.client.showInfo(message);
```

##### Show confirm

```js
kf.client.showConfirm({ title, content });
```

##### Redirect to URL

```js
kf.client.redirect(url);
```

---

### 3) Component Functions

getComponent(id) inside page returns component class instance.

```js
const componentName = await kf.app.page.getComponent(componentId);
```

By using component instance we can invoke following methods.

#### Refresh a component

```js
componentName.refresh();
```

#### Show a component

```js
componentName.show();
```

#### Hide a component

```js
componentName.hide();
```

### 4) Lowcode application functions

Application variables has global context to application,

##### Get value to application variable

```js
const appVarible1 = await kf.app.getVariable("variableId");
```

##### Set value of application variable

```js
let value = await kf.app.setVariable("variableId", value);
// or
await kf.app.setVariable({
	variableId_1: "value_1",
	variableId_2: 3345
});
```

##### Open a page

> Note: Page Input parameters are optional.

```js
let pageInputParameters = {
	param1: value,
	param2: value
};
kf.app.openPage(pageId, pageInputParameters);
```

##### Get values of page input parameters

```js
let value = await kf.app.page.getParameter();
```

##### Open a popup

```js
kf.app.page.openPopup(popupId, { inputParam1: 2 });
```

##### Close popup

> Closes the active popup in the page.

```js
kf.app.page.closePopup();
```

### 5) Fetch Api through kf sdk

Fetch any external api & other kissflow api using this method.

> Note: This method has a limit of 10 seconds for an api call

```js
kf.api(url, config).then((res) => {...})
// or
let resp = await kf.api(url, config)
```

---

### 6) Formatter Functions

##### Format to KF Date

```js
kf.formatter.toDate("08-24-2021").then((res) => {...})
// or
let value = await kf.formatter.toDate("08-24-2021");
```

##### Format to KF Date Time

```js
kf.formatter.toDateTime("2021-08-26T14:30").then((res) => {...})
// or
let value = await kf.formatter.toDateTime("2021-08-26T14:30");
```

##### Format to KF Number

```js
kf.formatter.toNumber("1,00,000.500000").then((res) => {...})
// or
let value = await kf.formatter.toNumber("1,00,000.500000");
```

##### Format to KF Currency

```js
kf.formatter.toCurrency("1,00,000.500000", "USD").then((res) => {...})
// or
let value = await kf.formatter.toCurrency("1,00,000.500000", "USD");
```

##### Format to KF Boolean

```js
kf.formatter.toBoolean("yes").then((res) => {...})
// or
let value = await kf.formatter.toBoolean("yes");
```

##### Other supported Boolean values

```js
let value = await kf.formatter.toBoolean("1");
let value = await kf.formatter.toBoolean("true");
let value = await kf.formatter.toBoolean("no");
let value = await kf.formatter.toBoolean("0");
let value = await kf.formatter.toBoolean("false");
```

---
