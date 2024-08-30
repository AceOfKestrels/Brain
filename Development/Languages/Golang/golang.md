# Golang (Go)

Go is an open-source programming language supported by Google, built for concurrency and lightweight cloud and network applications.

## Getting Started

Go can be downloaded from its [website](https.//go.dev), which also offers tutorials for beginners to learn the language.

### Creating a Module
The `go mod init` command can be used to create a new module (project):
```
$ go mod init example/helloWorld
go: creating new go.mod: module example/helloWorld
```

### Main Function
Go source files use the `.go` file extension. Functions are declared in packages. Like in other programming languages, the `main function` serves as the entry point of a Go program:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

`fmt` is a standard package that contains functions for formatting text, including printing text to the console. 

### External Packages
Packages that aren't included in the standard library must be added to the project dependencies. Packages can be found on [pkg.go.dev](https://pkg.go.dev/). The `index` section of a package's overview contains a list off all the functions and interfaecs it declares.

After importing a new package, use the `gp mod tidy` command to update dependencies:
```go
import "rsc.io/quote" // external package
```
```
$ go mod tidy
go: finding module for package rsc.io/quote
go: found rsc.io/quote in rsc.io/quote v1.5.2
```

After that, the `go.mod` module file is updated, and a `go.sum` file is created if it doesn't exist already.

### Local Packages
When creating your own packages you may want to use a local version if it's not published yet. 

Use the `go mod edit -replace` command to change a dependency's source:
```
$ go mod edit -replace example/greetings=../greetings
```
The above will update the module to use the local module located at `../greetings` in place of the `example/greetings` package.

Afterwards, use `go mod tidy` like normal to resolve the dependency.