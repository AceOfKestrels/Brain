# Reactive Forms
Using the Reactive approach, the object representing the form must be generated programmatically in TypeScript. While this seems more complicated than the Template-Driven approach, it allows for more flexibility and precision.

## Setup
First the `ReactiveFormsModule` must be imported in the app module:
```js
imports: [ ReactiveFormsModule ]
```
In the component TypeScrit file, create a property of type `FormGroup` for the object:
```js
form: FormGroup;
```
Initialize the property. This does not need to happen right away, but should be done before rendering (read [Lifecycle Hooks](../../Basics/lifecycle-hooks.md) for more). The `FormGroup` constructor takes an object as a parameter that contains the form controls:
```js
ngOnInit() {
    this.form = new FormGroup({
        "userName": new FormControl(undefined),
        "email":    new FormControl(undefined)
    });
}
```
To connect our object to the form itself, it must be bound to the `formGroup` property:
```html
<form [formGroup]="form">
    <input type="text" id="name">
    <input type="text" id="email">
</form>
```
Then for each form control, the property name in the object must be passed to the `formControlName` directive:
```html
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
</form>
```

---
## Form Controls
Any HTML input uses the `FormControl` class in our form object. Its constructor takes the default value and different [validators](./validation.md) as parameters.

### Accessing Form Controls
Properties of form controls can be accessed using the `FormGroup.get` method:
```html
<h1>Hello, {{ form.get("userName").value }}!</h1>
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
</form>
```