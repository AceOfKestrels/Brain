# Validation
In the Reactive approach adding validation works differently from the Template-Driven approach. Validation must be added to the object in TypeScript, *not* the HTML elements.

## Validation Methods:
The `Validators` class offers several built in validation methods that can be added to our `FormControl` objects. Make sure to not call the methods, just pass them to the constructor as arguments:
```js
ngOnInit() {
    this.form = new FormGroup({
        "userName": new FormControl(undefined, Validators.required),
        "email":    new FormControl(undefined, [Validators.required, Validators.email])
    });
}
```
A single validators can be passed direct. Instead an array of validators can be passed to the constructor.

---
## Checking Valiation State
In the Reactive approach we can also check the validation state of a form control or the form itself and react to it accordingly. Read about [Accessing Form Controls](./index.md#accessing-form-controls) to see how this works:
```html
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
    <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```

---
## Validation Classes
Validation classes can be used in the same way as in the [Reactive approach](../Reactive/validation.md#validation-classes).