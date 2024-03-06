# Child Routing
We can add nested (child) routes to existing ones, which all start with the same parent path. The corresponding components can get loaded by a different `router-outlet`.

## Setting up Child Routes
In the `Routes` arrays, add the `children` property to an existing route which ou want to add nested routes to and pass another array of routes:
```js
const routes: Routes = [
    { path: "users/list", component: UserListComponent },
    { path: "users/:id", component: UserComponent, 
        children: [
            { path: "details", component: UserDetailComponent },
            { path: "edit", component: UserEditComponent }
        ]
    },
];
```
Then add a new `router-outlet` to the parent component:
```html
<p>User ID: {{ userId }}</p>
<router-outlet></router-outlet>
```
In this example `users/:id` loads the `UsersComponent`, which can load two different children depending on the path that follows.