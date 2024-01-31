# Signals
Signals are a new way in Angular to detect changes. Using the old "Zone-based" algorithm, change detection runs automatically whenever any value might update. With Signals we call change detection manually on only the affected components, which is slightly more work but can lead to preformance improvements.

## Using Signals
`Signal` objects are wrappers around the data that will trigger change detection when the data is updated.

### Creating Signals
To initialize a signal, instead of assigning the value of a property directly, assign a `signal` function and pass the value to it:
```js
count = signal(1);
```

### Updating Signals
Signals are updated using the `set` and `update` functions.
`set` simply takes the new value:
```js
this.count.set(2);
```
While `update` takes a function that allows you to change the value based off the old one:
```js
this.count.update( old => old+1 );
```

### Accessing Signal Values
To access signal values, call the property as a function:
```html
<p>Current count is: {{ count() }}</p>
```

---
## Computed Values
We can use the `computed` function up update the value of a proeprty whenever a signal is updated:
```js
counter = signal(1);
message = computed( () => "Current count is: " + couter() );
```
It can then be used just like a signal:
```html
<p>{{ message() }}</p>
```

---
## Effects
Effects can be used to run code whenever a signal changes:
```js
counter = signal(1);

constructor() {
    effect( () => console.log(counter()) );
}
```