# Standalone Components
Normally, all components we want to use have to be declared in a module. Standalone components can be used without doing that.

## Creating Standalone Components
Simply add the `standalone` property to the component and set it to `true`:
```js
@Component({
    standalone: true,
    selector: "app-user-list",
    templateUrl: "./userList.component.html"
})
export class UserListComponent { }
```

---
## Using Standalone Components
In any component you want to use this in, add an `imports` property containing the standalone component, and make it standalone as well:
```js
@Component({
    imports: [UserListComponent],
    standalone: true,
    selector: "app-home",
    templateUrl: "./home.component.html"
})
export class HomeComponent { }
```

While migrating to standalone components, they can also be imported in a module and then be used by non-standalone components. During the migration process we get the worst of both worlds, having to write more code than before.

Similarly, to use non-standalone components or directives in standalone ones, they must also be imported.

---
## Standalone Root Component
The root component `AppComponent` is special. To make it standalone, we also have to change our bootstrapping in `main.ts` code to  use a component instead of a module to start the app:
```js
// Old bootstrapping code
// platformBrowserDynamic().bootstrapModule(AppModule)
//    .catch(err => console.log(err));

bootstrapApplication(AppComponent);
```

### Routing with Standalone Compontents
For routing to work with standalone components, two things need to be done.
First, the routing module needs to be imported in the `AppComponent`:
```js
@Component({
    imports: [HomeComponent, AppRoutingModule],
    standalone: true,
    selector: "app-root",
    templateUrl: "./app.component.html"
})
export class AppComponent { }
```
Then, it also needs to be imported in the bootstrap code providers array:
```js
bootstrapApplication(AppComponent, {
    providers: [ importProvidersFrom(AppRoutingModule) ]
});
```

Any conponent using routing features, such as `router-outlet` or `routerLink` must also import the routing module.

The `AppModule` is now longer required at this point.

---
## Lazy Loading with Standalone Components
Lazy Loading for standalone components can simply be enabled by using the `loadComponent` property and passing a loading function to it similarly to the `loadChildren` function (see [Lazy Loading](../Modules/lazy-loading.md) for more details):
```js
const routes: Route[] = [
    { 
        path: "", 
        component: HomeComponent
    },
    { 
        path: "users",  
        loadComponent: () => import("../components/users/userList.component")
                                .then((mod) => mod.UserListComponent)
    }
];
```
This allows us to load individual components lazily without having to wrap them into a separate module.

### Lazy Loading multiple Standalone Components
We can use `loadChildren` to lazy load multiple child routes together:
```js
const routes: Route[] = [
    { 
        path: "", 
        component: HomeComponent
    },
    { 
        path: "users",  
        loadChildren: () => import("./user-routing").then((mod) => mod.userRoutes)
    }
];
```
```js
const usersRoutes: Route[] = [
    { path: "", component: UserListComponent },
    { path: "user/:id", component: UserDetailComponent }
]
```

---
## Standalone Directives and Pipes
Directives and Pipes can be made standalone in the same way:
```js
@Directive({
    standalone: true,
    selector: "[clickoff]"
}) 
export class ClickoffDirective {  }
```
```js
@Pipe({
    standalone: true,
    name: "shorten"
})
export class ShortenPipe { }
```
