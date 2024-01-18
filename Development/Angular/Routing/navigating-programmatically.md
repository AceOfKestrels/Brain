# Navigating Programmatically
Instead of clicking on a link we can also change the route through TypeScript code. This might be used in a variety of scenarios.
This can be done using the `navigate` function on the Router class.

The Router is passed as an argument to the component's constructor:
```js
constructor( private router: Router ) {}
```
Then we can call `navigate` on it, passing an array of path segments, identically to [Router Links](basic-linking.md):
```js
loadUsersPage() {
    router.navigate( ["/users"] );
}
```
In there we can also reference variables and constants of course:
```js
const currentUserName: string = "AceOfKestrels";

loadUsersPage() {
    router.navigate( ["/users", this.currentUserName] );
}
```

---
## Relative Paths
The `navigate` function doesn't "know" which path we are currently on, so to use relative paths we have to also pass the current path. 

The active route can be passed to the constructor as an `ActivatedRoute` object:
```js
constructor( 
    private router: Router, 
    private currentRoute: ActivatedRoute 
) {}
```
This among other things can be passed as part of a `NavigationExtras` object:
```js
loadUsersPage() {
    this.router.navigate( 
        ["users"], 
        { relativeTo: this.currentRoute } 
    );
}
```
By default the `relativeTo` property is set to the root path.

## Query Parameters and Fragments
See [Query Parameters and Fragments](query-parameters-fragments.md#through-programmatic-navigation)