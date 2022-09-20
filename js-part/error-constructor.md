# Error Constructor

Javascript Error() constructor is used to create a new `error object`. Error objects are arising
at runtime errors.

The error object also uses as the base object for the exceptions defined by the user

## Syntax

```js
new Error([message[, fileName[, lineNumber]]])
```

## Parameters

- message: It contains information about this error object which is in human-readable form.
  An error message can be set using javascript Error message property. It is an optional parameter

- fileName: It is the name of the file for this error object. If no name is provided than fileName
  is equal to the name of file containing the code that called the Error() constructor. It is
  an optional parameter

- lineNumber: It is the calue for the lineNumber property on the created Error object. If no numbers
  is provided Than the lineNumbers is equal to the line number containing the Error() constructor invocation.
  It is an optional parameter

```js
try {
  const error = new Error("This object is created using new keyword");
} catch (err) {
  console.log("err--->", err);
}
```

[实例](./../test/error-cons.html)
