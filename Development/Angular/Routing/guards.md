# Guards
Guards are used to protect Routes. They can be used to check if a user should have access to a certain route or not.

## Guard Service
The Guard is simply a regular service which implements interfaces such as `CanActivate` or `CanDeactivate`.

## `CanActivate` Guard
To use this guard we must implement the `CanActivate` interface with our guard service.
This requires us to implement the `canActivate` method, which takes an `ActivatedRouteSnapshot` and a `RouterStateSnapshot` as arguments, and returns a `boolean`, `Observable<boolean>` or `Promise<boolean>`
```js
export class AuthGuardService implements CanActivate {

    canActivate( route: ActivatedRouteSnapshot, routerState: RouterStateSnapshot ):
        boolean | Observable<boolean> | Promise<boolean> {
        return true;
    }

}
```