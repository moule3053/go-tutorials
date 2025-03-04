# Advanced Concurrency Patterns in Go

## Introduction

Go's concurrency model, based on goroutines and channels, allows for powerful and flexible concurrent programming. In this tutorial, we will explore advanced concurrency patterns, including worker pools, fan-out/fan-in, and using context for cancellation and timeouts.

## Worker Pools

A worker pool is a common concurrency pattern where a fixed number of goroutines (workers) process tasks from a shared channel. This pattern helps manage resource usage and control the number of concurrent operations.

### Example: Worker Pool

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, jobs <-chan int, wg *sync.WaitGroup) {
    defer wg.Done()
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        time.Sleep(time.Second) // Simulate work
    }
}

func main() {
    const numWorkers = 3
    jobs := make(chan int, 10)
    var wg sync.WaitGroup

    // Start workers
    for w := 1; w <= numWorkers; w++ {
        wg.Add(1)
        go worker(w, jobs, &wg)
    }

    // Send jobs to the workers
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs) // Close the jobs channel

    wg.Wait() // Wait for all workers to finish
    fmt.Println("All jobs processed")
}
```

## Fan-Out/Fan-In

The fan-out pattern involves multiple goroutines reading from the same channel, while the fan-in pattern involves multiple channels being merged into a single channel.

### Example: Fan-Out

```go
package main

import (
    "fmt"
    "sync"
)

func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
    defer wg.Done()
    for job := range jobs {
        fmt.Printf("Worker %d processing job %d\n", id, job)
        results <- job * 2 // Simulate processing
    }
}

func main() {
    jobs := make(chan int, 10)
    results := make(chan int, 10)
    var wg sync.WaitGroup

    // Start workers
    for w := 1; w <= 3; w++ {
        wg.Add(1)
        go worker(w, jobs, results, &wg)
    }

    // Send jobs
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs) // Close the jobs channel

    wg.Wait() // Wait for all workers to finish
    close(results) // Close the results channel

    // Collect results
    for result := range results {
        fmt.Println("Result:", result)
    }
}
```

### Example: Fan-In

```go
package main

import (
    "fmt"
)

func merge(ch1, ch2 <-chan int) <-chan int {
    merged := make(chan int)
    go func() {
        for {
            select {
            case v, ok := <-ch1:
                if ok {
                    merged <- v
                }
            case v, ok := <-ch2:
                if ok {
                    merged <- v
                }
            }
        }
    }()
    return merged
}

func main() {
    ch1 := make(chan int)
    ch2 := make(chan int)

    go func() {
        for i := 1; i <= 5; i++ {
            ch1 <- i
        }
        close(ch1)
    }()

    go func() {
        for i := 6; i <= 10; i++ {
            ch2 <- i
        }
        close(ch2)
    }()

    merged := merge(ch1, ch2)

    for v := range merged {
        fmt.Println("Received:", v)
    }
}
```

## Context for Cancellation and Timeouts

The `context` package provides a way to manage cancellation signals and deadlines for goroutines. It is especially useful for long-running operations.

### Creating a Context

You can create a context using `context.Background()` or `context.TODO()`:

```go
ctx := context.Background()
```

### Using Context with Goroutines

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

### Using Context with Timeouts

You can create a context with a timeout using `context.WithTimeout()`:

```go
package main

import (
    "context"
    "fmt"
    "time"
)

func doWork(ctx context.Context) {
    select {
    case <-time.After(2 * time.Second):
        fmt.Println("Work completed")
    case <-ctx.Done():
        fmt.Println("Work canceled:", ctx.Err())
    }
}

func main() {
    ctx, cancel := context.WithTimeout(context.Background(), 1*time.Second)
    defer cancel() // Ensure the cancel function is called

    go doWork(ctx)

    time.Sleep(3 * time.Second) // Wait for the goroutine to finish
}
```

## Exercises

1. Implement a worker pool that processes tasks from a channel and returns results to another channel.

2. Create a program that demonstrates the fan-out pattern by starting multiple workers that read from the same jobs channel.

3. Write a program that merges multiple channels into a single channel using the fan-in pattern.

4. Implement a context-based cancellation mechanism for a long-running operation.

5. Create a program that uses context with a timeout to manage the execution of multiple goroutines.

## Summary

In this tutorial, you've learned about advanced concurrency patterns in Go:

- How to implement worker pools
- The fan-out and fan-in patterns for managing goroutines
- Using the `context` package for cancellation and timeouts

Understanding these advanced concurrency patterns will help you write more efficient and responsive Go applications. In the next tutorial, we'll explore testing in Go. 