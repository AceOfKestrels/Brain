# Attribute Directives
Attribute Directives change the behavior or appearance of a component. Two built-in examples of this are `ngClass` and `ngStyle`, which can conditionally attach CSS classes or styles to an element respectively:
```html
<h2 [ngStyle]="{ color: redText ? red : black }">Text with style</h2>
<h2 [ngClass]="{ red: redText }">Text with class</h2>

<button (click)="redText=!redText">Toggle red text<button>
```
```js
redText: bool = false;
```