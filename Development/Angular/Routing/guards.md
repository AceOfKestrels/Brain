# Guards
Guards are used to protect Routes. They can be used to check if a user should have access to a certain route or not.
Applying a guard to a route will also apply it to all its children.

## Applying a Guard to a route
To apply a guard, simply pass a guard function to a route's `canActivate` or `canDeactivate` properties. This takes an array of any gaurds that should be applied to this route:
```js
// Guard Function
const authGuard = () => {
    return false;
};

const routes: Routes = [
    { path: "", component: HomeComponent},
    { path: "users", component: UserComponent, canActivate: [authGuard] }
];
```
`/users` can only be accessed if the `authGuard` function returns true (which it won't in this example since it always returns false).

---
## Guarding Children
The `canActivateChild` property can be used in the same way as `canActivate` to protect *only* the child routes of the route it is applied it:
```js
// Guard Function
const authGuard = () => {
    return false;
};

const routes: Routes = [
    { path: "", component: HomeComponent},
    { path: "users/:id", component: UserComponent, 
        canActivateChild: [authGuard],
        children: [
            { path: "details", component: UserDetailComponent },
            { path: "edit", component: UserEditComponent }
        ]
    },
];
```
This guard will protect `/users/:id/details` and `/users/:id/details`, but *not* `/users/:id/.`

---
## See also
- [Child Routes](child-routing.md)