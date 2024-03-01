# Authentication

Normally, authentication works through sessions. However, since we are building a single-page application in Angular, our frontend is decoupled from the backend. We are also communicating with our backend through an API, usually a REST or GraphQL one, both of which are stateless. 

To mitigate this and implement user authentication we use tokens generated and encoded by the server, which the user sends with every request that requires authentication.

## Receiving a Token
The specifics of this will depend on the API you are using, but generally you send a request to sign up/log in to the server, which then returns a token to the client. 
It might look something like this:
```js
signUp(email: string, password: string) {
    this.httpClient.post( 
        "https://example.com/api/signup", 
        {
            email: email,
            password: password
        } 
    ).subscribe( 
        response => {
            this.userToken = respnse.token;
        },
        error => {
            this.errorMessage = "Something went wrong."
        }
    )
}
```

## Using a Token
To authenticate requests to the backend, we generally have to attach our token to the HTTP header or query parameters of our request. See [Headers and Query Parameters](./http-requests.md#headers-and-query-parameters) for more information.