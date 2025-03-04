
# Arrays and Slices in Go

## Introduction

Arrays and slices are fundamental data structures in Go that allow you to store and manipulate collections of values. While they serve similar purposes, they have important differences in terms of flexibility and usage. This tutorial will cover both arrays and slices in detail, explaining when and how to use each one effectively.

## Arrays

An array is a fixed-size sequence of elements of the same type. The size of an array is part of its type, which means that `[5]int` and `[10]int` are different types.

### Declaring and Initializing Arrays

There are several ways to declare and initialize arrays in Go:

#### 1. Declaration with a specific size

```go
var numbers [5]int // Creates an array of 5 integers, initialized to zero values
```

#### 2. Declaration with initialization

```go
var fruits = [3]string{"apple", "banana", "cherry"}
```

#### 3. Using short declaration

```go
colors := [4]string{"red", "green", "blue", "yellow"}
```

#### 4. Let the compiler count the elements

```go
animals := [...]string{"dog", "cat", "bird", "fish"} // The compiler counts the elements
```

### Accessing and Modifying Array Elements

Array elements are accessed using zero-based indexing:

```go
package main

import "fmt"

func main() {
    fruits := [3]string{"apple", "banana", "cherry"}
    
    // Accessing elements
    fmt.Println(fruits[0]) // Output: apple
    fmt.Println(fruits[1]) // Output: banana
    
    // Modifying elements
    fruits[2] = "orange"
    fmt.Println(fruits) // Output: [apple banana orange]
}
```

### Array Length

The built-in `len` function returns the length of an array:

```go
package main

import "fmt"

func main() {
    numbers := [5]int{1, 2, 3, 4, 5}
    fmt.Println("Length:", len(numbers)) // Output: Length: 5
}
```

### Iterating Over Arrays

You can iterate over arrays using a `for` loop:

```go
package main

import "fmt"

func main() {
    numbers := [5]int{1, 2, 3, 4, 5}
    
    // Using a traditional for loop
    for i := 0; i < len(numbers); i++ {
        fmt.Printf("numbers[%d] = %d\n", i, numbers[i])
    }
    
    // Using range
    for index, value := range numbers {
        fmt.Printf("numbers[%d] = %d\n", index, value)
    }
}
```

### Multi-dimensional Arrays

Go supports multi-dimensional arrays:

```go
package main

import "fmt"

func main() {
    // 2D array: 3 rows, 2 columns
    matrix := [3][2]int{
        {1, 2},
        {3, 4},
        {5, 6},
    }
    
    fmt.Println(matrix)
    
    // Accessing elements
    fmt.Println(matrix[1][1]) // Output: 4
    
    // Iterating over a 2D array
    for i := 0; i < len(matrix); i++ {
        for j := 0; j < len(matrix[i]); j++ {
            fmt.Printf("matrix[%d][%d] = %d\n", i, j, matrix[i][j])
        }
    }
}
```

### Arrays as Function Parameters

When you pass an array to a function, Go makes a copy of the array:

```go
package main

import "fmt"

func modifyArray(arr [5]int) {
    arr[0] = 100 // This modification won't affect the original array
    fmt.Println("Inside function:", arr)
}

func main() {
    numbers := [5]int{1, 2, 3, 4, 5}
    
    fmt.Println("Before:", numbers)
    modifyArray(numbers)
    fmt.Println("After:", numbers) // The original array remains unchanged
}
```

## Slices

Slices are more flexible and commonly used than arrays in Go. A slice is a dynamically-sized, flexible view into an array. Unlike arrays, slices are reference types.

### Creating Slices

There are several ways to create slices:

#### 1. Using the make function

```go
numbers := make([]int, 5)       // Creates a slice with length 5, capacity 5
numbers := make([]int, 5, 10)   // Creates a slice with length 5, capacity 10
```

#### 2. Slice literal

```go
fruits := []string{"apple", "banana", "cherry"} // Creates a slice
```

#### 3. Slicing an array or another slice

```go
package main

import "fmt"

func main() {
    // Creating a slice from an array
    array := [5]int{1, 2, 3, 4, 5}
    slice1 := array[1:4] // Elements 1 through 3 (indices are inclusive:exclusive)
    fmt.Println(slice1)  // Output: [2 3 4]
    
    // Creating a slice from another slice
    slice2 := slice1[1:] // From index 1 to the end
    fmt.Println(slice2)  // Output: [3 4]
}
```

### Length and Capacity

Slices have both a length and a capacity:
- Length: the number of elements in the slice
- Capacity: the number of elements in the underlying array, counting from the first element in the slice

```go
package main

import "fmt"

func main() {
    numbers := make([]int, 3, 5)
    fmt.Printf("Length: %d, Capacity: %d\n", len(numbers), cap(numbers))
    // Output: Length: 3, Capacity: 5
    
    // Slicing affects length and capacity
    array := [5]int{1, 2, 3, 4, 5}
    slice := array[1:4]
    fmt.Printf("Length: %d, Capacity: %d\n", len(slice), cap(slice))
    // Output: Length: 3, Capacity: 4
}
```

### Modifying Slices

When you modify a slice, you're modifying the underlying array:

```go
package main

import "fmt"

func main() {
    array := [5]int{1, 2, 3, 4, 5}
    slice := array[1:4]
    
    fmt.Println("Before modification:")
    fmt.Println("Array:", array)
    fmt.Println("Slice:", slice)
    
    // Modify the slice
    slice[0] = 20
    
    fmt.Println("\nAfter modification:")
    fmt.Println("Array:", array) // The array is also modified
    fmt.Println("Slice:", slice)
}
```

### Appending to Slices

The built-in `append` function adds elements to a slice:

```go
package main

import "fmt"

func main() {
    fruits := []string{"apple", "banana"}
    
    // Append one element
    fruits = append(fruits, "cherry")
    fmt.Println(fruits) // Output: [apple banana cherry]
    
    // Append multiple elements
    fruits = append(fruits, "date", "elderberry")
    fmt.Println(fruits) // Output: [apple banana cherry date elderberry]
    
    // Append a slice to another slice
    moreFruits := []string{"fig", "grape"}
    fruits = append(fruits, moreFruits...)
    fmt.Println(fruits) // Output: [apple banana cherry date elderberry fig grape]
}
```

### Slice Capacity Expansion

When appending to a slice, if the capacity is not enough, Go creates a new underlying array with a larger capacity:

```go
package main

import "fmt"

func main() {
    numbers := make([]int, 0, 3)
    fmt.Printf("Length: %d, Capacity: %d\n", len(numbers), cap(numbers))
    
    for i := 1; i <= 5; i++ {
        numbers = append(numbers, i)
        fmt.Printf("After appending %d: Length: %d, Capacity: %d\n", 
                   i, len(numbers), cap(numbers))
    }
}
```

### Copying Slices

The built-in `copy` function copies elements from one slice to another:

```go
package main

import "fmt"

func main() {
    src := []int{1, 2, 3, 4, 5}
    dst := make([]int, 3) // Create a destination slice with length 3
    
    copied := copy(dst, src) // Copy from src to dst
    
    fmt.Println("Source:", src)
    fmt.Println("Destination:", dst)
    fmt.Println("Number of elements copied:", copied)
}
```

### Slices as Function Parameters

When you pass a slice to a function, you're passing a reference to the underlying array:

```go
package main

import "fmt"

func modifySlice(slice []int) {
    slice[0] = 100 // This modification affects the original slice
    fmt.Println("Inside function:", slice)
}

func main() {
    numbers := []int{1, 2, 3, 4, 5}
    
    fmt.Println("Before:", numbers)
    modifySlice(numbers)
    fmt.Println("After:", numbers) // The original slice is modified
}
```

## Nil Slices

A nil slice has a length and capacity of 0 and has no underlying array:

```go
package main

import "fmt"

func main() {
    var slice []int // Declares a nil slice
    
    fmt.Println(slice)
    fmt.Println(len(slice))
    fmt.Println(cap(slice))
    fmt.Println(slice == nil) // Output: true
    
    // You can append to a nil slice
    slice = append(slice, 1, 2, 3)
    fmt.Println(slice) // Output: [1 2 3]
}
```

## Empty Slices

An empty slice has a length and capacity of 0 but has an underlying array:

```go
package main

import "fmt"

func main() {
    slice := []int{} // Creates an empty slice
    
    fmt.Println(slice)
    fmt.Println(len(slice))
    fmt.Println(cap(slice))
    fmt.Println(slice == nil) // Output: false
}
```

## Examples

### Example 1: Working with Slices

```go
package main

import "fmt"

func main() {
    // Create a slice
    numbers := []int{1, 2, 3, 4, 5}
    
    // Print the slice
    fmt.Println("Original slice:", numbers)
    
    // Append elements
    numbers = append(numbers, 6, 7, 8)
    fmt.Println("After appending:", numbers)
    
    // Slice the slice
    slice1 := numbers[2:5]
    fmt.Println("Slice from index 2 to 4:", slice1)
    
    // Modify the slice
    slice1[0] = 30
    fmt.Println("After modifying slice1:", slice1)
    fmt.Println("Original slice (also modified):", numbers)
    
    // Create a copy
    copySlice := make([]int, len(numbers))
    copy(copySlice, numbers)
    
    // Modify the copy
    copySlice[0] = 100
    fmt.Println("Copy after modification:", copySlice)
    fmt.Println("Original remains unchanged:", numbers)
}
```

### Example 2: Implementing a Stack with a Slice

```go
package main

import "fmt"

// Stack represents a stack data structure
type Stack struct {
    items []int
}

// Push adds an item to the top of the stack
func (s *Stack) Push(item int) {
    s.items = append(s.items, item)
}

// Pop removes and returns the top item from the stack
func (s *Stack) Pop() (int, bool) {
    if len(s.items) == 0 {
        return 0, false
    }
    
    index := len(s.items) - 1
    item := s.items[index]
    s.items = s.items[:index]
    return item, true
}

// Peek returns the top item without removing it
func (s *Stack) Peek() (int, bool) {
    if len(s.items) == 0 {
        return 0, false
    }
    
    return s.items[len(s.items)-1], true
}

// IsEmpty returns true if the stack is empty
func (s *Stack) IsEmpty() bool {
    return len(s.items) == 0
}

// Size returns the number of items in the stack
func (s *Stack) Size() int {
    return len(s.items)
}

func main() {
    stack := Stack{}
    
    // Push items onto the stack
    stack.Push(1)
    stack.Push(2)
    stack.Push(3)
    
    fmt.Println("Stack size:", stack.Size())
    
    // Peek at the top item
    if item, ok := stack.Peek(); ok {
        fmt.Println("Top item:", item)
    }
    
    // Pop items from the stack
    for !stack.IsEmpty() {
        if item, ok := stack.Pop(); ok {
            fmt.Println("Popped:", item)
        }
    }
    
    fmt.Println("Stack is empty:", stack.IsEmpty())
}
```

## Exercises

1. Write a function that takes a slice of integers and returns a new slice containing only the even numbers.

2. Create a function that reverses a slice in place (without creating a new slice).

3. Implement a function that merges two sorted slices into a single sorted slice.

4. Write a function that removes duplicates from a slice while preserving the original order.

5. Create a function that rotates the elements of a slice by a given number of positions.

## Summary

In this tutorial, you've learned about arrays and slices in Go:

- Arrays are fixed-size collections of elements of the same type
- Slices are flexible, dynamic views of arrays
- How to create, access, and modify arrays and slices
- The concepts of length and capacity for slices
- How to use built-in functions like `append` and `copy`
- The difference between nil slices and empty slices
- Practical examples of working with slices

Slices are one of the most commonly used data structures in Go due to their flexibility. Understanding how to work with slices effectively is essential for Go programming. In the next tutorial, we'll explore maps in Go.