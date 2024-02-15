# Services
Services are used to decrease code duplication, storing and providing data, as well as a way for components to communicate more clearly.

## Creating a Service
Simply create and export a new class. The naming conventiion for the class and file are `[Name]Service` and `[name].service.ts` respectively. 
In there add all the methods and properties necessary.:
```js
export class UserService {
    users: User[] = [];

    getUser(id: string) {
        // ...
    }
}
```

## Dependency Injection
To make sure every component and class uses the same instance of a service we use [Dependency Injection](../../Concepts/dependency-injection.md). Angular will automatically inject services into the constructor of components as well as other services, as long as it knows how to create an instance of it. Simply add a parameter with the type of a service to the constructor:
```js
@Component({
    // ...
})
export class UserComponent {
    constructor(protected userService: UserService) {}
}
```
When injecting a service into another service, the `@Injectable` decorator must be added first:
```js
@Injectable
export class AdminService {
    constructor(private userService: UserService) {}
}
```

&nbsp;
There are two ways of informing Angular how to create an instance of a service:

### `providedIn` Approach
If we want a service to be available everywhere (including other services), the easiest way of achieving this is by passing the `providedIn` property with the value `"root"` to the `@Injectable` decorator of a service:
```js
@Injectable({ providedIn: "root" })
export class UserService {
    // ...
}
```

### Providers Approach
Another way to provide services is the `providers` array in any component or module:
```js
@Component({
    // ...
    providers: [ UserService ]
})
export class UserComponent {}
```
Services provided this way share the same instance between the component they're provided in and any child components of it (providing a service again in a child component will create a new instance for *that* component and its children).
To be able to use a service in another service this way, it has to be provided in the app module. When providing a service applcation-wide the previous approach is recommended.