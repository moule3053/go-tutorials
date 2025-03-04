# HTTP Servers and Clients in Go

## Introduction

Go provides a powerful and easy-to-use HTTP package that allows you to create web servers and clients. This tutorial will cover how to build a simple HTTP server, handle requests, and create HTTP clients to make requests to other servers.

## Creating an HTTP Server

You can create an HTTP server using the `net/http` package. The `http.HandleFunc()` function allows you to define handlers for specific routes.

### Example: Simple HTTP Server

```go
package main

import (
    "fmt"
    "net/http"
)

func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, World!")
}

func main() {
    http.HandleFunc("/", helloHandler) // Handle requests to the root URL
    fmt.Println("Starting server on :8080")
    http.ListenAndServe(":8080", nil) // Start the server
}
```

### Running the Server

To run the server, execute the program and navigate to `http://localhost:8080` in your web browser. You should see "Hello, World!" displayed.

## Handling HTTP Requests

You can handle different HTTP methods (GET, POST, etc.) by checking the `r.Method` field in your handler function.

### Example: Handling Different Methods

```go
package main

import (
    "fmt"
    "net/http"
)

func methodHandler(w http.ResponseWriter, r *http.Request) {
    switch r.Method {
    case http.MethodGet:
        fmt.Fprintf(w, "Received a GET request")
    case http.MethodPost:
        fmt.Fprintf(w, "Received a POST request")
    default:
        http.Error(w, "Unsupported method", http.StatusMethodNotAllowed)
    }
}

func main() {
    http.HandleFunc("/", methodHandler)
    fmt.Println("Starting server on :8080")
    http.ListenAndServe(":8080", nil)
}
```

## Reading Request Data

You can read data from the request, such as query parameters, form data, and headers.

### Example: Reading Query Parameters

```go
package main

import (
    "fmt"
    "net/http"
)

func queryHandler(w http.ResponseWriter, r *http.Request) {
    name := r.URL.Query().Get("name") // Get the "name" query parameter
    if name == "" {
        name = "World"
    }
    fmt.Fprintf(w, "Hello, %s!", name)
}

func main() {
    http.HandleFunc("/", queryHandler)
    fmt.Println("Starting server on :8080")
    http.ListenAndServe(":8080", nil)
}
```

### Example: Reading Form Data

To read form data, you need to parse the request body:

```go
package main

import (
    "fmt"
    "net/http"
)

func formHandler(w http.ResponseWriter, r *http.Request) {
    if r.Method == http.MethodPost {
        r.ParseForm() // Parse the form data
        name := r.FormValue("name") // Get the "name" form field
        fmt.Fprintf(w, "Hello, %s!", name)
        return
    }
    // Display the form
    fmt.Fprintf(w, `
        <form method="POST">
            Name: <input type="text" name="name">
            <input type="submit" value="Submit">
        </form>
    `)
}

func main() {
    http.HandleFunc("/", formHandler)
    fmt.Println("Starting server on :8080")
    http.ListenAndServe(":8080", nil)
}
```

## Creating an HTTP Client

You can create an HTTP client using the `http.Client` type to make requests to other servers.

### Example: Making a GET Request

```go
package main

import (
    "fmt"
    "net/http"
)

func main() {
    resp, err := http.Get("https://api.github.com")
    if err != nil {
        fmt.Println("Error making GET request:", err)
        return
    }
    defer resp.Body.Close() // Ensure the response body is closed

    fmt.Println("Response status:", resp.Status)
}
```

### Example: Making a POST Request

To make a POST request, you can use `http.Post()`:

```go
package main

import (
    "bytes"
    "fmt"
    "net/http"
)

func main() {
    data := []byte(`{"name": "John Doe"}`)
    resp, err := http.Post("https://httpbin.org/post", "application/json", bytes.NewBuffer(data))
    if err != nil {
        fmt.Println("Error making POST request:", err)
        return
    }
    defer resp.Body.Close()

    fmt.Println("Response status:", resp.Status)
}
```

## Handling HTTP Errors

When making HTTP requests, always check the response status code to handle errors appropriately:

```go
if resp.StatusCode != http.StatusOK {
    fmt.Println("Error: received status code", resp.StatusCode)
}
```

## Exercises

1. Create a simple HTTP server that serves static files from a directory.

2. Write a program that handles form submissions and displays the submitted data.

3. Implement an HTTP client that fetches data from a public API and displays the response.

4. Create a RESTful API with endpoints for creating, reading, updating, and deleting resources.

5. Write a program that makes concurrent HTTP requests to multiple URLs and collects the responses.

## Summary

In this tutorial, you've learned about HTTP servers and clients in Go:

- How to create a simple HTTP server and handle requests
- How to read request data, including query parameters and form data
- How to create an HTTP client and make GET and POST requests
- How to handle HTTP errors and check response status codes

Understanding how to work with HTTP servers and clients is essential for building web applications and services in Go. In the next tutorial, we'll explore working with JSON in Go. 