# Angular

## What is Angular?
Angular is a TypeScript framework that allows for the creation of single-page web applications.

## Getting Started
To create a new Angular project, we use the Angular CLI, an offcial tool made by the Angular team. It can be installed using the `npm` packet manager:
```
npm install -g @angular/cli
```

NPM requires Node JS, so if you don't have that installed yet, download it from the [official website](https://nodejs.org/en) and install it first.

With the CLI installed you can open a command prompt or terminal in the folder you want your project to be located in and use the `ng new` command:
```
ng new [project name] --no-strict --standalone false --ssr
```
The `--no-strict` flag will make our lives a bit easier, but strict mode is required for some additional optimizations.

The `--standalone false` flag creates the project with an app module and non-standalone app component. Remove it if you wish to only use [standalone components](./standalone-components.md).

The `--ssr` configures the application to use [server side rendering](./server-side-rendering.md) for the first view of the page.

After the project has been create, use the `ng serve` command in the newly created folder to start up the project in development mode.


## Using environment variables
In the `src/environment` folder, you can create two files, `environment.ts` and `environment.prod.ts` export a constant `environment`. These contain enironment variables in the form of key-value-pairs for development and production mode respectively. You can add your own variables here and use them throughout your application simply by importing them:
```js
export const environment = {
    production: false,
    apiKey: "abcde"
}
```
```js
import { environment } from "../environment/environment"
```
Angular will automatically switch out the `environment.ts` file for `environment.prod.ts` when deploying in production mode.

## Deployment
Before deploying your application, make sure that it is tested properly and optimized. Then use the `ng build` command to build your application. This will compile all templates and TypeScript into JavaScript. The steps to deploy the application to a web server will depend on the environment used.

## Read on
- [Components](./components.md)
- [Databinding](./databinding.md)
- [Lifecycle Hooks](./lifecycle-hooks.md)
- [Directives](./Directives/directives.md)
- [Services](./services.md)
- [Observables](./observables.md)
- [Animations](./animations.md)
- [Pipes](./pipes.md)
- [Routing](./Routing/routing.md)
- [HTTP Requests](./http-requests.md)
    - [Authentication](./authentication.md)
- [Forms](./Forms/forms.md)
- [Modules](./modules.md)
- [Server Side Rendering](./server-side-rendering.md)
- [Service Workers](./service-workers.md)
- [Standalone Components](./standalone-components.md)
- [Signals](./signals.md)