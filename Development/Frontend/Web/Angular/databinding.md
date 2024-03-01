# Databinding
Databinding is used for different components to communicate with each other, or for the HTML template to communicate with the TypeScript code of a component.

## String Interpolation
String Interpolation allows you to fill in a string from a field or expression. You use it by surrounding the expression by double curly brackets:
```html
<h1>Hello, {{ userName }}</h1>
```

## Property Binding
Property Binding is used to bind the value of an attribute or property to a TypeScript expression. It is used using square brackets:
```html
<button [disabled]="isDisabled" >Some Button</button>
```
```js
isDisabled: bool = false;
```

&nbsp;
### Custom Properties
We can also bind to properties on our own components. For a property to be able to be accessed this way we need to add the `@Input` decorator to it:
```js
@Component({
    // ...
})
export class HomeComponent {
    @Input()
    title: string = "";
}
```
The value assiged in our component code will be the default value if no other value is passed by the parent.
Optionally, the `@Input` decorator accepts a string as an argument, which will be the alias that will be used to bind to this property instead of the property name.

## Event Binding
Event Binding is used to react to user input or other kinds of events. It is used using regular paranthesis:
```html
<button (click)="onButtonClick()" >Some Button</button>
```
```js
onButtonClick() {
    console.log("Button Clicked!");
}
```

&nbsp;
### Custom Events
When using event binding, you can use `$event` to get access to the event object:
```html
<input type="text" (input)="onInput($event)" >
```
```js
onButtonClick(event: Event) {
    console.log(event.target.value);
}
```

&nbsp;
We can create custom events on our own components, and react to them on the parent. To do this we must create a property of type `EventEmitter` in our component and add the `@Output` decorator to it:
```js
@Component({
    // ...
})
export class HomeComponent {
    @Output()
    userAdded = new EventEmitter<User>();
}
```
`EventEmitter` is a generic type. The type we assign to it will be the type of the object accessed by `$event`. Just like `@Input` we can give `@Output` an alias it we be avaliable as instead of the property name.

&nbsp;
After creating an `EventEmitter` we can emit an event on it using the `emit` method:
```js
@Component({
    // ...
})
export class HomeComponent {
    @Output()
    userAdded = new EventEmitter<User>();

    userName: string = "";

    addUser(name: string) {
        userAdded.emit( new User(name) );
    }
}
```
```html
<input type="text" [(ngModel)]="userName">
<button (click)="addUser(userName)">Add User</button>
```
If the `EventEmitter` has a generic type that is not `void` an object of that type must be passed to `emit`.

## Two-Way-Binding
Two-way-binding can be used to bind the value of an input field to a property. When one of them changes the other is updated accordingly. 
This requires the `FormsModule` to be imported in the `AppModule`. For more information on the `FormsModule` see [Forms](../Forms/index.md).
Two-way-binding can be used using both paranthesis and square brackets, and the `ngModel` property:
```html
<input type="text" [(ngModel)]="inputValue" >
```
```js
inputValue: string = "";
```

## Local References
Local References can be used to get access to an object representing an element. I can be used with the `#` symbol:
```html
<input type="text" #nameInput >
<button (click)="addUser(nameInput.value)">Add User</button>
```
These references are only accessible in the HTML template normally. However you can use the `@ViewChild` decorator to assign them to a property of type `ElementRef`:
```js
@Component({
    // ...
})
export class HomeComponent {
    @ViewChild("nameInput")
    nameInput: ElementRef;
}
```
To access the element itself from this, use the `nativeElement` property on it.

## Content Projection
Normally anything between the opening and closing tags of a component is lost. To change this behavior, simply add a `ng-content` directive to the template:
```html
<h3>Child content:</h3>
<ng-content></ng-content>
```
While this looks like a component, `ng-content` *is* a directive, simply using the element-style selector.

Now any child content we add will be projected in the place we added the `ng-content` directive:
```html
<app-home>
    <p>Child Content!</p>
</app-home>
```