# Query Parameters and Fragments
Query Parameters (`?someParam=value`) and Fragments (`#something`) can be passed via router links or programmatic navigation, and accessed by components.

## In Router Links
Query Parameters and Fragments can be passed in addition to the path using the `queryParams` and `fragment` properties respectively.
`queryParams` takes an object of key-value-pairs defining the parameters and their values. 
Since only one fragmet may be passed, `fragment` is simply set to its value. Property binding is *not* required for fragments.
```html
<a 
    routerLink="/users"
    [queryParams]="{ listAll: true }"
    fragment="userList"
>List All Users</a>
```
This link will load `[domain]/users?listAll=true#userList`.

---
## Through Programmatic Navigation
Query Parameters and Fragments can be passed in addition to the path using the `queryParams` and `fragment` properties respectively.
`queryParams` takes an object of key-value-pairs defining the parameters and their values. 
Since only one fragmet may be passed, `fragment` is simply set to its value.
```js
listAllUsers() {
    this.router.navigate( 
        ["/users"], 
        { 
            queryParams: { listAll: true },
            fragment: "userList"
        } 
    );
}
```
This function will navigate to `[domain]/users?listAll=true#userList`.

---
## Preserving Query Parameters and Fragments
### Query Parameters
To preserve query parameters, set the `queryParamsHandling` property to `"preserve"`:
```html
<a routerLink="/users/edit" queryParamsHandling="preserve">Edit User</a>
```
```js
editUser() {
    this.router.navigate(
        ["/users/edit"],
        {
            queryParamsHandling: "preserve"
        }
    )
}
```

### Fragments
To preserve query parameters, set the `preserveFragment` property to `true`:
```html
<a routerLink="/users" preserveFragment="true">Users</a>
```
```js
editUser() {
    this.router.navigate(
        ["/users"],
        {
            preserveFragment: true
        }
    )
}
```

---
## Retrieving Query Parameters and Fragments
Query Parameters and Fragments can be retrieved in a similar way to [route parameters](route-parameters.md). 
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

---
## See Also
- [Router Links](basic-linking.md)
- [Navigating Programmatically](navigating-programmatically.md)
- [Route Parameters](route-parameters.md)