# HTTP Requests

## Sending Requests
Add the `HttpClientModule` to the app module imports:
```js
imports: [ BrowserModule, HttpClientModule ]
```
Inject an `HttpClient` into the component you want to use it in:
```js
constructor ( private httpClient: HttpClient ) {}
```
The `HttpClient` can then be used to send HTTP requests:
```js
post(item: { id: string, name: string }) {
    httpClient.post("https://example.com/api/", item);
}
```
Methods such as `get` or `post` return an observable with the response. 
To make sure Angular even sends the request, you must subscribe to this observable:
```js
post(item: { id: string, name: string }) {
    let request = httpClient.post("https://example.com/api/", item);
    request.subscribe( responseData => console.log(responseData) );
}
```
The observable will automatically complete when a response is received.