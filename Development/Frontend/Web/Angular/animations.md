# Animations
Since we are constantly adding and removing elements from the DOM when working with Angular, it is difficult to build animations with CSS transitions alone. Angular offers its own animation framework however.

## Getting Started
First we need to install the `@angular/animations` package:
```
npm install --save @angular/animations
```
And import the `BrowserAnimationsModule` in our module:
```js
imports: [ BrowserAnimationsModule ]
```

&nbsp;
Then we can add animations to a component using the `animations` property:
```js
@Component({
    selector: "app-home",
    templateUrl: "./appHome.component.html",
    animations: []
})
```
This is an array of animation objects defining the animations we want to apply.

There we add `trigger` functions, which will define the animation. 
The first argument is an identifier that we will add to an element. The second one is an array containing the animation metadata.
```js
animations: [
    trigger("divAnim", [])
]
```

## States
Animations use "states" to determine the stlye an element should be displayed in. We bind the name of the state to the trigger name we defined earlier:
```html
<div style="width: 100px; height: 100px" [@divAnim]="state"></div>
```
```js
state: string = "default";
```

&nbsp;
We can then add different styles and other properties to the element depending on its state:
```js
animations: [
    trigger("divAnim", [
        state("default", style({
            backgroundColor: "transparent",
            transform: "translateX(0)"
        })),
        state("red", style({
            backgroundColor: "red",
            transform: "translateX(100)"
        }))
    ])
]
```
Now, when changing the `state` property accordingly, the styles with switch between the two.

## Transitions
Using the `transition` method we can then create an animation between the two states.
As a first argument this method takes the states it should play between. In the second argument it is defined how the animation should look like. This can use intermediate style, but by default takes the animation duration in ms:
```js
animations: [
    trigger("divAnim", [
        // ...
        transition("default => red", animate(500))
    ])
]
```
Use `<=>` to make the same animation play both ways, and `*` as a wildcard for any other state:
```js
animations: [
    trigger("divAnim", [
        // ...
        transition("default <=> *", animate(500))
    ])
]
```

&nbsp;
We can also pass multiple animations to a transition that will play after one another, as well as styles to apply:
```js
animations: [
    trigger("divAnim", [
        // ...
        transition("default <=> *", [
            style({
                backgroundColor: "orange"
            }),
            animate(400, styles({ backgroundColor: "pink" })),
            animate(100)
        ])
    ])
]
```

## The `void` State
The `void` state can be used to add animations when an element is being removed or added:
```js
animations: [
    trigger("divAnim", [
        state("loaded", styles({ backgroundColor: "red" })),
        transition("* <=> void", [
            style({ backgroundColor: "transparent" }),
            animate(500)
        ])
    ])
]
```

## Callbacks
We can listen to different events that get called when an animation starts or finishes:
```html
<div 
    style="width: 100px; height: 100px" 
    [@divAnim]="state"
    (@divAnim.start)="onAnimationStart()"
    (@divAnim.done)="onAnimationDone()"
></div>
```