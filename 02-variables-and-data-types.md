# Variables and Data Types in Go

## Introduction

Variables are fundamental building blocks in any programming language. In Go, variables are statically typed, which means that once a variable is declared with a certain type, it cannot hold values of other types. This tutorial will cover how to declare and use variables in Go, as well as the various data types available.

## Variable Declaration

Go provides several ways to declare variables:

### 1. Using the `var` keyword

```go
var name string = "John"
var age int = 30
var isEmployed bool = true
```

### 2. Type inference

Go can infer the type when an initializer is present:

```go
var name = "John"    // string
var age = 30         // int
var isEmployed = true // bool
```

### 3. Short declaration with `:=`

Inside functions, you can use the short declaration operator `:=` to declare and initialize variables:

```go
func main() {
    name := "John"      // string
    age := 30           // int
    isEmployed := true  // bool
}
```

### 4. Multiple variable declaration

You can declare multiple variables at once:

```go
var x, y, z int = 1, 2, 3

// Or with type inference
var a, b, c = 1, "hello", true

// Or with short declaration
i, j, k := 1, "world", false
```

### 5. Block declaration

```go
var (
    name      string = "John"
    age       int    = 30
    isEmployed bool   = true
)
```

## Basic Data Types in Go

Go has several built-in data types:

### Numeric Types

#### Integer Types
- `int`: Platform-dependent, 32 or 64 bits
- `int8`: 8-bit signed integer (-128 to 127)
- `int16`: 16-bit signed integer (-32,768 to 32,767)
- `int32`: 32-bit signed integer (-2,147,483,648 to 2,147,483,647)
- `int64`: 64-bit signed integer
- `uint`: Platform-dependent, 32 or 64 bits
- `uint8`: 8-bit unsigned integer (0 to 255)
- `uint16`: 16-bit unsigned integer (0 to 65,535)
- `uint32`: 32-bit unsigned integer (0 to 4,294,967,295)
- `uint64`: 64-bit unsigned integer
- `uintptr`: Unsigned integer for storing a pointer

#### Floating-Point Types
- `float32`: 32-bit floating-point
- `float64`: 64-bit floating-point (default and recommended)

#### Complex Types
- `complex64`: Complex numbers with float32 real and imaginary parts
- `complex128`: Complex numbers with float64 real and imaginary parts

### Boolean Type
- `bool`: Represents true or false

### String Type
- `string`: A sequence of characters

### Other Types
- `byte`: Alias for uint8
- `rune`: Alias for int32, represents a Unicode code point

## Zero Values

Variables declared without an explicit initial value are given their zero value:

- Numeric types: `0`
- Boolean type: `false`
- String type: `""` (empty string)
- Pointer types: `nil`

## Constants

Constants are values that cannot be changed during program execution:

```go
const Pi = 3.14159
const (
    StatusOK       = 200
    StatusNotFound = 404
)
```

### iota

Go provides `iota` for creating enumerated constants:

```go
const (
    Sunday = iota    // 0
    Monday           // 1
    Tuesday          // 2
    Wednesday        // 3
    Thursday         // 4
    Friday           // 5
    Saturday         // 6
)
```

## Type Conversion

Go requires explicit type conversion:

```go
var i int = 42
var f float64 = float64(i)
var u uint = uint(f)
```

## Examples

### Basic Variable Usage

```go
package main

import "fmt"

func main() {
    // Variable declaration
    var name string = "John"
    var age int = 30
    
    // Short declaration
    city := "New York"
    
    // Print variables
    fmt.Println("Name:", name)
    fmt.Println("Age:", age)
    fmt.Println("City:", city)
    
    // Update variables
    name = "Jane"
    age = 25
    city = "Boston"
    
    fmt.Println("\nAfter update:")
    fmt.Println("Name:", name)
    fmt.Println("Age:", age)
    fmt.Println("City:", city)
}
```

### Working with Different Data Types

```go
package main

import "fmt"

func main() {
    // Integer types
    var a int = 10
    var b int8 = 127
    var c uint8 = 255
    
    // Floating-point types
    var d float32 = 3.14
    var e float64 = 2.71828
    
    // Boolean type
    var f bool = true
    
    // String type
    var g string = "Hello, Go!"
    
    // Print all variables
    fmt.Println("Integer:", a)
    fmt.Println("Int8:", b)
    fmt.Println("Uint8:", c)
    fmt.Println("Float32:", d)
    fmt.Println("Float64:", e)
    fmt.Println("Boolean:", f)
    fmt.Println("String:", g)
    
    // Type conversion
    var h int = int(d)
    fmt.Println("Float32 to Int:", h)
}
```

## Exercises

1. Declare variables of different types (int, float64, string, bool) and print their values.
2. Create a program that converts temperature from Celsius to Fahrenheit using the formula: F = C * 9/5 + 32.
3. Declare a constant for PI and use it to calculate the area and circumference of a circle with a given radius.
4. Create an enumeration for days of the week using iota.
5. Write a program that demonstrates type conversion between different numeric types.

## Summary

In this tutorial, you've learned:

- Different ways to declare variables in Go
- Basic data types available in Go
- How to work with constants and use iota for enumerations
- Type conversion in Go
- How to use variables in practical examples

Understanding variables and data types is essential for writing effective Go programs. In the next tutorial, we'll explore control structures like if statements, loops, and switches. 