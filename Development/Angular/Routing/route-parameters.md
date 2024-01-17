# Route Parameters

## Passing Parameters to a Route
We can declare part of a route path to  be a parameter by adding a colon `:` in front of it:
```js
const routes: Routes = [
    { path: "users/:id", component: UserComponent}
];
```
This example will make `id` available as a parameter.

---
## Accessing Parameters
First we need to inject the currently loaded route into our component's constructor:
```js
constructor( private currentRoute: ActivatedRoute ) {}
```
Then we can access it using the `snapshot.params` property on that object:
```js
userId: string;

ngOnInit() {
    this.userId = this.currentRoute.snapshot.params["id"];
}
```

---
## Updating Parameter Values
Sometimes parameter values might change without the component being reinitialized. We can access the changed values whenever they are updated by subscribing to the `params` observable of the route:
```js
userId: string;

ngOnInit() {
    this.currentRoute.params.subscribe(
        (updatedParams) =>  {
            this.userId = updatedParams["id"];
        }
    );
}
```
The function passed here will *not* be called when the component is first initialized, so the snapshot should be used to set the values initially.

---
## Query Parameters And Fragments
Query Parameters (`?someParam=value`) and Fragments (`#something`) can be retrieved in a similar way to route parameters. 
To do this, we can use the `snapshot.queryParams` and `snapshot.fragment` properties respectively, or subscribe to the `queryParams` and `fragment` observables to react when they are updated:

Query Parameters:
```js
constructor( private currentRoute: ActivatedRoute ) {}
listAllUsers: boolean;

ngOnInit() {
    this.listAllUsers = this.currentRoute.snapshot.queryParams["listAll"] === "true";

    this.currentRoute.queryParams.subscribe(
        (updatedParams) =>  {
            this.listAllUsers = updatedParams["listAll"] === "true";
        }
    );
}
```
Fragments:
```js
constructor( private currentRoute: ActivatedRoute ) {}
currentPageSection: string;

ngOnInit() {
    this.currentPageSection = this.currentRoute.snapshot.fragment;

    this.currentRoute.fragment.subscribe(
        (updatedFragment) =>  {
            this.currentPageSection = updatedFragment;
        }
    );
}
```