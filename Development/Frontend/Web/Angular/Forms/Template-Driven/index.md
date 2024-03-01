# Template-Driven Forms
Using the Template-Driven Form approach, Angular infers the structure of the object representing the form directly from the DOM. 

## Setup

First the `FormsModule` must be imported in the app module:
```js
imports: [ FormsModule ]
```

Angular then automatically creates a representation for any `<form>` object it detects. However, it will not automatically detects the inputs in those forms. To add an input to the object, add the `ngModel` directive to it:
```html
<form>
    <input type="text" id="userName" name="userName" ngmodel>
</form>
```
In addition to the `ngModel` directive, the `name` attribute also needs to be set in order for Angular to be able to add the input to the object.

## Submitting a Form
On a form we can listen to the `ngSubmit` event, which gets triggered when the form is submitted:
```html
<form (ngSubmit)="onSubmit()">
    <input type="text" id="userName" name="userName" ngmodel>
    <button type="submit">Submit</button>
</form>
```

## Accessing the Form Object
To access the form object created by Angular, create a local reference of the element and set it to `ngForm`. This local reference of type `NgForm` can then be used to access the object:
```html
<form #form="ngForm">
    <input type="text" id="userName" name="userName" ngmodel>
</form>
```
&nbsp;
```js
@ViewChild("form") 
formRef: NgForm;
```

### Accessing Form Fields
Similarly we can add a local reference set to `ngModel` to any form field to get access to its representation:
```html
<form>
    <input type="text" id="userName" name="userName" ngmodel #nameInput="ngModel">
</form>
```
&nbsp;
```js
@ViewChild("nameInput") 
formRef: NgModel;
```

## Model Grouping
Multiple form fields can be grouped together, for example to check validity of multiple inputs at once, using the `ngModelGroup` property:
```html
<form #form="ngForm">
    <div id="userName" ngModelGroup="userName">
        <input type="text" id="firstName" name="firstname" ngmodel>
        <input type="text" id="lastName" name="lastName" ngmodel>
    </div>
    <input type="text" id="email" name="email" ngmodel>
</form>
```
