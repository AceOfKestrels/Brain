# Observables
Observables can be thought of as *data sources*. They emit data packages in the form of events to their observers (subscribers).
The three types of data packages observables can emit are:
- Data
- Error
- Completion

Whem receiving data packages, observers can execute code with their contents.

Observables are generally used to handle ansynchronous tasks.

## Subscribing to an Observable
Observers can be added by calling the `subscribe` method on an observable. This method takes three arguments in the form of functions that are called when the observer receives a corresponding data package: 
- `Data` of the type of the observable
- An `Error` in the form of a string
- `Completion` with no arguments

```js
this.observable.subscribe(
    {
        next: (data) => console.log(data),
        error: (error) => console.log(error),
        complete: () => console.log("Completed")
    }
); 
```
When only subscribing to new data, a simplified syntax can be used:
```js
this.observable.subscribe( data => console.log(data) ); 
```

If an observable returns an error, it completes *without* calling the completion function.

### Operators
Operators can be used to transform the data received through an observable. To add an operator to an observable, call the `pipe` method on it. The `pipe` method takes one or more operators as arguments and returns an observable that returns the transformed data:
```js
let pipe = this.observable.pipe( 
    map( (data: number) => {
        return data + 1;
    } 
));
```
In this example we use the `map` operator to transform the data (which is a number) by adding 1 to whatever value it originally had.

Now, you can subscribe to the new observable:
```js
pipe.subscribe( data => { console.log(data) } );
```

As mentioned, it is possible to pass multiple operators to `pipe`:
```js
let pipe = this.observable.pipe( 
    filter( (data: number) => {
        return data % 2 === 0;
    } ),
    map( (data: number) => {
        return data * 5;
    } )
);
```
In this example, we only pass on even numbers and multiply them by five.

### Unsubscribing
When a subscription is no longer needed (for example when the component it is in gets disposed), the subscription should also be ended. Otherwise an undisposed subscription can cause memory leaks. To unsubscribe simply call the `unsubscribe` method on the subscription.
```js
subscription: Subscription;

ngOnInit() { // Saving the subscription to a variable
    this.subscription = this.observable.subscribe( data => {} ); 
}

ngOnDestroy() { // Unsubscribing when the component is destroyed
    this.subscription.unsubscribe();
}
```
When an observable completes or returns an error it doesn't need to be unsubscribed from.

## Creating an Observable
A new observable can simply be created using the `new` keyword. Its constructor takes a function defining the observable's behavior:
```js
new Observable( observer => {
    let count = 0;
    setInterval( () => {
        observer.next(count);
        if(this.error) {
            observer.error("An error occurred");
        }
        if(this.complete) {
            observer.complete();
        }
        count++;
    }, 1000);
});
```
This will create a new observable that counts up every second and returns the time it has been running. If the `error` property is true it returns an error, and if the `complete` property is true it completes.

## Subjects
Subjects are a special kind of observable. While observables wrap another event source (such as the `setInterval` in the previous example), subjects can be triggered from the outside by calling `next` on them:
```js
onButtonClicked = new Subject();

buttonClicked() {
    this.onButtonClicked.next();
}

throwError() {
    this.onButtonClicked.error("Something went wrong");
}

ngOnInit() {
    this.onButtonClick.subscribe(
        {
            next: () => console.log("Button clicked!"),
            error: (error) => console.log(error)
        }
    );
}
```

Subjects should also be unsubscribed from:
```js
subscription: Subscription;

ngOnDestroy() {
    this.subscription.unsubscribe();
}
```

### Behavior Subjects
Behavior Subjects are a special type of subject that also give new subscribers access to the previous value emitted. Unlike regular `Subject` it must be initialized with a starting value (that may be null):
```js
ngOnInit() {
    const subject = new BehaviorSubjecct(0);
    subject.subscribe( value => console.log(value) )
}
```
On subscription, current value will be emitted as if `next` was called.
 
## RxJS
Observables are part of the `RxJS` package. If it is not installed already, use the following command to install it:
```
npn install --save  rxjs
```
Also install the `RxJS-compat` package:
```
npn install --save  rxjs-compat
```