# Context Package in Go

## Introduction

The `context` package in Go provides a way to manage deadlines, cancellation signals, and request-scoped values across API boundaries and between goroutines. It is particularly useful for managing long-running operations and ensuring that resources are cleaned up properly. This tutorial will cover the basics of the `context` package, including how to create and use contexts effectively.

## Creating Contexts

You can create a context using `context.Background()` or `context.TODO()`:

```go
ctx := context.Background() // Base context
```

### With Cancel

You can create a cancellable context using `context.WithCancel()`:

```go
ctx, cancel := context.WithCancel(context.Background())
defer cancel() // Ensure the cancel function is called
```

### With Timeout

You can create a context with a timeout using `context.WithTimeout()`:

```go
ctx, cancel := context.WithTimeout(context.Background(), 2*time.Second)
defer cancel() // Ensure the cancel function is called
```

### With Deadline

You can create a context with a specific deadline using `context.WithDeadline()`:

```go
deadline := time.Now().Add(2 * time.Second)
ctx, cancel := context.WithDeadline(context.Background(), deadline)
defer cancel() // Ensure the cancel function is called
```

## Using Contexts with Goroutines

You can pass a context to goroutines to allow them to be canceled:

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func doWork(ctx context.Context) {
    for {
        select {
        case <-ctx.Done():
            fmt.Println("Work canceled")
            return
        default:
            fmt.Println("Working...")
            time.Sleep(1 * time.Second)
        }
    }
}

func main() {
    ctx, cancel := context.WithCancel(context.Background())
    go doWork(ctx)

    time.Sleep(3 * time.Second)
    cancel() // Cancel the context
    time.Sleep(1 * time.Second) // Give time for goroutine to finish
}
```

## Context Values

Contexts can carry request-scoped values. You can use `context.WithValue()` to create a context that carries a value:

```go
ctx := context.WithValue(context.Background(), "key", "value")
```

### Example: Using Context Values

```go
package main

import (
    "context"
    "fmt"
)

func main() {
    ctx := context.WithValue(context.Background(), "userID", 42)

    // Pass the context to a function
    processRequest(ctx)
}

func processRequest(ctx context.Context) {
    userID := ctx.Value("userID").(int) // Type assertion
    fmt.Println("Processing request for user ID:", userID)
}
```

## Best Practices for Using Contexts

1. **Use Context for Cancellation**: Use contexts to manage cancellation signals for long-running operations.

2. **Pass Contexts Explicitly**: Always pass contexts explicitly to functions and goroutines that need them.

3. **Avoid Storing Contexts in Structs**: Contexts should not be stored in struct fields; they should be passed as parameters.

4. **Use Context Values Sparingly**: Use context values for request-scoped data that is needed across API boundaries. Avoid using them for passing optional parameters.

5. **Handle Timeouts and Deadlines**: Use `WithTimeout` and `WithDeadline` to manage time-sensitive operations.

## Exercises

1. Write a program that uses a context to manage the cancellation of multiple goroutines.

2. Create a function that simulates a long-running operation and uses a context with a timeout.

3. Implement a program that passes values through a context and retrieves them in a nested function.

4. Write a program that demonstrates the use of context values to carry request-scoped data.

5. Create a function that accepts a context and performs an operation based on the context's cancellation status.

## Summary

In this tutorial, you've learned about the context package in Go:

- How to create and use contexts for cancellation and timeouts
- How to pass contexts to goroutines
- How to use context values to carry request-scoped data
- Best practices for using contexts effectively

The context package is a powerful tool for managing cancellation and deadlines in Go applications. In the next tutorial, we'll explore HTTP servers and clients in Go. 