# Basic Routing

## How to set up routing?
### Defining Routes
- Add a `RouterModule` (from `@angular/router`) to the `imports` section of the app module (`app.module.ts`).
- Add `.forChild` to this module and pass in a `Routes` array.
- This array can be defined directly here or as a `const` above the module.

The above can also be put in a [separate module](#routing-as-a-separate-module)

- Add a `<router-outlet></router-outlet>` element to the place where the component should get displayed, such as your main app component.

### Setting up the Routes array
- Create a property of type `Routes`, which is an array of Route objects
- Start with the `path` property. This is a string with the path of this route, *without* a leadings slash.
- Then add a component to display at this route using the `component` property. This is an Angular component class.

### Routing as a separate module
- TODO

### Example
```
TODO
```

## Navigating with Router Links
### TODO