# Structural Directives
Structural Directives change the structure of the DOM. Under the hood they are replaced with an `ng-template`, but they can be added to elements by prefixing them with a `*`. Two commonly used structural directives are `ngIf` and `ngFor`.

## `ngIf`
`ngIf` can be used to conditionally show or hide an element:
```html
<p *ngIf="show">Text</p>
<button (click)="show=!show">Toggle text<button>
```
```js
show: bool = false;
```
Alternatively, `@if` can be used around the element(s) to show or hide:
```html
@if(show) {
    <p>Text</p>
}
<button (click)="show=!show">Toggle text<button>
```

---
## `ngFor`
`ngFor` can be used to iterate over a collection and display an element for each of them. `$index` can be used to access the index of the current iteration:
```html
<p *ngFor="let t in texts; let i = $index">{{ i }}: {{ t }}</p>
```
```js
texts = [ "a", "b", "c" ]
```
Alternatively, `@for` can be used around the element(s) to repeat. `track` must be added here to let Angular efficiently track and update the different items. Conextual variables such as `$index` are automatically available:
```html
@for(let t in texts; track $index) {    
    <p>{{ $index }}: {{ t }}</p>
}
```