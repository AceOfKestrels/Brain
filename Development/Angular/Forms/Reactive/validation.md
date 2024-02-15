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

## Checking Valiation State
In the Reactive approach we can also check the validation state of a form control or the form itself and react to it accordingly. Read about [Accessing Form Controls](./index.md#accessing-form-controls) to see how this works:
```html
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <input type="text" id="email" formControlName="email">
    <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```

## Validation Classes
Validation classes can be used in the same way as in the [Template-Driven approach](../Reactive/validation.md#validation-classes).

## Custom Validators
While the built in validators already cover a lot of use cases, we can create custom ones as well.

A validator is simply a method that takes the control it checks as an argument and returns an object containing a boolean under any `string` type name:
```js
userNameValidation(control: FormControl): {[s: string]: boolean} {
    if( this.userNameTaken(control.value) ) {
        return { "userNameTaken": true };
    }
    return null;
}
```
Instead of returning an object with `false` in it, `null` needs to be returned if the input is valid.

Then we can pass this method to the control. Make sure to bind `this` since it will be a different object calling the method:
```js
ngOnInit() {
    this.form = new FormGroup({
        "userName": new FormControl(undefined, [
            Validators.required, 
            this.userNameValidation.bind(this)
        ]),
        "email": new FormControl(undefined, [
            Validators.required, 
            Validators.email
        ])
    });
}
```

### Validator Functions
Instead of a validator method we can also pass an arrow function directly to the control. This way we also don't need to use `bind`:
```js
ngOnInit() {
    this.form = new FormGroup({
        "userName": new FormControl(undefined, [
            Validators.required, 
            (control) => this.userNameTaken(control.value) ? { userNameTaken: true } : null
        ]),
        "email":    new FormControl(undefined, [
            Validators.required, 
            Validators.email
        ])
    });
}
```

### Asynchronous Validators
Instead of an object, async validators should return a `Promise<any>` or `Observable<any>`:
```js
userNameValidationAsync(control: FormControl): 
    Promise<any> | Observable<any> {
        return new Promise<any>(
            (resolve, reject) => {
                setTimeout( () => {
                    resolve( this.userNameTaken(control.value) ? { userNameTaken: true } : null );
                }, 1500);
            }
        );
}
```
Ansync validators are passed as the third argument to the `FormControl` constructor:
```js
ngOnInit() {
    this.form = new FormGroup({
        "userName": new FormControl(undefined, 
            Validators.required, 
            this.userNameValidationAsync.bind(this)
        ),
        "email": new FormControl(undefined, [
            Validators.required, 
            Validators.email
        ])
    });
}
```

## Error Codes
When an input is invalid, an object containing an error code is returned. We can check this error code:
```html
<form [formGroup]="form">
    <input type="text" id="name" formControlName="userName">
    <span *ngif="form.get("userName").errors["usernameTaken"]" style="color: red">
        User Name is already taken!
    </span>
    <span *ngif="form.get("userName").errors["required"]" style="color: red">
        User Name is required!
    </span>

    <input type="text" id="email" formControlName="email">
    <span *ngif="form.get("email").errors["required"]" style="color: red">
        Email is required!
    </span>

    <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```