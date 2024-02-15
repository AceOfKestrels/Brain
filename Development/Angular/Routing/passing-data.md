# Passing Data to Routes
We can pass data to routes, either statically (always the same data) or dynamically.

## Passing Static Data
To pass static data we can use the `data` property of the route. This takes an object containing all the data:
```js
const routes: Routes = [
    { path: "", component: HomeComponent},
    { path: "not-found", component: ErrorComponent, 
        data: { 
            message: "Page not found!" 
        } 
    }
];
```

## Passing Dynamic Data
Passing dynamic data to routes workes similarly to [Guards](guards.md). A map of functions that fetche the data can be passed to the `resolve` property:
```js
// Resolver Function
const getUser = () => {
    return false;
};

const routes: Routes = [
    { path: "", component: HomeComponent},
    { path: "users", component: UserComponent, 
        resolve: { 
            user: getUser 
        } 
    }
];
```

## Retrieving Data
Data can be retrievt similarly to [Route Parameters](route-parameters.md). To do this, we can use the `snapshot.data` property or subscribe to the `data` observable to react when it is updated:
```js
constructor( private currentRoute: ActivatedRoute ) {}
errorMessage: string;

ngOnInit() {
    this.listAllUsers = this.currentRoute.snapshot.data["message"];

    this.currentRoute.data.subscribe(
        (updatedData) =>  {
            this.errorMessage = updatedParams["message"];
        }
    );
}
```