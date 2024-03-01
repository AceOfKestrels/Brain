# Routing

## What is Routing?
Routing describes the process of simulating different URLs to your client browser while staying on the same page, by loading different components depending on the URL entered.

We use Angular to build single-page-applications. Loading different pages would go against that goal.

## Defining Routes
Add a `RouterModule` to the `imports` section of the app module and call the `forRoot` method: 
```js
@NgModule({
    imports: [ RouterModule.forRoot(routes) ]
    
    // other module properties
})
```
This method takes a `Routes` array containing the defined routes.

Then add a router outlet to the place where the component should get displayed, such as your main app component:
```html
<router-outlet></router-outlet>
```

### Setting up the Routes array
Create a property of type `Routes`, which is an array of Route objects:
```js
const routes: Routes = [
    { path: "", component: HomeComponent },
    { path: "users", component: UsersComponent }
];
```
- Start with the `path` property. This is a string with the path of this route, *without* a leadings slash.
- Then add a component to display at this route using the `component` property. This is an Angular component class.

## Routing as a separate module
We can put both the `Routes` array as well as the `RouterModule` import into a separate module. Both can be removed from the app module.

This new module needs to be imported in the app module and the `RoutingModule` exported in the new one:
```js
const routes: Routes = [
    // Some routes
];

@NgModule({
    imports: [ RouterModule.forRoot(routes) ],
    exports: [ RouterModule ]
})
export class AppRoutingModule {}
```
Afterwards, this new module needs to be imported in the app module:
```js
@NgModule({
    imports: [ AppRoutingModule ]
    
    // other module properties
})
```
See [modules](../modules.md) for more information on Angular modules.

## Redirecting
We can redirect a route to a different one using the `redirectTo` property:
```js
const routes: Routes = [
    { path: "", component: HomeComponent},
    { path: "home", redirectTo: "/" }
];
```
In this example `[domain]/home` will automatically redirect to the root page.

## Path Matching
By default, route paths are matched by *prefix*. This means that a URL will match a route as long as it *starts* with the path specified:
```js
const routes: Routes = [
    { path: "users", component: UsersComponent },
    { path: "", component: HomeComponent }
];
```
In particular, this means the root path will always match any other path. This can be avoided using the `pathMatch: "full"` property:
```js
const routes: Routes = [
    { path: "users", component: UsersComponent },
    { path: "", component: HomeComponent, pathMatch: "full" }
];
```

### Wildcards
Using the wildcard `**` we can match any path we haven't defined:
```ts
const routes: Routes = [
    { path: "", component: HomeComponent },
    { path: "not-found", component: NotFoundComponent },
    { path: "**", redirectTo: "/not-found" }
];
```
In this example any path that is not the root path will redirect to `[domain]/not-found`

## Example `app-routing.module.ts`
```js
const routes: Routes = [
    { path: "home", component: HomeComponent },
    { path: "", component: HomeComponent, pathMatch: "full" },
    { path: "users", component: UsersComponent, children: 
        [
            {path: ":id", component: UserDetailComponent },
            {path: ":id/edit", component: UserEditComponent }
        ] 
    },
    { path: "not-found", component: NotFoundComponent },
    { path: '**', redirectTo: "/not-found" }
];

@NgModule({
    imports: [ RouterModule.forRoot(routes) ],
    exports: [ RouterModule ]
})
export class AppRoutingModule {}
```

## Read on
- [Location Strategies](location-strategy.md)
- [Linking to Routes](basic-linking.md)
- [Navigating Programmatically](navigating-programmatically.md)
- [Child/Nested Routes](child-routing.md)
- [Route Guards](guards.md)
- [Route Parameters](route-parameters.md)
- [Query Parameters and Fragments](query-parameters-fragments.md)
- [Passing Data](passing-data.md)
- [Lazy Loading](../modules.md#lazy-loading)