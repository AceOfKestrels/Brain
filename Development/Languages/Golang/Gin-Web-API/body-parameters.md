# Request Body and Parameters
When handling API requests in Gin, you often want to handle parameters from the path or additional data from the request body.

## Parameters
Path parameters are defined in the endpoint definition when setting up the Gin engine. Use `:` in front of a path segment to declare it as a parameter:
```go
func main() {
    engine := gin.Default()

    engine.GET("/foo/:id", GetFoo)

    engine.Run("localhost:8080")
}
```
<br>

In the request handler, the parameter can be retrieved using the `Context.Param` method:
```go
func GetFoo(c *gin.Context) {
    id := c.Param("id")

    foo, error := fooService.Get(id)
    if error != nil {
        c.AbortWithError(http.StatusBadRequest, error)
        return
    }

    c.JSON(http.StatusOK, foo)
}
```

### Query Parameters
Query Parameters can be retrieved similarly to path parameters, using the `Context.Query` method. In addition to that a default value may be specfied, which will be used if the parameter wasn't given:
```go
func GetBars(c *gin.Context) {
    filterString := c.Query("filter")   // empty string if not given
    page := c.DefaultQuery("page", "1") // defaults to "1"

    bar, error := barService.GetBars(filterString, page)
    if error != nil {
        c.AbortWithError(http.StatusBadRequest, error)
        return
    }

    c.JSON(http.StatusOK, bars)
}
```

## Request Body
To retrieve data from the request body, a model must be defined first, so that the JSON can be deserialized. Use struct tags to define the JSON property name:
```go
type Foo struct {
    Value   int     `json:"value"`
    Name    string  `json:"name"`
}
```
<br>

After defining a model, the `Context.BindJSON` method can be used to deserialize the data. It takes a pointer to a previously declared variable as its argument:
```go
func PostFoo(c *gin.Context) {
    var foo models.Todo
    error := c.BindJSON(&foo)

    if error != nil {
        c.AbortWithError(http.StatusBadRequest, error)
        return
    }

    added, error := fooService.Add(foo)
    if error != nil {
        c.AbortWithError(http.StatusBadRequest, error)
        return
    }

    c.IndentedJSON(http.StatusOK, added)
}
```