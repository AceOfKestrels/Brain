# HTTP Verbs
> HTTP defines a set of request methods to indicate the desired action to be performed for a given resource. Although they can also be nouns, these request methods are sometimes referred to as HTTP verbs. Each of them implements a different semantic, but some common features are shared by a group of them: e.g. a request method can be safe, idempotent, or cacheable.
>
> *([mdn web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods))*

## GET
The [GET](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) method requests a representation of the target resource. GET requests should have no other side effects. 
- GET requests **are cachable**.
- The body of GET requests **may have a body**.
- The response body to a GET request should contain a **representation of the target resource** as a body, if successful.
- Usual response status codes:
    - **200 (OK)**, if the requested resource exists
    - **404 (Not Found)**, if the requested resource does not exist

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-get))

## HEAD
The [HEAD](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/HEAD) method requests the same response as GET, but *without* the response body. It may be used to check if a resource exists, or find the size of a file without transferring the whole resource.
- HEAD requests **are cachable**.
- The body of HEAD requests **may have a body**.
- The response to a HEAD request **have no body**.
- Usual response status codes:
    - **200 (OK)**, if the requested resource exists
    - **404 (Not Found)**, if the requested resource does not exist

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-head))

## POST
The [POST](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) method requests that the target resource processes the resource represented in its body. This might mean posting a message to a forum, signing up as a user or uploading a file. The exact semantics to this however, are up to the server.
- POST requests **are cachable**.
- POST requests **require a body**.
- The response body to a POST request contains a **representation of the new resource** if successful.
- Usual response status codes:
    - **201 (Created)**, if the resource was created successfully (as well as a `Location` header pointing to the new resource)
    - **303 (See Other)**, if an equivalent resource exists (with that resource's identifier as a `Location` header) 

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-post))

## PUT
The [PUT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/put) method requests creates a new resource or replaces a target resource with the representation sent as a body.
- PUT requests **are not cachable**.
- PUT requests **require a body**.
- The response to a PUT request **may have a body**.
- Usual response status codes:
    - **201 (Created)**, if the resource was created successfully
    - **200 (OK)** or **204 (No Content)**, if an existing resource was modified
    - **409 (Conflict)** or **415 (Unsupported Media Type)**, if the request resource does not match the target resource's constraints

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-put))

## PATCH
The [PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/patch) method requests partial modification of a target resource. PATCH requests contains a set of instructions to modify an existing resource, such as which data fields to update.
- PATCH requests **may be cachable** if freshness information is included.
- PATCH requests **require a body**.
- The response to a PATCH request **may have a body**.

## DELETE
The [DELETE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/delete) method requests deletion of a target resource. 
- DELETE requests **are not cachable**.
- DELETE requests **may have a body**.
- The response to a DELETE request **may have a body**.
- Usual response status codes:
    - **204 (Not Content)**, if deletion was successful and no further information is provided
    - **202 (Accepted)**, if deletion is likely successful but not yet completed
    - **200 (OK)**, if deletion was successful and the response message describes the status

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-delete))

## CONNECT
The [CONNECT](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/connect) method requests to start two-way communication with the target resource.
- CONNECT requests **are not cachable**.
- CONNECT requests **have no body**.
- The response to a CONNECT request **have no body**.

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-connect))

## OPTIONS
The [OPTIONS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/options) method requests information about the supported HTTP methods of the target resource.
- OPTIONS requests **are not cachable**.
- OPTIONS requests **have no body**.
- The response to a OPTIONS request **may have a body**.

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-options))

## TRACE
The [TRACE](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/trace) method requests a message loop-back test along the path to the target resource. The target should send the original request back in the response body.
- TRACE requests **are not cachable**.
- TRACE requests **have no body**.
- The response to a OPTIONS request **must have a body** if successful.

([specification](https://www.rfc-editor.org/rfc/rfc9110.html#name-trace))