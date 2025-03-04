# Working with JSON in Go

## Introduction

JSON (JavaScript Object Notation) is a lightweight data interchange format that is easy to read and write for humans and machines. Go provides built-in support for encoding and decoding JSON data through the `encoding/json` package. This tutorial will cover how to work with JSON in Go, including marshaling and unmarshaling data.

## Marshaling JSON

Marshaling is the process of converting a Go data structure into JSON format. You can use the `json.Marshal()` function to achieve this.

### Example: Marshaling a Struct to JSON

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Person struct {
    Name  string `json:"name"`
    Age   int    `json:"age"`
    Email string `json:"email,omitempty"` // Omit if empty
}

func main() {
    p := Person{Name: "Alice", Age: 30, Email: "alice@example.com"}
    
    jsonData, err := json.Marshal(p)
    if err != nil {
        fmt.Println("Error marshaling to JSON:", err)
        return
    }
    
    fmt.Println("JSON data:", string(jsonData))
}
```

### Pretty Printing JSON

To produce more readable (pretty-printed) JSON, use `json.MarshalIndent()`:

```go
jsonData, err := json.MarshalIndent(p, "", "  ")
```

## Unmarshaling JSON

Unmarshaling is the process of converting JSON data back into a Go data structure. You can use the `json.Unmarshal()` function for this purpose.

### Example: Unmarshaling JSON to a Struct

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    jsonData := `{"name":"Bob","age":25,"email":"bob@example.com"}`
    
    var p Person
    err := json.Unmarshal([]byte(jsonData), &p)
    if err != nil {
        fmt.Println("Error unmarshaling JSON:", err)
        return
    }
    
    fmt.Printf("Decoded Person: %+v\n", p)
}
```

## Working with JSON Arrays

You can also work with JSON arrays by defining a slice of structs.

### Example: Marshaling and Unmarshaling JSON Arrays

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    people := []Person{
        {Name: "Alice", Age: 30, Email: "alice@example.com"},
        {Name: "Bob", Age: 25, Email: "bob@example.com"},
    }
    
    // Marshaling
    jsonData, err := json.Marshal(people)
    if err != nil {
        fmt.Println("Error marshaling to JSON:", err)
        return
    }
    fmt.Println("JSON data:", string(jsonData))
    
    // Unmarshaling
    var decodedPeople []Person
    err = json.Unmarshal(jsonData, &decodedPeople)
    if err != nil {
        fmt.Println("Error unmarshaling JSON:", err)
        return
    }
    fmt.Printf("Decoded People: %+v\n", decodedPeople)
}
```

## Handling JSON with Maps

You can also unmarshal JSON data into a map:

```go
package main

import (
    "encoding/json"
    "fmt"
)

func main() {
    jsonData := `{"name":"Charlie","age":35,"email":"charlie@example.com"}`
    
    var result map[string]interface{}
    err := json.Unmarshal([]byte(jsonData), &result)
    if err != nil {
        fmt.Println("Error unmarshaling JSON:", err)
        return
    }
    
    fmt.Println("Decoded JSON to map:")
    for key, value := range result {
        fmt.Printf("%s: %v\n", key, value)
    }
}
```

## Error Handling in JSON Operations

Always check for errors when marshaling and unmarshaling JSON data to handle potential issues gracefully.

## Exercises

1. Write a program that reads a JSON file and unmarshals it into a struct.

2. Create a struct that represents a book and implement functions to marshal and unmarshal JSON data for it.

3. Implement a program that marshals a slice of structs to JSON and writes it to a file.

4. Write a program that reads a JSON array from a file and unmarshals it into a slice of structs.

5. Create a program that uses a map to represent dynamic JSON data and demonstrates marshaling and unmarshaling.

## Summary

In this tutorial, you've learned about working with JSON in Go:

- How to marshal Go data structures to JSON
- How to unmarshal JSON data back into Go data structures
- How to work with JSON arrays and maps
- Error handling in JSON operations

Understanding how to work with JSON is essential for building web applications and APIs in Go. In the next tutorial, we'll explore advanced Go features and best practices. 