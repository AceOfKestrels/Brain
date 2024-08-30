# Errors
In Go, errors are treated as values. Functions that can fail should return an error object in addition to the regular return type:
```go
func Hello(name string) (string, error) { }
```

## Throwing Errors
When an error occurs, a new error can be returned using the `error.New` function. This is part of the `errors` standard package. 

If a function executes successfully, the error should be `nil`.
```go
package greetings

import "errors"

func Hello(name string) (string, error) {
    if name == "" {
        return "", errors.New("empty name")
    }

    message := "Hello, " + name
    return message, nil
}
```

## Catching Errors
Since errors are values, they can be "caught" by checking the return value of a function:
```go
func main() {
    message, error := greetings.Hello("Kes")
    if error != nil {
        fmt.Println("An error occurred.")        
        return
    }

    fmt.Println(message)
}
```