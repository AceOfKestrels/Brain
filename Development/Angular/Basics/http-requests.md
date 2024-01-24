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

---
## Handling Errors
HTTP Requests might fail for a varietey of reasons. To enhance the user experience, we should react to those errors.

As we learned when looking at [observables](./observables.md) we can react to errors returned by an observable:
```js
get(id: int) : UserData {
    let request = httpClient.get<UserData>("https://example.com/api/" + id);
    request.subscribe( 
        userData => {
            return userData;
        }, 
        error => {
            console.log("Fetching user data failed: \n" + error);
            return null;
        }
     )
}
```
[Subjects](./observables.md#subjects) can be used to pass error messages to multiple components from an HTTP service.

### The `catchError` operator
You can also use the `catchError` operator using the `Observable.pipe` method to react to errors before they are passed to the subscriber:
```js
get(id: int) : Observable<UserData> {
    let request = 
        httpClient
            .get<UserData>("https://example.com/api/" + id)
            .pipe( catchError( error => {
                console.log(error);
                return throwError(error);
            }));
    return request;
}
```

---
## Headers and Query Parameters
All request methods on the `HttpClient` also take configurations to the reqeust as another parameter. Here, headers, query params and other things can be set:
```js
post(item: { id: string, name: string }) {
    let request = httpClient.get<UserData>(
        "https://example.com/api/", 
        {
            headers: new HttpHeaders({ "Authorization": this.apiKey }),
            params: new HttpParams().set("detail", "true")
        }
    );
    return request;
}
```

---
## Interceptors
Interceptors can be used to run code just before an HTTP request is sent. They should be put in a separate service class that inplements the `HttpInterceptor` interface:
```js
export class LogInterceptorService implements HttpInterceptor {
    intercept( req: HttpRequest<any>, next: HttpHandler) {
        console.log("A request was sent: \n" + req);
        return next.handle(req);
    }
}
```
The interceptor then needs to be added to the `providers` array in the app module in a special way:
```js
providers: [ { 
    provide: HTTP_INTERCEPTORs, 
    useClass: LogInterceptorService,
    multi: true
} ]
```

### Modifying Requests
Interceptors can also be used to modifify any requests that are sent:
```js
export class AuthInterceptorService implements HttpInterceptor {

    apiKey = "abcde"

    intercept( req: HttpRequest<any>, next: HttpHandler) {
        const modiifiedRequest = req.clone({
            headers = req.headers.append("Authorization", apiKey)
        });
        return next.handle(modifiedRequest);
    }
}
```

### Intercepting Responses
Interceptors can also be used to interact with and modify the response of a request using the `pipe` method:
```js
export class ResponseLoggerInterceptorService implements HttpInterceptor {
    intercept( req: HttpRequest<any>, next: HttpHandler) {
        return next.handle(req).pipe(
            tep( event => console.log(event) )
        );
    }
}
```