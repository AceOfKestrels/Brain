# Structs
Structs in Go can hold values and implement methods that can be called on their instances. They are defined using the `type` and `struct` keywords:
```go
type Foo struct{}
```

## Fields
Struct fields are defined directly in the declaration itself:
```go
type Foo struct {
    Bar: Bar
    Value: int
}
```
No default values can be set this way.

## Constructor
Instances of structs can be created by calling their name. When doing so, not all fields need to be initialized. It is possible however to set their values directly this way:
```go
func main() {
    bar := Bar{}

    foo := Foo{
        Bar: bar,
        Value: 1
    }
}
```
<br>

To make sure structs are instantiated correctly, you can define a "constructor function" to create new instances. This is similar to factory methods or a dedicated constructor in other languages:
```go
type Foo struct {
    Bar: Bar
    Value: int
}

func NewFoo(bar Bar) *Foo {
    return &Foo{ Bar: bar, Value: 1 }
}

func main() {
    bar := Bar{}
    foo := NewFoo(bar)
}
```
Constructor methods should return a pointer to the newly created instance, as denoted by the `*` and `&` symbols.

## Methods
Functions are turned to methods and added to a struct by using so-called "receiver arguments". They come before the function name and are a reference to the instance of the struct they belong to. You may think of them as the `this` keyword in other languages, however naming it that way is discouraged:
```go
package service

import "example/models"

type ExampleServiceImpl struct{}

func (serv *ExampleServiceImpl) GetFoo() (models.Foo) {
    // ...
}
func (serv *ExampleServiceImpl) AddFoo(foo models.Foo) (models.Foo, error) {
    // ...
}
```

### Pointer vs Value Receivers
In the previous example, the receiver arguments were used as Pointer Receivers, as denoted by the `*`. That means the method operates directly on the struct it belongs to, modifying any data directly. 

This may not be the intended behavior, so Value Receivers exist. They are used by simply leaving out the `*`. When using Value Receivers, the method operates on a copy of the struct. Modifying data will have no impact on the original instance:
```go
type ExampleServiceImpl struct {
	Number int
}

func (serv *ExampleServiceImpl) Get() int {
	return serv.Number
}
func (serv *ExampleServiceImpl) SetPointer(newNumber int) {
	serv.Number = newNumber
}
func (serv ExampleServiceImpl) SetValue(newNumber int) {
	serv.Number = newNumber
}

func main() {
	service := ExampleServiceImpl{Number: 1}

	service.SetPointer(2)
	fmt.Println(service.Number) // 2
	service.SetValue(5)
	fmt.Println(service.Number) // still 2
}
```
<br>

A useful application for value receivers is to return a modified copy of an object:
```go
type Rectangle struct {
    Height, Width float
}

func (rect Rectangle) Scale(factor float) Rectangle {
    rect.Height *= factor
    rect.Width *= factor
    return rect
}
```

## Access
Go does not use keywords such as `public`, `private` or `protected` as access modifiers. Instead access to a struct, field, function or other member across packages and instances in controlled via the capitalisation of their names:
```go
// Uppercase - "public"
func GetFoo() models.Foo {
    // ...
}

// lowercase - "private"
func getBar() models.Foo {
    // ...
}
```