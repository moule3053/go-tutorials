# Packages and Imports in Go

## Introduction

In Go, packages are the primary way to organize and reuse code. A package is a collection of Go source files in the same directory that are compiled together. The Go standard library provides a rich set of packages that you can use in your applications. This tutorial will cover how to create, use, and manage packages and imports in Go.

## Understanding Packages

Every Go program is made up of packages. The main package is the entry point of the program, and it must contain a `main` function. Other packages can be created to organize code into reusable components.

### Creating a Package

To create a package, follow these steps:

1. Create a new directory for your package.
2. Create a Go file in that directory and declare the package name at the top.

For example, create a directory named `mathutils` and a file named `mathutils.go`:

```go
// mathutils.go
package mathutils

// Add returns the sum of two integers
func Add(a, b int) int {
    return a + b
}

// Subtract returns the difference of two integers
func Subtract(a, b int) int {
    return a - b
}
```

### Using Packages

To use a package in your Go program, you need to import it. You can import standard library packages or your own packages.

```go
// main.go
package main

import (
    "fmt"
    "path/to/your/mathutils" // Replace with the actual path to your package
)

func main() {
    sum := mathutils.Add(5, 3)
    difference := mathutils.Subtract(5, 3)
    
    fmt.Println("Sum:", sum)           // Output: Sum: 8
    fmt.Println("Difference:", difference) // Output: Difference: 2
}
```

### Importing Multiple Packages

You can import multiple packages in a single import statement:

```go
import (
    "fmt"
    "math"
)
```

### Aliasing Imports

You can alias an imported package to avoid name conflicts or for convenience:

```go
import (
    m "math"
)

func main() {
    fmt.Println("Square root of 16:", m.Sqrt(16)) // Output: Square root of 16: 4
}
```

### Blank Identifier

If you want to import a package solely for its side effects (e.g., initializing a package), you can use the blank identifier:

```go
import _ "some/package"
```

## Example: Creating and Using a Custom Package

1. Create a directory named `mathutils`.
2. Create a file named `mathutils.go` in that directory with the following content:

```go
// mathutils.go
package mathutils

// Multiply returns the product of two integers
func Multiply(a, b int) int {
    return a * b
}

// Divide returns the quotient of two integers
func Divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```

3. Create a `main.go` file in the parent directory:

```go
// main.go
package main

import (
    "fmt"
    "path/to/your/mathutils" // Replace with the actual path to your package
)

func main() {
    product := mathutils.Multiply(4, 5)
    quotient, err := mathutils.Divide(20, 4)
    
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Product:", product)       // Output: Product: 20
        fmt.Println("Quotient:", quotient)     // Output: Quotient: 5
    }
}
```

## Exercises

1. Create a package named `stringutils` that contains functions for string manipulation (e.g., reversing a string, checking if a string is a palindrome).

2. Write a program that imports your `stringutils` package and uses its functions.

3. Create a package named `geometry` that contains functions for calculating the area and perimeter of different shapes (e.g., rectangle, circle).

4. Write a program that imports your `geometry` package and calculates the area and perimeter of a rectangle with given dimensions.

5. Explore the Go standard library and find a package that interests you. Write a program that uses that package.

## Summary

In this tutorial, you've learned about packages and imports in Go:

- How to create and use packages
- How to import standard and custom packages
- How to alias imports and use the blank identifier
- Practical examples of creating and using custom packages

Understanding packages and imports is essential for organizing and reusing code in Go. In the next tutorial, we'll explore file I/O in Go. 