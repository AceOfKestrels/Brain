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
## Grouping Controls
In the Reactive approach we can also group controls together. To do this, instead of passing a `FormControl` directly to our form object, we can wrap it in another `FormGroup`:
```js
ngOnInit() {
    this.form = new FormGroup({
        "name": new FormGroup({
            "firstName": new FormControl(undefined),
            "lastName":  new FormControl(undefined)
        }),
        "email": new FormControl(undefined)
    });
}
```
Of course we also need to update our HTML code accordingly. The container wrapping the controls needs to have its `formGroupName` property set to the name we defined in the object:
```html
<form [formGroup]="form">
    <div formGroupName="name">
        <input type="text" id="first" formControlName="firstName">
        <input type="text" id="last" formControlName="lastName">
    </div>
    <input type="text" id="email" formControlName="email">
</form>
```

---
## Form Controls
Any HTML input uses the `FormControl` class in our form object. Its constructor takes the default value and different [validators](./validation.md) as parameters.
```js
ngOnInit() {
    this.form = new FormGroup({
        "userName": new FormControl(undefined),
        "email":    new FormControl("user@test.com")
    });
}
```
The email field has a default value of `"user@test.com"`

### Accessing Form Controls
Properties of form controls can be accessed using the `FormGroup.get` method. This takes the name or path of a control:
```html
<h1>Hello, {{ form.get("userName").value }}!</h1>
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
</form>
```
For controls inside a form group, the path is seperated by `.`:
```html
<h1>Hello, {{ form.get("name.firstName").value }}!</h1>
<form [formGroup]="form">
    <div formGroupName="name">
        <input type="text" id="first" formControlName="firstName">
        <input type="text" id="last" formControlName="lastName">
    </div>
    <input type="text" id="email" formControlName="email">
</form>
```

---
## Form Arrays
We can use form arrays to dynamically add or remove controls from a form. To do this we first add a `FormArray` object to our from object:
```js
ngOnInit() {
    this.form = new FormGroup({
        "userName": new FormControl(undefined),
        "email":    new FormControl(undefined),
        "hobbies": new FormArray([])
    });
}
```
The `FormArray` constructor takes its starting values as a parameter. In this case there are no starting values, so we pass an empty array.

We can add the controls in this array to the form by iterating over it. First we create a property to retrieve the array, casting the `FormControl` we get to a `FormArray` first:
```js
get hobbies() {
    return (this.form.get("hobbies") as FormArray).controls;
}
```
We add a `div` with the `formArrayName` property set to the name of the array:
```html
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
    <div formArrayName="hobbies">
    </div>
</form>
```

Then we can iterate over it using `*ngFor`. The `formControlName` should be the index in the array:
```html
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
    <div formArrayName="hobbies">
        <div *ngFor="let hobby of hobbies; let i = index">
            <input type=text [formControlName]="i">
        </div>
    </div>
</form>
```

We can also use `@for`:
```html
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
    <div formArrayName="hobbies">
        @for(let hobby of hobbies; track $index) {
            <input type=text [formControlName]="$index">
        }
    </div>
</form>
```
In both cases we must use property binding to set `formControlName` to the index since we are referencing a variable.

## Setting and Patching Forms
We can set the entire form to a specific object, or change (patch) specific values of it:
```js
set() {
    this.form.setValue({
        "userName": "Kes",
        "email": "kes@test.com" 
    });
}

patch() {
    this.form.patchValue({
        "userName": "Kes"
    });
}
```
The first method will set the username to `"Kes"` and the email to `"kes@test.com"`. The second method will set the username to `"Kes"` and leave the email untouched.