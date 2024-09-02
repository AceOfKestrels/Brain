# Interfaces
Like in other programming languages, interfaces in Go describe the public members without specifying the implementation details.

Unlike in other languages, interfaces are implemented *implicitly*, that means any struct that contains all the members defined in the interface is considered an implementation.

## Defining an Interface
When using an interface pattern it is a good idea to start at the consumer end. Consider which methods you need and declare them in an interface. The interface should be declared in the package of the consumer:
```go
package foo

import "example/models"

type ExampleService interface {
    GetFoo() []models.Foo
    AddFoo(foo models.Foo) (models.Foo, error)
}
```

## Implementing an Interface
Interfaces are implemented by structs. Whether a given struct implements an interface depends soley on its implemented members. 

See [Structs](./structs.md) for more information on implementing them.

### Implementing multiple Interfaces
A struct may implement more members than are required for the interface and can thus implement different interfaces:
```go
package foo
import "example/models"

type FooService interface {
    GetFoo() []models.Foo
}
```
```go
package bar
import "example/models"

type BarService interface {
    GetBar() []models.Bar
}
```
```go
package service
import "example/models"

type FooBarServiceImpl struct{}

func (serv *ExampleServiceImpl) GetFoo() (models.Foo) {
    // ...
}
func (serv *ExampleServiceImpl) GetBar() (models.Bar) {
    // ...
}
```