# Lifecycle Hooks
Lifecycle Hooks are special methods on any component that are called at certain points during its lifecycle.

## Implementing Lifecycle Hooks
There are several lifecycle hooks available. Almost all of them work the same way.
First we should implement an interface that represents that hook:
```js
@Component({
    // ...
})
export class HomeComponent implements OnInit {}
```
The names of these are convered in [Component Lifecycle](#component-lifecycle)

Then we implement the respective method:
```js
@Component({
    // ...
})
export class HomeComponent implements OnInit {

    ngOnInit() {
        console.log("Home Component initialized!");
    }

}
```

The exception to this is the `ngOnChanges` hook, which also receives an argument of type `SimpleChanges`:
```js
@Component({
    // ...
})
export class HomeComponent implements OnChanges {

    ngOnChanges(changes: SimpleChanges) {
        console.log(changes);
    }

}
```

## Component Lifecycle
The available Lifecycle Hooks are the following:

### OnChanges
`OnChanges` is called right when a component is created, as well as any time a bound input property changes.

### OnInit
`OnInit` is called once the component is initialized. This happens after the constructor has been called and the input properties have received their values.

### DoCheck
`DoCheck` is called every time change detection runs. This happens for example when a property value changes or an event is emitted.

### AfterContentInit
`AfterContentInit` is called after the view of any [projected content](./databinding.md#content-projection) has been initialized. 

### AfterContentChecked
`AfterContentChecked` is called after every time change detection has finished on projected content.

### AfterViewInit
`AfterViewInit` is called after the current component has been rendered into the DOM.

### AfterViewChecked
`AfterViewChecked` is called after every time change detection has finished on the current component.

### OnDestroy
`OnDestroy` is called right before it is being destroyed.