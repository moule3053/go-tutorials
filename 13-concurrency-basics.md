# Concurrency Basics in Go

## Introduction

Concurrency is a powerful feature in Go that allows you to run multiple tasks simultaneously. Go's concurrency model is built around goroutines and channels, making it easy to write concurrent programs. This tutorial will introduce you to the basics of concurrency in Go, including how to create goroutines and communicate between them using channels.

## Goroutines

A goroutine is a lightweight thread managed by the Go runtime. You can create a goroutine by using the `go` keyword followed by a function call:

```go
package main

import (
    "fmt"
    "time"
)

func sayHello() {
    fmt.Println("Hello from goroutine!")
}

func main() {
    go sayHello() // Start a new goroutine
    time.Sleep(1 * time.Second) // Wait for the goroutine to finish
    fmt.Println("Main function finished")
}
```

### Goroutine Lifecycle

Goroutines run concurrently with the main program. The Go runtime schedules goroutines, and they can be paused, resumed, or stopped based on system resources.

### Synchronization

When using goroutines, you often need to synchronize their execution. This can be done using channels, which we will cover next.

## Channels

Channels are a way for goroutines to communicate with each other. They allow you to send and receive values between goroutines, providing a safe way to share data.

### Creating Channels

You can create a channel using the `make` function:

```go
ch := make(chan int) // Create a channel for integers
```

### Sending and Receiving Values

You can send values to a channel using the `<-` operator:

```go
ch <- value // Send value to the channel
```

To receive a value from a channel, use the same operator:

```go
value := <-ch // Receive value from the channel
```

### Example: Using Channels

Here's an example that demonstrates how to use channels to communicate between goroutines:

```go
package main

import (
    "fmt"
)

func calculateSquare(n int, ch chan int) {
    square := n * n
    ch <- square // Send the result to the channel
}

func main() {
    ch := make(chan int) // Create a channel

    for i := 1; i <= 5; i++ {
        go calculateSquare(i, ch) // Start a goroutine for each number
    }

    // Receive results from the channel
    for i := 1; i <= 5; i++ {
        result := <-ch // Receive the square from the channel
        fmt.Println("Square:", result)
    }
}
```

## Buffered Channels

By default, channels are unbuffered, meaning that sends and receives block until the other side is ready. You can create buffered channels to allow a limited number of values to be sent without blocking:

```go
ch := make(chan int, 2) // Create a buffered channel with a capacity of 2
```

### Example: Buffered Channels

```go
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int, 2) // Buffered channel

    ch <- 1 // Send value to the channel
    ch <- 2 // Send another value

    fmt.Println(<-ch) // Receive value from the channel
    fmt.Println(<-ch) // Receive another value
}
```

## Closing Channels

You can close a channel to indicate that no more values will be sent. This is useful for signaling completion:

```go
close(ch) // Close the channel
```

### Example: Closing Channels

```go
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int)

    go func() {
        for i := 1; i <= 5; i++ {
            ch <- i * i
        }
        close(ch) // Close the channel when done
    }()

    // Receive values until the channel is closed
    for value := range ch {
        fmt.Println("Received:", value)
    }
}
```

## Select Statement

The `select` statement allows you to wait on multiple channel operations. It is similar to a `switch` statement but for channels:

```go
select {
case value := <-ch1:
    // Handle value from ch1
case value := <-ch2:
    // Handle value from ch2
case <-time.After(1 * time.Second):
    // Handle timeout
}
```

### Example: Using Select

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(2 * time.Second)
        ch1 <- "Result from channel 1"
    }()

    go func() {
        time.Sleep(1 * time.Second)
        ch2 <- "Result from channel 2"
    }()

    select {
    case res := <-ch1:
        fmt.Println(res)
    case res := <-ch2:
        fmt.Println(res)
    case <-time.After(3 * time.Second):
        fmt.Println("Timeout")
    }
}
```

## Exercises

1. Write a program that starts multiple goroutines to calculate the factorial of numbers from 1 to 5 and sends the results to a channel.

2. Create a program that uses a buffered channel to send messages from multiple goroutines to a single receiver.

3. Implement a program that uses the `select` statement to handle multiple channels and a timeout.

4. Write a program that demonstrates closing a channel and how to handle it in a receiving goroutine.

5. Create a producer-consumer example using channels where one goroutine produces data and another consumes it.

## Summary

In this tutorial, you've learned about concurrency in Go:

- What goroutines are and how to create them
- How to use channels for communication between goroutines
- The difference between buffered and unbuffered channels
- How to close channels and use the `select` statement

Concurrency is a powerful feature in Go that allows you to write efficient and responsive programs. In the next tutorial, we'll explore channels in more detail. 