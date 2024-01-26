# Components
Angular works by dynamically loading so-called components into the `index.html` file that is displayed in the browser. That means our application is made up of these components and they are the key to create single-page applications.

By default, the `app-root` component is loaded. From there we add other HTML elements as well as our own components to build our application.

## Creating a Component
Since each component is made up of multiple files, it is a good idea to create separate folders for each component you create.
The naming convention for all files related to a component is `[name].component`. These always include a TypeScript (.ts) file, generally an HTML and sometimes a css file. 

A component is just a TypeScript class with the `@Component` decorator. The naming convention for component classes is `[Name]Component`.
The `@Component` decorator is required to makr the class as a component and is also used for its configuration. An object containing several properties is passed to it with these configuration options:
```js
@Component({
    selector: "app-home",
    template: `
        <h2>This is my home component!</h2>
    `
})
export class HomeComponent {}
```
In the class body we can add properties and methods to define the logic and behavior of our component.
The `selector` property will be the name of the HTML element we use to insert this component. To make sure we are not overwriting a default HTML element a common naming convention is `app-[name]`.
The `template` property defines the HTML template used for this component. Alternatively, the `templateUrl` property can be used to point to a separate HTML file:
```js
@Component({
    selector: "app-home",
    templateUrl: "./home.component.html"
})
export class HomeComponent {}
```

&nbsp;
To be able to use a component we created, we also have to declare it in a module. By default this should be the `AppModule` (see [Modules](./modules.md) for more information). Simply add the class to the `declarations` array:
```js
@NgModule({
    declarations: [HomeComponent]
    // ...
})
export class AppModule {}
```

&nbsp;
After this is done we can use our component just like any regular HTML element:
```html
<h1>This is the app root!</h1>
<hr>
<app-home></app-home>
```

&nbsp;
As a shortcut for all of this you can use the `ng generate component` (or `ng g c`) command:
```
ng generate component [name]
```

### Styles
By using the `styleUrls` property we can point to one (or multiple) CSS file(s) that will be applied to the template of this component only:
```js
@Component({
    selector: "app-home",
    templateUrl: "./home.component.html",
    styleUrls: [ "./home.component.css" ]
})
export class HomeComponent {}
```

### Alternative Component Selectors
The selector of a component doesn't have to be the element-style selector we used. Angular also supports components to be selected either as attributes or CSS classes.

Attribute-Style selector:
```js
@Component({
    selector: "[appHome]",
    templateUrl: "./home.component.html"
})
export class HomeComponent {}
```
Can be used as an attribute:
```html
<div appHome></div>
```

&nbsp;
Class-Style selector:
```js
@Component({
    selector: ".appHome",
    templateUrl: "./home.component.html"
})
export class HomeComponent {}
```
Can be used as an attribute:
```html
<div class="appHome"></div>
```

&nbsp;
Generally we use the element style for components, however other features such as directives might use a different style of selector.