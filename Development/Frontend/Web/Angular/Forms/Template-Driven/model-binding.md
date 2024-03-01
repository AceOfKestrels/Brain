# Model Binding
Like we have seen in [databinding](../../Basics/databinding.md) we can bind to the `ngModel` property.

## Default Values
We can use one-way-binding to set a default value for form fields:
```js
const defaultOption = "a";
```
&nbsp;
```html
<form>
    <select id="question1" name="question1" [ngModel]="defaultOption">
        <option value="a">Option A</option>
        <option value="b">Option B</option>
    </select>
</form>
```

## Displaying User Input
Two-way-binding can be used to immediately display a user's input by binding it to a property:
```js
const userName = "";
```
&nbsp;
```html
<h1>Hello, {{ userName }}!</h1>
<form>
    <input type="text" id="userName" name="userName" [(ngmodel)]="userName">
</form>
```