# Testing
Golang's standard packages also include the `testing` package. This can be used to create unit tests.

Test files should be named `[originalFileName]_test.go`, use the same package as the one they're testing and import the `testing package`.

Tests are functions with a name `Test[FunctionToTest][TestCase]` and take a pointer to the `testing.T` type as parameter:
```go
package greetings

import "testing"

func TestGreetingEmpty(t *testing.T) {
    // ...
}
```

The test itself should run the function in question and check its result:
```go
package greetings

import "testing"

func TestGetGreetingEmpty(t *testing.T) {
	message, error := GetGreeting("")

	if error == nil || message != "" {
		t.Fatalf(`GetGreeting("") = %q, %v, want match for "", error`, message, error)
	}
}
```