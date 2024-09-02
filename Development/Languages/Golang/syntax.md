# Syntax

## If Statements
```go
if i == 0 {
    // ...
}
```

## Collections
Slices are like lists:
```go
greetings := []string{"Hello", "Goo morning", "Welcome"}
```
Maps:
```go
messages := make(map[string]string)
messages["Kes"] = "Hello, Kes"
```

## Loops
```go
for i := 0; i < 10; i++ {
    // ...
}
```
`for` can also be used as a while-loop:
```go
i := 0
for i < 10 {
    i++
}
```
Infinite loop:
```go
for {
    // ...
}
```
For in range:
```go
for index, greeting := range greetings {
    // ...
}

for index := range greetings {
    greeting := greetings[index]
}

for _, greeting := range greetings {
    // ...
}
```

## Switch
No `break` is required in each case:
```go
switch i {
    case 0:
        fmt.Println("zero")
    case 1:
        fmt.Println("one")
    case 2:
        fmt.Println("two")
    default:
        fmt.Println("other")
} 
```
Switch can be used without a starting condition to simplify if-else chains:
```go
t := time.Now()
switch {
    case t.Hour() < 12:
        fmt.Println("Good morning!")
    case t.Hour() < 17:
        fmt.Println("Good afternoon.")
    default:
        fmt.Println("Good evening.")
}
```

## Defer
The `defer` keyword delays the execution of a line until the surrounding function finishes:
```go
func main() {
  defer fmt.Println("hello")
  fmt.Println("world")
}

// Output:
// world
// hello
```