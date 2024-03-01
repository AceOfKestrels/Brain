# Modules
Modules in Angular are used to bundle different building blocks (components, directives, etc.) together. Building blocks defined in one module will then not be available in another module. That means, if we split off building blocks into other modules and any of these modules use features not part of the angular core, the respective modules must be imported there again.

Some core Angular features are wrapped together into built-in modules, such as `FormsModule` to laod them only when needed.

## Feature Modules
It might be a good idea to split off independent features into separate modules. This declutters the main `AppModule`. To use feature modules, they must be imported by whichever module they are used in and export all the components that are used elsewhere:
```js
@NgModule({
    declarations: [HomeComponent] ,
    imports: [
        BrowserModule,
        AppRoutingModule,
        UsersModule
    ]
})
export class AppModule {}
```
```js
@NgModule({
    declarations: [ UserListComponent, UserDetailComponent, UserEditComponent ],
    imports: [
        // Import CommonModule instead of BrowserModule in non-root components. 
        // This is a special case
        CommonModule,
        RouterModule
    ],
    exports: [ UserListComponent, UserDetailComponent ]
})
export class UsersModule {}
```

### Routing in Feature Modules
It is a good idea to also move the routes related to a feature module into another module. To do this we use the `forChild` method:
```js
const routes: Routes = [
    { path: "users", component: UserListComponent },
    { path: "users/user/:id", component: UserDetailComponent }
];

@NgModule({
    declarations: [ UserListComponent, UserDetailComponent, UserEditComponent ],
    imports: [
        CommonModule,
        RouterModule.forChild(routes)
    ]
})
export class UsersModule {}
```
The `UserListComponent` and `UserDetailComponent` exports were removed here because theye are no longer used in the `AppRoutingModule`.

## Shared Modules
Sometimes features are used in multiple modules. To avoid code duplication it can help to bundle them together in a shared module that is then imported by the other ones. To make the building blocks defined in that module available in other modules they need to be exported:
```js
@NgModule({
    declarations: [
        CalendarComponent,
        CalendarItemComponent,
        ListComponent,
        ClickoffDirective
    ],
    exports: [
        CalendarComponent,
        ListComponent,
        ClickoffDirective
    ]
})
export class SharedModule {}
```
Shared modules are especially useful if you want to use a component in different modules because building blocks can only be *declared* once, but a module can be *imported* into any other module.

## Lazy Loading
Lazy Loading allows us to download components only when we actually access their respective route. This is an important feature to optimize loading times of our application. Any routes and components that should be loaded lazily need to be separated into a [feature module](#feature-modules). 

To implement Lazy Loading, we first change the route paths defined in the module we want to use and make the look like child routes:
```js
const routes: Routes = [
    { path: "", component: UserListComponent },
    { path: "/user/:id", component: UserDetailComponent }
];
```
Then, we use the `loadChild` property in our root routing module to load them lazily:
```js
const routes: Routes = [
    { path: "", component: HomeComponent },
    { 
        path: "users", 
        loadChildren: () => import("./users/users.module")
                                .then((mod) => mod.UsersModule)
    }
];
```
Finally remove the import to the lazily loaded module from the `imports` array:
```js
@NgModule({
    declarations: [HomeComponent] ,
    imports: [
        BrowserModule,
        AppRoutingModule
        // UsersModule - No longer loaded eagerly
    ]
})
export class AppModule {}
```
Also remember to remove any unused imports.

### Preloading
Instead of lazy loading modules only when we access the route associated with them, we can preload them while the user is idling on another route path. This can be enabled by passing a configuration object with the `preloadigStrategy` property to the `forRoot` method:
```js
@NgModule({
    imports: [ 
        RouterModule.forRoot(routes, { preloadingStrategy: PreloadAllModules }) 
    ],
    exports: [ RouterModule ]
})
export class AppRoutingModule {}
```

### Services and Lazy Loading
Services are special because they are available application-wide even when loaded inside a non-root module. Lazily loaded modules are an exception to this exception. Services provided there are only available inside that module. 