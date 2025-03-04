# Methods and Interfaces in Go

## Introduction

Methods and interfaces are fundamental concepts in Go that enable object-oriented programming patterns. Methods allow you to associate functions with specific types, while interfaces define behavior that types can implement. Together, they provide a powerful way to create flexible, modular, and reusable code.

This tutorial will cover methods and interfaces in Go, explaining how they work and demonstrating their practical applications.

## Methods

A method is a function that is associated with a particular type. It has a special receiver argument that appears between the `func` keyword and the method name.

### Defining Methods

```go
package main

import (
    "fmt"
    "math"
)

// Define a type
type Circle struct {
    Radius float64
}

// Method with a receiver of type Circle
func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

// Method with a receiver of type Circle
func (c Circle) Circumference() float64 {
    return 2 * math.Pi * c.Radius
}

func main() {
    c := Circle{Radius: 5}
    
    fmt.Printf("Circle with radius %.2f\n", c.Radius)
    fmt.Printf("Area: %.2f\n", c.Area())
    fmt.Printf("Circumference: %.2f\n", c.Circumference())
}
```

In this example, `Area()` and `Circumference()` are methods of the `Circle` type. The receiver `c` is like the `this` or `self` reference in other languages.

### Value Receivers vs. Pointer Receivers

Methods can have either value receivers or pointer receivers:

#### Value

When a method has a value receiver, it operates on a copy of the value:

```go
func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}
```

#### Pointer

When a method has a pointer receiver, it operates on the actual value:

```go
func (c *Circle) SetRadius(r float64) {
    c.Radius = r
}
```

Here's a complete example showing both types of receivers:

```go
package main

import (
    "fmt"
    "math"
)

type Circle struct {
    Radius float64
}

// Value receiver - doesn't modify the original Circle
func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

// Value receiver - doesn't modify the original Circle
func (c Circle) Circumference() float64 {
    return 2 * math.Pi * c.Radius
}

// Pointer receiver - modifies the original Circle
func (c *Circle) SetRadius(r float64) {
    c.Radius = r
}

// Value receiver that tries to modify the receiver (won't affect the original)
func (c Circle) TryToSetRadius(r float64) {
    c.Radius = r // This only modifies the copy, not the original
}

func main() {
    c := Circle{Radius: 5}
    fmt.Printf("Initial radius: %.2f\n", c.Radius)
    
    // Call methods with value receiver
    area := c.Area()
    circumference := c.Circumference()
    fmt.Printf("Area: %.2f\n", area)
    fmt.Printf("Circumference: %.2f\n", circumference)
    
    // Try to modify using a value receiver method
    c.TryToSetRadius(10)
    fmt.Printf("After TryToSetRadius: %.2f\n", c.Radius) // Still 5
    
    // Modify using a pointer receiver method
    c.SetRadius(10) // Go automatically converts c to &c when needed
    fmt.Printf("After SetRadius: %.2f\n", c.Radius) // Now 10
    
    // Calculate with the new radius
    fmt.Printf("New area: %.2f\n", c.Area())
    fmt.Printf("New circumference: %.2f\n", c.Circumference())
}
```

### When to Use Pointer Receivers

Use pointer receivers when:

1. You need to modify the receiver
2. The receiver is a large struct or array (to avoid copying)
3. Consistency: if some methods need pointer receivers, consider using pointer receivers for all methods of that type

### Methods on Non-Struct Types

In Go, you can define methods on any type that you define, not just structs:

```go
package main

import (
    "fmt"
    "strings"
)

// Define a new type based on a built-in type
type MyString string

// Method for MyString type
func (s MyString) Uppercase() string {
    return strings.ToUpper(string(s))
}

// Method for MyString type
func (s MyString) Lowercase() string {
    return strings.ToLower(string(s))
}

func main() {
    var name MyString = "John Doe"
    
    fmt.Println("Original:", name)
    fmt.Println("Uppercase:", name.Uppercase())
    fmt.Println("Lowercase:", name.Lowercase())
}
```

Note: You cannot define methods on types from other packages directly (including built-in types). You need to create a new type based on them first, as shown above.

## Interfaces

An interface is a type that defines a set of methods. A type implements an interface by implementing its methods. In Go, interfaces are implemented implicitly - there's no explicit declaration of intent.

### Defining Interfaces

```go
package main

import (
    "fmt"
    "math"
)

// Define an interface
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Circle type
type Circle struct {
    Radius float64
}

// Implement methods for Circle
func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

// Rectangle type
type Rectangle struct {
    Width, Height float64
}

// Implement methods for Rectangle
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Function that works with any Shape
func PrintShapeInfo(s Shape) {
    fmt.Printf("Area: %.2f\n", s.Area())
    fmt.Printf("Perimeter: %.2f\n", s.Perimeter())
}

func main() {
    c := Circle{Radius: 5}
    r := Rectangle{Width: 4, Height: 6}
    
    fmt.Println("Circle:")
    PrintShapeInfo(c)
    
    fmt.Println("\nRectangle:")
    PrintShapeInfo(r)
}
```

In this example:
- `Shape` is an interface that defines two methods: `Area()` and `Perimeter()`
- Both `Circle` and `Rectangle` implement these methods, so they implicitly implement the `Shape` interface
- `PrintShapeInfo()` can accept any type that implements the `Shape` interface

### Interface Values

An interface value consists of a concrete value and a dynamic type. The zero value of an interface is `nil` (both value and type are `nil`).

```go
package main

import "fmt"

type Speaker interface {
    Speak() string
}

type Dog struct {
    Name string
}

func (d Dog) Speak() string {
    return d.Name + " says Woof!"
}

type Cat struct {
    Name string
}

func (c Cat) Speak() string {
    return c.Name + " says Meow!"
}

func main() {
    // Interface values
    var s Speaker
    
    fmt.Printf("Value: %v, Type: %T\n", s, s) // Value: <nil>, Type: <nil>
    
    // Assign a Dog to the interface
    s = Dog{Name: "Rex"}
    fmt.Printf("Value: %v, Type: %T\n", s, s) // Value: {Rex}, Type: main.Dog
    fmt.Println(s.Speak()) // Rex says Woof!
    
    // Assign a Cat to the interface
    s = Cat{Name: "Fluffy"}
    fmt.Printf("Value: %v, Type: %T\n", s, s) // Value: {Fluffy}, Type: main.Cat
    fmt.Println(s.Speak()) // Fluffy says Meow!
}
```

### Empty Interface

The empty interface `interface{}` (or `any` in Go 1.18+) has no methods, so all types implement it. It's used when you need to accept any type:

```go
package main

import "fmt"

func PrintAny(v interface{}) {
    fmt.Printf("Value: %v, Type: %T\n", v, v)
}

func main() {
    PrintAny(42)          // Value: 42, Type: int
    PrintAny("hello")     // Value: hello, Type: string
    PrintAny(true)        // Value: true, Type: bool
    PrintAny([]int{1, 2}) // Value: [1 2], Type: []int
}
```

### Type Assertions

A type assertion provides access to an interface value's underlying concrete value:

```go
package main

import "fmt"

func main() {
    var i interface{} = "hello"
    
    // Type assertion
    s, ok := i.(string)
    fmt.Println(s, ok) // hello true
    
    // Failed type assertion
    n, ok := i.(int)
    fmt.Println(n, ok) // 0 false
    
    // Type assertion without check (will panic if wrong type)
    // s = i.(string) // This is safe
    // n = i.(int)    // This would panic
}
```

### Type Switches

A type switch performs several type assertions in series:

```go
package main

import "fmt"

func describe(i interface{}) {
    switch v := i.(type) {
    case int:
        fmt.Printf("Integer: %d\n", v)
    case string:
        fmt.Printf("String: %s\n", v)
    case bool:
        fmt.Printf("Boolean: %v\n", v)
    default:
        fmt.Printf("Unknown type: %T\n", v)
    }
}

func main() {
    describe(42)
    describe("hello")
    describe(true)
    describe([]int{1, 2, 3})
}
```

### Interface Composition

Interfaces can be composed of other interfaces:

```go
package main

import "fmt"

type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// ReadWriter is the composition of Reader and Writer
type ReadWriter interface {
    Reader
    Writer
}

// A concrete type implementing ReadWriter
type Buffer struct {
    data []byte
}

func (b *Buffer) Read(p []byte) (n int, err error) {
    n = copy(p, b.data)
    return n, nil
}

func (b *Buffer) Write(p []byte) (n int, err error) {
    b.data = append(b.data, p...)
    return len(p), nil
}

func main() {
    var rw ReadWriter = &Buffer{}
    
    // Write data
    data := []byte("Hello, Go!")
    n, _ := rw.Write(data)
    fmt.Printf("Wrote %d bytes\n", n)
    
    // Read data
    buf := make([]byte, 100)
    n, _ = rw.Read(buf)
    fmt.Printf("Read %d bytes: %s\n", n, buf[:n])
}
```

## Common Interfaces in Go Standard Library

Go's standard library defines several useful interfaces:

### io.Reader and io.Writer

```go
package main

import (
    "fmt"
    "io"
    "os"
    "strings"
)

func main() {
    // Using strings.Reader as an io.Reader
    r := strings.NewReader("Hello, Reader!")
    
    // Read from the reader
    buf := make([]byte, 8)
    for {
        n, err := r.Read(buf)
        fmt.Printf("n = %d err = %v buf = %v\n", n, err, buf[:n])
        fmt.Printf("buf[:n] = %q\n", buf[:n])
        if err == io.EOF {
            break
        }
    }
    
    // Using os.Stdout as an io.Writer
    w := os.Stdout
    fmt.Fprintln(w, "\nHello, Writer!")
}
```

### fmt.Stringer

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}

// Implement the fmt.Stringer interface
func (p Person) String() string {
    return fmt.Sprintf("%s (%d years)", p.Name, p.Age)
}

func main() {
    p := Person{"John Doe", 30}
    fmt.Println(p) // Calls p.String()
}
```

### sort.Interface

```go
package main

import (
    "fmt"
    "sort"
)

type ByLength []string

func (s ByLength) Len() int {
    return len(s)
}

func (s ByLength) Swap(i, j int) {
    s[i], s[j] = s[j], s[i]
}

func (s ByLength) Less(i, j int) bool {
    return len(s[i]) < len(s[j])
}

func main() {
    fruits := []string{"peach", "banana", "kiwi", "apple", "blueberry"}
    fmt.Println("Original:", fruits)
    
    // Sort by string length
    sort.Sort(ByLength(fruits))
    fmt.Println("Sorted by length:", fruits)
    
    // Sort in reverse order
    sort.Sort(sort.Reverse(ByLength(fruits)))
    fmt.Println("Reverse sorted by length:", fruits)
}
```

## Examples

### Example 1: Payment Processing System

```go
package main

import "fmt"

// PaymentMethod interface
type PaymentMethod interface {
    ProcessPayment(amount float64) bool
    GetName() string
}

// CreditCard type
type CreditCard struct {
    Name       string
    Number     string
    CVV        string
    ExpireDate string
}

func (c CreditCard) ProcessPayment(amount float64) bool {
    // In a real application, this would connect to a payment gateway
    fmt.Printf("Processing $%.2f payment with credit card %s\n", amount, c.Number)
    return true
}

func (c CreditCard) GetName() string {
    return "Credit Card"
}

// PayPal type
type PayPal struct {
    Email    string
    Password string
}

func (p PayPal) ProcessPayment(amount float64) bool {
    // In a real application, this would connect to PayPal API
    fmt.Printf("Processing $%.2f payment with PayPal account %s\n", amount, p.Email)
    return true
}

func (p PayPal) GetName() string {
    return "PayPal"
}

// BankTransfer type
type BankTransfer struct {
    AccountNumber string
    BankCode      string
}

func (b BankTransfer) ProcessPayment(amount float64) bool {
    // In a real application, this would connect to bank API
    fmt.Printf("Processing $%.2f payment with bank transfer to account %s\n", amount, b.AccountNumber)
    return true
}

func (b BankTransfer) GetName() string {
    return "Bank Transfer"
}

// Order type
type Order struct {
    ID            string
    Items         []string
    TotalAmount   float64
    PaymentMethod PaymentMethod
}

// Process the order payment
func (o *Order) Checkout() bool {
    fmt.Printf("Checking out order %s for $%.2f using %s\n", 
               o.ID, o.TotalAmount, o.PaymentMethod.GetName())
    return o.PaymentMethod.ProcessPayment(o.TotalAmount)
}

func main() {
    // Create payment methods
    creditCard := CreditCard{
        Name:       "John Doe",
        Number:     "4111-1111-1111-1111",
        CVV:        "123",
        ExpireDate: "12/25",
    }
    
    paypal := PayPal{
        Email:    "john.doe@example.com",
        Password: "password123",
    }
    
    bankTransfer := BankTransfer{
        AccountNumber: "123456789",
        BankCode:      "ABCDEF",
    }
    
    // Create orders with different payment methods
    orders := []Order{
        {
            ID:            "ORD-001",
            Items:         []string{"Laptop", "Mouse"},
            TotalAmount:   1299.99,
            PaymentMethod: creditCard,
        },
        {
            ID:            "ORD-002",
            Items:         []string{"Headphones"},
            TotalAmount:   199.99,
            PaymentMethod: paypal,
        },
        {
            ID:            "ORD-003",
            Items:         []string{"Monitor", "Keyboard"},
            TotalAmount:   499.99,
            PaymentMethod: bankTransfer,
        },
    }
    
    // Process all orders
    for _, order := range orders {
        fmt.Println("\n--- Processing Order ---")
        success := order.Checkout()
        if success {
            fmt.Println("Payment successful!")
        } else {
            fmt.Println("Payment failed!")
        }
    }
}
```

### Example 2: File Compression System

```go
package main

import (
    "fmt"
    "strings"
)

// Compressor interface
type Compressor interface {
    Compress(data string) string
    Decompress(data string) string
    GetName() string
}

// GzipCompressor type
type GzipCompressor struct{}

func (g GzipCompressor) Compress(data string) string {
    // Simplified implementation for demonstration
    return "gzip:" + data[:len(data)/2] + "..."
}

func (g GzipCompressor) Decompress(data string) string {
    // Simplified implementation for demonstration
    return strings.TrimPrefix(data, "gzip:") + "..." + "decompressed"
}

func (g GzipCompressor) GetName() string {
    return "Gzip"
}

// ZipCompressor type
type ZipCompressor struct{}

func (z ZipCompressor) Compress(data string) string {
    // Simplified implementation for demonstration
    return "zip:" + data[:len(data)/3] + "..."
}

func (z ZipCompressor) Decompress(data string) string {
    // Simplified implementation for demonstration
    return strings.TrimPrefix(data, "zip:") + "..." + "decompressed"
}

func (z ZipCompressor) GetName() string {
    return "Zip"
}

// File type
type File struct {
    Name       string
    Content    string
    Compressed bool
    Data       string
}

// CompressFile compresses a file using the specified compressor
func CompressFile(file *File, compressor Compressor) {
    if file.Compressed {
        fmt.Printf("File %s is already compressed\n", file.Name)
        return
    }
    
    file.Data = compressor.Compress(file.Content)
    file.Compressed = true
    
    fmt.Printf("Compressed file %s using %s\n", file.Name, compressor.GetName())
}

// DecompressFile decompresses a file using the specified compressor
func DecompressFile(file *File, compressor Compressor) {
    if !file.Compressed {
        fmt.Printf("File %s is not compressed\n", file.Name)
        return
    }
    
    file.Content = compressor.Decompress(file.Data)
    file.Compressed = false
    
    fmt.Printf("Decompressed file %s using %s\n", file.Name, compressor.GetName())
}

func main() {
    // Create compressors
    gzip := GzipCompressor{}
    zip := ZipCompressor{}
    
    // Create a file
    file := &File{
        Name:       "document.txt",
        Content:    "This is a sample document with some content that we want to compress.",
        Compressed: false,
    }
    
    // Print original file
    fmt.Printf("Original file: %s\nContent: %s\n\n", file.Name, file.Content)
    
    // Compress the file using Gzip
    CompressFile(file, gzip)
    fmt.Printf("Compressed data: %s\n\n", file.Data)
    
    // Decompress the file
    DecompressFile(file, gzip)
    fmt.Printf("Decompressed content: %s\n\n", file.Content)
    
    // Compress the file using Zip
    CompressFile(file, zip)
    fmt.Printf("Compressed data: %s\n\n", file.Data)
    
    // Decompress the file
    DecompressFile(file, zip)
    fmt.Printf("Decompressed content: %s\n", file.Content)
}
```

## Exercises

1. Create a `Logger` interface with methods for logging at different levels (Info, Warning, Error). Implement this interface with at least two different types: `ConsoleLogger` and `FileLogger`.

2. Design a `Vehicle` interface with methods like `Start()`, `Stop()`, and `GetFuelLevel()`. Implement this interface for different vehicle types like `Car`, `Motorcycle`, and `Truck`.

3. Create a `Sorter` interface that defines methods for sorting a collection. Implement this interface for different sorting algorithms like bubble sort, insertion sort, and quick sort.

4. Design a `Cache` interface with methods for storing, retrieving, and invalidating cached items. Implement this interface with different caching strategies like LRU (Least Recently Used) and FIFO (First In, First Out).

5. Create a `Notifier` interface with a method for sending notifications. Implement this interface for different notification channels like email, SMS, and push notifications.

## Summary

In this tutorial, you've learned about methods and interfaces in Go:

- Methods are functions associated with a particular type
- Value receivers vs. pointer receivers
- Interfaces define a set of methods that types can implement
- Implicit interface implementation
- Interface values and the empty interface
- Type assertions and type switches
- Interface composition
- Common interfaces in the Go standard library
- Practical examples of using methods and interfaces

Methods and interfaces are powerful features in Go that enable object-oriented programming patterns while maintaining Go's simplicity and efficiency. They allow you to create flexible, modular, and reusable code. In the next tutorial, we'll explore error handling in Go.