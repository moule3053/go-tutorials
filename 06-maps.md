# Maps in Go

## Introduction

Maps are one of Go's built-in data structures that associate keys with values, similar to dictionaries or hash tables in other programming languages. Maps provide an efficient way to look up values based on keys, making them ideal for scenarios where you need fast lookups, unique associations, or dynamic collections of key-value pairs.

This tutorial will cover how to create, manipulate, and use maps effectively in Go.

## What is a Map?

A map is an unordered collection of key-value pairs. Each key in a map must be unique, and it is used to retrieve the corresponding value. In Go, maps are reference types, which means when you assign a map to a new variable or pass it to a function, they refer to the same underlying data structure.

## Creating Maps

There are several ways to create maps in Go:

### 1. Using the make function

```go
// Syntax: make(map[KeyType]ValueType)
scores := make(map[string]int)
```

### 2. Map literal

```go
// Empty map
colors := map[string]string{}

// Map with initial values
countries := map[string]string{
    "US": "United States",
    "GB": "United Kingdom",
    "FR": "France",
    "DE": "Germany",
}
```

### 3. Nil map

```go
var ages map[string]int // Declares a nil map
```

Note: A nil map has no keys and cannot be assigned to. You must use `make` or a map literal before adding key-value pairs.

## Adding and Updating Elements

Adding or updating elements in a map is straightforward:

```go
package main

import "fmt"

func main() {
    // Create a map
    users := make(map[string]int)
    
    // Add elements
    users["Alice"] = 25
    users["Bob"] = 30
    users["Charlie"] = 35
    
    fmt.Println(users) // Output: map[Alice:25 Bob:30 Charlie:35]
    
    // Update an element
    users["Alice"] = 26
    fmt.Println(users) // Output: map[Alice:26 Bob:30 Charlie:35]
}
```

## Accessing Elements

You can access map elements using the key:

```go
package main

import "fmt"

func main() {
    users := map[string]int{
        "Alice":   26,
        "Bob":     30,
        "Charlie": 35,
    }
    
    // Access an element
    aliceAge := users["Alice"]
    fmt.Println("Alice's age:", aliceAge) // Output: Alice's age: 26
    
    // Accessing a non-existent key returns the zero value for the value type
    daveAge := users["Dave"]
    fmt.Println("Dave's age:", daveAge) // Output: Dave's age: 0
}
```

### Checking if a Key Exists

When accessing a map, you can check if a key exists using the "comma ok" idiom:

```go
package main

import "fmt"

func main() {
    users := map[string]int{
        "Alice":   26,
        "Bob":     30,
        "Charlie": 35,
    }
    
    // Check if a key exists
    age, exists := users["Alice"]
    if exists {
        fmt.Println("Alice's age is", age)
    } else {
        fmt.Println("Alice not found")
    }
    
    // Check for a non-existent key
    age, exists = users["Dave"]
    if exists {
        fmt.Println("Dave's age is", age)
    } else {
        fmt.Println("Dave not found")
    }
}
```

## Deleting Elements

You can remove key-value pairs from a map using the `delete` function:

```go
package main

import "fmt"

func main() {
    users := map[string]int{
        "Alice":   26,
        "Bob":     30,
        "Charlie": 35,
    }
    
    fmt.Println("Before deletion:", users)
    
    // Delete an element
    delete(users, "Bob")
    fmt.Println("After deletion:", users)
    
    // Deleting a non-existent key is a no-op (no error)
    delete(users, "Dave")
    fmt.Println("After deleting non-existent key:", users)
}
```

## Iterating Over Maps

You can use the `range` keyword to iterate over a map:

```go
package main

import "fmt"

func main() {
    countries := map[string]string{
        "US": "United States",
        "GB": "United Kingdom",
        "FR": "France",
        "DE": "Germany",
    }
    
    // Iterate over the map
    for key, value := range countries {
        fmt.Printf("Code: %s, Country: %s\n", key, value)
    }
    
    // Iterate over just the keys
    fmt.Println("\nCountry codes:")
    for key := range countries {
        fmt.Println(key)
    }
    
    // Iterate over just the values
    fmt.Println("\nCountry names:")
    for _, value := range countries {
        fmt.Println(value)
    }
}
```

Note: The iteration order of a map is not guaranteed. The order can vary between iterations and between different runs of the program.

## Map Length

You can find the number of key-value pairs in a map using the `len` function:

```go
package main

import "fmt"

func main() {
    users := map[string]int{
        "Alice":   26,
        "Bob":     30,
        "Charlie": 35,
    }
    
    fmt.Println("Number of users:", len(users)) // Output: Number of users: 3
    
    delete(users, "Bob")
    fmt.Println("Number of users after deletion:", len(users)) // Output: Number of users after deletion: 2
}
```

## Maps as Function Parameters

When you pass a map to a function, you're passing a reference to the map. This means that any changes made to the map inside the function will affect the original map:

```go
package main

import "fmt"

func addUser(users map[string]int, name string, age int) {
    users[name] = age
}

func main() {
    users := map[string]int{
        "Alice": 26,
        "Bob":   30,
    }
    
    fmt.Println("Before:", users)
    
    addUser(users, "Charlie", 35)
    
    fmt.Println("After:", users) // The original map is modified
}
```

## Maps with Slices and Structs

Maps can have complex value types, such as slices or structs:

### Map with Slices

```go
package main

import "fmt"

func main() {
    // Map with slices as values
    userHobbies := map[string][]string{
        "Alice":   {"Reading", "Hiking", "Photography"},
        "Bob":     {"Gaming", "Cooking"},
        "Charlie": {"Swimming", "Running", "Cycling"},
    }
    
    fmt.Println("Bob's hobbies:", userHobbies["Bob"])
    
    // Add a hobby for Alice
    userHobbies["Alice"] = append(userHobbies["Alice"], "Painting")
    fmt.Println("Alice's updated hobbies:", userHobbies["Alice"])
}
```

### Map with Structs

```go
package main

import "fmt"

type Person struct {
    Name    string
    Age     int
    Address string
}

func main() {
    // Map with structs as values
    people := map[string]Person{
        "alice": {
            Name:    "Alice Smith",
            Age:     26,
            Address: "123 Main St",
        },
        "bob": {
            Name:    "Bob Johnson",
            Age:     30,
            Address: "456 Oak Ave",
        },
    }
    
    fmt.Println("Alice's details:", people["alice"])
    
    // Update Bob's address
    bobRecord := people["bob"]
    bobRecord.Address = "789 Pine Rd"
    people["bob"] = bobRecord
    
    fmt.Println("Bob's updated details:", people["bob"])
}
```

## Comparing Maps

Maps cannot be compared directly with the `==` operator, except to check if they are `nil`:

```go
package main

import (
    "fmt"
    "reflect"
)

func main() {
    map1 := map[string]int{"a": 1, "b": 2}
    map2 := map[string]int{"a": 1, "b": 2}
    
    // This will not compile:
    // fmt.Println(map1 == map2)
    
    // Instead, you can compare maps by checking if they have the same keys and values
    fmt.Println("Maps are equal:", reflect.DeepEqual(map1, map2))
    
    // Or you can write your own comparison function
    fmt.Println("Maps are equal:", mapsEqual(map1, map2))
    
    // You can compare a map to nil
    var nilMap map[string]int
    fmt.Println("map1 is nil:", map1 == nil)
    fmt.Println("nilMap is nil:", nilMap == nil)
}

// Custom function to compare maps
func mapsEqual(map1, map2 map[string]int) bool {
    if len(map1) != len(map2) {
        return false
    }
    
    for key, val1 := range map1 {
        val2, exists := map2[key]
        if !exists || val1 != val2 {
            return false
        }
    }
    
    return true
}
```

## Examples

### Example 1: Word Frequency Counter

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    text := "Go is an open source programming language that makes it easy to build simple reliable and efficient software Go is expressive concise clean and efficient"
    
    // Split the text into words
    words := strings.Fields(text)
    
    // Count the frequency of each word
    frequency := make(map[string]int)
    for _, word := range words {
        word = strings.ToLower(word)
        frequency[word]++
    }
    
    // Print the word frequencies
    fmt.Println("Word frequencies:")
    for word, count := range frequency {
        fmt.Printf("%s: %d\n", word, count)
    }
    
    // Find the most frequent word
    mostFrequentWord := ""
    highestCount := 0
    
    for word, count := range frequency {
        if count > highestCount {
            highestCount = count
            mostFrequentWord = word
        }
    }
    
    fmt.Printf("\nMost frequent word: '%s' with %d occurrences\n", 
               mostFrequentWord, highestCount)
}
```

### Example 2: Student Grade Tracker

```go
package main

import "fmt"

func main() {
    // Map to store student grades
    studentGrades := map[string]map[string]int{
        "Alice": {
            "Math":    90,
            "Science": 85,
            "English": 95,
        },
        "Bob": {
            "Math":    80,
            "Science": 90,
            "English": 75,
        },
        "Charlie": {
            "Math":    70,
            "Science": 65,
            "English": 80,
        },
    }
    
    // Print all student grades
    fmt.Println("Student Grades:")
    for student, grades := range studentGrades {
        fmt.Printf("%s:\n", student)
        
        total := 0
        count := 0
        
        for subject, grade := range grades {
            fmt.Printf("  %s: %d\n", subject, grade)
            total += grade
            count++
        }
        
        average := float64(total) / float64(count)
        fmt.Printf("  Average: %.2f\n\n", average)
    }
    
    // Add a new grade for Alice
    studentGrades["Alice"]["History"] = 88
    
    // Add a new student
    studentGrades["Dave"] = map[string]int{
        "Math":    75,
        "Science": 80,
        "English": 85,
    }
    
    // Print updated grades for Alice
    fmt.Println("Alice's updated grades:")
    for subject, grade := range studentGrades["Alice"] {
        fmt.Printf("  %s: %d\n", subject, grade)
    }
}
```

## Exercises

1. Create a program that reads a string and counts the frequency of each character, storing the results in a map.

2. Write a function that takes two maps and returns a new map containing only the key-value pairs that appear in both maps.

3. Implement a simple address book using maps, where you can add, update, delete, and search for contacts.

4. Create a function that takes a slice of strings and returns a map where the keys are the strings and the values are the number of times each string appears in the slice.

5. Write a program that simulates a voting system, where users can vote for candidates and the program keeps track of the vote count for each candidate.

## Summary

In this tutorial, you've learned about maps in Go:

- How to create maps using `make`, map literals, and nil maps
- Adding, accessing, and deleting elements from maps
- Checking if a key exists in a map
- Iterating over maps using the `range` keyword
- Using maps with complex value types like slices and structs
- Comparing maps
- Practical examples of using maps

Maps are a powerful and flexible data structure in Go that allow you to associate keys with values. They are commonly used for lookups, counting, grouping, and many other tasks that require key-value associations. Understanding how to work with maps effectively is essential for Go programming. In the next tutorial, we'll explore structs in Go.