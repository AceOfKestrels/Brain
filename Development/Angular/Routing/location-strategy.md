# Location Strategies
By default, URLs are handled by the server first. This means that Angular might not be able to take over when Routing is used.

To fix this, the server must be set up so that in case of a `404 Not Found` error it returns the `index.html` instead of an error page.

## Hash-Mode-Routing
Alternatively, you can enable Hash-Mode-Routing by enabling it in the routes import section of your module:
```js
@NgModule({
    imports: [ 
        RouterModule.forRoot(
            routes, 
            { useHash: true }
        ) 
    ],
})
```
This will however add a `#` in between your domain and route in the URL.