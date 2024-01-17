# Linking to Routes

## Navigating with Router Links

### Why not just use `href="..."`?
Using a regular link using `href`, clikcing it will send a new request to the server and reload the entire page. This defeats the point of building a single page application.

### How to do it correctly
Instead of the `href` attribute we use a special `routerLink` directive. The syntax is otherwise the same: 
```html
<a routerLink="/users/AceOfKestrels" >Users</a>
```
Alternatively, Property Binding can be used to bind an array of route segments to the `routerLink` property:
```html
<a [routerLink]="['/users', 'AceOfKestrels']" ></a>
```
This is useful for some more advanced use cases.

---
## Relative vs Absolute Paths in Router Links

### Absolute Paths
Using a leading slash in the router link (`"/users"` in the above example) indicates an **absolute path**, meaning this route's path is `[domain]/users`. (see [Absolute Path Example](#absolute-path-example))

### Relative Paths
If we wish to link to a route relavtive to the current route's path we can omit the leading slash:
`"users"` would lead to `[domain]/users` when clicked on a root component, but if the component is loaded on say `[domain]/home`, its target would be `[domain]/home/users`.

Importantly, it is the *component's* path that is important, *not* the URL we see in the adress bar! (see [ Relative Path Example 1](#relative-paths-example-1))

### Directory-Style Navigation in Relative Paths
Using `./` or `../` in a router link we can navigate similarly to a direcctory structure. 
- `./` is equivalent to leaving out the leading slash: `"./users" == "users"`
- Multiple `../`'s can be chained to navigate up multiple levels: `"../../users"`

Note that using `../` to navigate up one level will not just remove one segment of the path, but the entire path of the component the link is in. (see [Relative Path Example 2](#relative-paths-example-2))

---
## Router Link Presentation
The `routerLinkActive` directive can be used to add css classes to an element with a Router Link, if the link's path is currently active:
```html
<button 
    class="btn" routerLink="/users" 
    routerLinkActive="btn-success" 
>Users</button>
```
This will add the `btn-success` class to the button if the the currently loaded path is `[basePath]/users`.

### Active Link Matching
Specifically, `routerLinkActive` checks if the current path *contains* the link's path. For example, when `[domain]/home/users` is currently loaded, `[domain]/home` is also considered active. 
As a consquence, the root path `[domain]` is always considered active.
This behavior can be changed by binding to the `routerLinkActiveOptions` property:
```html
<button 
    class="btn" routerLink="/" 
    routerLinkActive="btn-success" 
    [routerLinkActiveOptions]="{ exact: true }"
>Users</button>
```

---
## Passing Query Parameters and Fragmets
Query Parameters (`?someParam=value`) and Fragments (`#something`) can be passed in addition to the path using the `queryParams` and `fragment` properties respectively.
`queryParams` takes an object of key-value-pairs defining the parameters and their values. 
Since only one fragmet may be passed, `fragment` is simply set to its value. Property binding is *not* required for fragments.
```html
<a 
    routerLink="/users"
    [queryParams]="{ listAll: true }"
    fragment="userList"
>List All Users</a>
```
This link will load `[domain]/users?listAll=true#userList`.

---
## Examples

Routes in `app.module.ts` for [Absolute Path](#absolute-path-example) and [Relative Path Example 1](#relative-paths-example-1)
```js
const routes: Routes = [ { path: "users", component: UsersComponent } ]
```

### Absolute Path Example
`app-root` component:
```html
<h1>Page Content<h1>
<a routerLink="/users" >Users Page in page content</a>
<hr>
<router-outlet></router-outlet>
```
`app-users` component:
```html
<p>Users Component</p>
<a routerLink="/users" >Users Page in users component</a>
```
Both links will lead to `[domain]/users`.
### Relative Path Example 1
`app-root` component:
```html
<app-header></app-header>
<h1>Page Content<h1>
<a routerLink="users" >Users Page in page content</a>
<hr>
<router-outlet></router-outlet>
```
`app-header` component:
```html
<a routerLink="users" >Users Page in page header</a>
<hr>
```
`app-users` component:
```html
<p>Users Component</p>
<a routerLink="users" >Users Page in users component</a>
```
- The link in `app root` and `app-header` will lead to `[domain]/users`.
- The link in `app-users` will lead to `[domain]/users/users`, throwing an error since that route is not defined.

### Relative Path Example 2
Routes in `app.module.ts`
```js
const routes: Routes = [ 
    { path: "home/default", component: HomeComponent },
    { path: "users", component: UsersComponent } 
]
```
`app-root` component:
```html
<h1>Page Content<h1>
<a routerLink="home/default" >Home Page in page content</a>
<a routerLink="users" >Users Page in page content</a>
<hr>
<router-outlet></router-outlet>
```
`app-home` component:
```html
<p>Users Component</p>
<a routerLink="users" >Users Page in home component</a>
```
`app-users` component:
```html
<p>Users Component</p>
<a routerLink="../users" >Users Page in users component</a>
```
- In `app-root`:
    - The users link will lead to `[domain]/users`
    - The home link will lead to `[domain]/home/default`

- In `app-home` the link will lead to `[domain]/home/default/users`. It will *not* navigate up one segment (to `[domain]/home`), but instead remove the entire last loaded path, which was `"home/default"`.

- In `app-users` it will depend on which link was used to load the component originally:
    - When loaded from the link in `app-root` the link will lead to `[domain]/users`. On there the link will lead to the same path.
    - When loaded from `app-home` it will lead to `[domain]/home/default/users`. This will result in an error since that route is not defined.