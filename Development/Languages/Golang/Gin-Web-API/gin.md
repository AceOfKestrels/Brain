# Web APIs with Gin
Gin is a web framework written in Go, that can be used to create simple web APIs.

To use Gin, import it to your module:
```go
import "github.com/gin-gonic/gin"
```
[Package Overview](https://pkg.go.dev/github.com/gin-gonic/gin)

## Running the API
To create a web application, create a new `gin.Engine` in your main function and call the `Run` method:
```go
import "github.com/gin-gonic/gin"

func main() {
    engine := gin.Default()

    engine.Run("localhost:8080")
}
```
This will have no endpoints defined so far, however after starting the application with `go run` and opening it in a browser, it should return a `404 Not Found`.

## Defining Endpoints
API endpoints in Gin are defined in two steps.

### 1. Endpoint Function
Define a function that should get called when the endpoint is requested. It takes a pointer to a `gin.Context` as its argument, that can be used to access the request and response:
```go
func GetFoo(c *gin.Context) {
    c.JSON(http.StatusOK, foo)
}
```
`gin.Context.JSON` returns a response containing the JSON representation of an object.

Every execution path must end in a response or the connection will time out and fail.

### 2. Endpoint Definition
After defining a function to call, you can tell Gin at which path or endpoint this function should get called. The `gin.Engine` has several methods for this purpose:
```go
func main() {
    engine := gin.Default()

    engine.GET("/foo", GetFoo)

    engine.Run("localhost:8080")
}
```