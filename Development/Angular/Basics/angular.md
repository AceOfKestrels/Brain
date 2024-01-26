# Angular

## What is Angular?
Angular is a JavaScript framework that allows for the creation of single-page web application.

---
## Creating an Angular application


---
## Using environment variables
In the `src/environment` folder, you can create two files, `environment.ts` and `environment.prod.ts` export a constant `environment`. These contain enironment variables in the form of key-value-pairs for development and production mode respectively. You can add your own variables here and use them throughout your application simply by importing them:
```js
export const environment = {
    production = false,
    apiKey = "abcde"
}
```
```js
import { environment } from "../environment/environment"
```
Angular will automatically switch out the `environment.ts` file for `environment.prod.ts` when deploying in production mode.

---
## Deployment
Before deploying your application, make sure that it is tested properly and optimized. Then use the `ng build` command to build your application. This will compile all templates and TypeScript into JavaScript. The steps to deploy the application to a web server will depend on the environment used.

---
## Read on
- [Databinding](./databinding.md)
- [Lifecycle Hooks](./lifecycle-hooks.md)
- [Observables](./observables.md)
- [Pipes](./pipes.md)
- [Routing](../Routing/index.md)
- [HTTP Requests](../HTTP%20Requests/index.md)
- [Forms](../Forms/index.md)
- [Authentication](./authentication.md)
- [Modules](./modules.md)
- [Standalone Components](./standalone-components.md)

### Todo
- Databinding
- Lifecycle Hooks
- Services
- Directives
- Components?