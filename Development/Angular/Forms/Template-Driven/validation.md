# Form Validation
Validation is important to prevent the user from submitting any form they want.

## Validation Directives
Angular offers several directives that automatically add validation to a form field. The HTML `required` attribute can be used this way, but Angular also adds [its own directives](https://angular.io/api?query=validator&type=directive):
```html
<form #form="ngForm">
    <input type="text" id="userName" name="userName" ngmodel required>
    <input type="text" id="email" name="email" ngmodel email>
</form>
```
In this example we have a form with a username field that is required, and an email field that has to be a valid email address.

---
## Checking Validation State
On its own validation does not prevent a form from being submitted. We can check the validation state however, and use it to disable the button on an invalid form for example:
```html
<form>
    <input type="text" id="userName" name="userName" ngmodel>
    <button type="submit" [disabled]="form.invalid">Submit</button>
</form>
```

## Validation Classes
Angular automatically adds the `ng-valid` or `ng-invalid` classes to a form field depending on its validation state. These can be used to style the input fields conditionally:
```css
input.ng-invalid {
    border: 1px solid red;
}
```
### Other Form Classes
Angular also adds other css classes suchs as `ng-dirty` or `ng-touched` to these fields.
Using `ng-touched` for example we can show a red border on a field only if it's invalid and has already been clicked by the user:
```css
input.ng-invalid.ng-touched {
    border: 1px solid red;
}
```
