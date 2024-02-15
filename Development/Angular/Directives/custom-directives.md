# Custom Directives
As mentioned previously, creating directives is similar to components. 

## Creating new Directives
First we create a new file. The naming convention is `[name].directive.ts`. In there we export a new class with the `@Directive` decorator that is responsible for the directive's configuration:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective { }
```
Here we are creating a directive with an attribute-style slector. Similarly to components, the naming convention for this is `app[name]`. 
Also similarly to components the naming convention for the class is `[Name]Directive`.

In the constructor we can inject an `ElementRef`, which will be a reference to the element this directive is placed on:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective { 
    constructor(private element: ElementRef) {}
}
```

&nbsp;
Just like components, new directives must also be declared in a module.

And then the new directive can simply be applied to any element:
```html
<h1 appHighlight>Text with green highlighting</h1>
```

## Changing the Host Element
It is not a good idea to access the element directly. Instead we can inject a `Renderer2` to change the appearance of the parent element:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective { 
    constructor(private element: ElementRef, private renderer: Renderer2) {}
}
```

&nbsp;
Then we can use `ngOnInit` and other [lifecycle hooks](../Basics/lifecycle-hooks.md) to apply changes to the element:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective implements OnInit { 
    constructor(private element: ElementRef, private renderer: Renderer2) {}

    ngOnInit() {
        this.renderer.setStyle(this.element.nativeElement, "background-color", "green");
    }
}
```
Of course the renderer can do more than just change the style of an element.

## Listening to Events
The `@HostListener` decorator can be used to listen to events occurring on the host element:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective implements OnInit { 
    constructor(private element: ElementRef, private renderer: Renderer2) {}

    @HostListener("mouseenter")
    mouseOver(event: Event) {
        this.renderer.setStyle(this.element.nativeElement, "background-color", "green");
    }

    @HostListener("mouseleave")
    mouseLeave(event: Event) {
        this.renderer.setStyle(this.element.nativeElement, "background-color", "transparent");
    }
}
```

## Binding to Host Properties
The `@HostBinding` decorator can be used to access properties of the host element:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective implements OnInit { 
    @HostBinding("style.backgroundColor")
    backgroundColor: string = "transparent";

    @HostListener("mouseenter")
    mouseOver(event: Event) {
        this.backgroundColor = "green";
    }

    @HostListener("mouseleave")
    mouseLeave(event: Event) {
        this.backgroundColor = "transparent";
    }
}
```

## Binding to Directive Properties
Similarly to [property binding on components](../Basics/databinding.md#property-binding) we use `@Input` to declare a property to be accessible from the outside:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective implements OnInit { 
    @Input()
    color: string = "transparent";

    // ...
}
```
&nbsp;
Then we can bind to this property like normal after we have added the directive itself:
```html
<h1 appHighlight [color]="'green'">Text with green highlighting</h1>
```
We can also use the alias feature of `@Input` to change the name of the property we bind to. If we change it to the name of the directive itself, we can add the directive to an element and bind to the respective property in one step:
```js
@Directive({
    selector: "[appHighlight]"
})
export class HighlightDirective implements OnInit { 
    @Input("appHighlight")
    color: string = "transparent";

    // ...
}
```
```html
<h1 [appHighlight]="'green'">Text with green highlighting</h1>
```
We saw this when using [`ngClass` or `ngStyle`](./attribute-directives.md).

## Creating Structural Directives
Using `TemplateRef` and `ViewContainerRef` we can add or remove elements in directives.
First we want to inject those in the constructor:
```js
@Directive({
    selector: "[appUnless]"
})
export class UnlessDirective { 
    constructor(private template: TemplateRef, private view: ViewContainerRef) {}
}
```

&nbsp;
Then we can use methods such as `clear` and `createEmbeddedView` to remove or add elements to the view container:
```js
@Directive({
    selector: "[appUnless]"
})
export class UnlessDirective { 
    constructor(private template: TemplateRef, private view: ViewContainerRef) {}

    @Input("appUnless")
    set unless(condition : boolean) {
        condition
            ? this.view.clear();
            : this.view.createEmbeddedView(this.template)
    }
}
```

&nbsp;
When using custom structural directives, remember to use a `*` in front:
```html
<p *[appUnless]="condition">Condition is false</p>
```