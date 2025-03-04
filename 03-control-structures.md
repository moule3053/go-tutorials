# Control Structures in Go

## Introduction

Control structures are fundamental components of any programming language that allow you to control the flow of your program's execution. Go provides a clean and straightforward set of control structures including conditionals, loops, and switches. This tutorial will cover these control structures in detail with examples.

## Conditional Statements

### If Statement

The basic `if` statement in Go looks like this:

```go
if condition {
    // code to execute if condition is true
}
```

Unlike many other languages, the condition doesn't need to be surrounded by parentheses, but the braces `{}` are required.

Example:

```go
package main

import "fmt"

func main() {
    age := 18
    
    if age >= 18 {
        fmt.Println("You are an adult")
    }
}
```

### If-Else Statement

You can add an `else` clause to execute code when the condition is false:

```go
if condition {
    // code to execute if condition is true
} else {
    // code to execute if condition is false
}
```

Example:

```go
package main

import "fmt"

func main() {
    age := 16
    
    if age >= 18 {
        fmt.Println("You are an adult")
    } else {
        fmt.Println("You are a minor")
    }
}
```

### If-Else If-Else Statement

For multiple conditions, you can use `else if`:

```go
if condition1 {
    // code to execute if condition1 is true
} else if condition2 {
    // code to execute if condition2 is true
} else {
    // code to execute if all conditions are false
}
```

Example:

```go
package main

import "fmt"

func main() {
    score := 85
    
    if score >= 90 {
        fmt.Println("Grade: A")
    } else if score >= 80 {
        fmt.Println("Grade: B")
    } else if score >= 70 {
        fmt.Println("Grade: C")
    } else if score >= 60 {
        fmt.Println("Grade: D")
    } else {
        fmt.Println("Grade: F")
    }
}
```

### If with a Short Statement

Go allows you to execute a short statement before the condition:

```go
if initialization; condition {
    // code to execute if condition is true
}
```

Example:

```go
package main

import "fmt"

func main() {
    if num := 9; num < 10 {
        fmt.Println(num, "is less than 10")
    }
    // num is not accessible here
}
```

## Loops

### For Loop

Go has only one looping construct: the `for` loop. However, it can be used in several ways:

#### Standard For Loop

```go
for initialization; condition; post {
    // code to execute in each iteration
}
```

Example:

```go
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println("Iteration:", i)
    }
}
```

#### For as a While Loop

```go
for condition {
    // code to execute while condition is true
}
```

Example:

```go
package main

import "fmt"

func main() {
    i := 0
    for i < 5 {
        fmt.Println("Value:", i)
        i++
    }
}
```

#### Infinite Loop

```go
for {
    // code to execute indefinitely
    // use break to exit the loop
}
```

Example:

```go
package main

import "fmt"

func main() {
    i := 0
    for {
        fmt.Println("Iteration:", i)
        i++
        if i >= 5 {
            break
        }
    }
}
```

### Range

The `range` form of the for loop iterates over a slice, array, string, map, or channel:

```go
for index, value := range collection {
    // code to execute for each element
}
```

Examples:

```go
package main

import "fmt"

func main() {
    // Iterating over a slice
    numbers := []int{1, 2, 3, 4, 5}
    for i, num := range numbers {
        fmt.Printf("Index: %d, Value: %d\n", i, num)
    }
    
    // Iterating over a string
    for i, char := range "hello" {
        fmt.Printf("Index: %d, Character: %c\n", i, char)
    }
    
    // Iterating over a map
    capitals := map[string]string{
        "France": "Paris",
        "Italy":  "Rome",
        "Japan":  "Tokyo",
    }
    for country, capital := range capitals {
        fmt.Printf("The capital of %s is %s\n", country, capital)
    }
}
```

### Break and Continue

- `break`: Exits the innermost loop
- `continue`: Skips to the next iteration of the loop

Example:

```go
package main

import "fmt"

func main() {
    // Using break
    fmt.Println("Break example:")
    for i := 0; i < 10; i++ {
        if i == 5 {
            break
        }
        fmt.Println(i)
    }
    
    // Using continue
    fmt.Println("\nContinue example:")
    for i := 0; i < 10; i++ {
        if i%2 == 0 {
            continue
        }
        fmt.Println(i)
    }
}
```

## Switch Statement

The `switch` statement is a cleaner way to write multiple if-else statements:

### Basic Switch

```go
switch expression {
case value1:
    // code to execute if expression == value1
case value2:
    // code to execute if expression == value2
default:
    // code to execute if no case matches
}
```

Example:

```go
package main

import "fmt"

func main() {
    day := "Tuesday"
    
    switch day {
    case "Monday":
        fmt.Println("Start of the work week")
    case "Tuesday", "Wednesday", "Thursday":
        fmt.Println("Midweek")
    case "Friday":
        fmt.Println("End of the work week")
    case "Saturday", "Sunday":
        fmt.Println("Weekend")
    default:
        fmt.Println("Invalid day")
    }
}
```

### Switch with No Expression

```go
switch {
case condition1:
    // code to execute if condition1 is true
case condition2:
    // code to execute if condition2 is true
default:
    // code to execute if no case matches
}
```

Example:

```go
package main

import "fmt"

func main() {
    hour := 15
    
    switch {
    case hour < 12:
        fmt.Println("Good morning!")
    case hour < 17:
        fmt.Println("Good afternoon!")
    default:
        fmt.Println("Good evening!")
    }
}
```

### Switch with Short Statement

```go
switch initialization; expression {
case value1:
    // code to execute if expression == value1
case value2:
    // code to execute if expression == value2
default:
    // code to execute if no case matches
}
```

Example:

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    switch today := time.Now().Weekday(); today {
    case time.Saturday, time.Sunday:
        fmt.Println("It's the weekend!")
    default:
        fmt.Println("It's a weekday.")
    }
}
```

### Fallthrough

By default, Go's switch cases break automatically. Use `fallthrough` to execute the next case:

```go
package main

import "fmt"

func main() {
    num := 5
    
    switch num {
    case 5:
        fmt.Println("Five")
        fallthrough
    case 4:
        fmt.Println("Four")
        fallthrough
    case 3:
        fmt.Println("Three")
    case 2:
        fmt.Println("Two")
    case 1:
        fmt.Println("One")
    }
}
```

## Exercises

1. Write a program that checks if a number is positive, negative, or zero.
2. Create a program that prints the first 10 Fibonacci numbers using a for loop.
3. Write a program that uses a switch statement to print the name of a month given its number (1-12).
4. Create a program that iterates through a string and counts the number of vowels.
5. Write a program that uses nested loops to print a multiplication table for numbers 1 through 5.

## Summary

In this tutorial, you've learned about Go's control structures:

- Conditional statements (`if`, `else if`, `else`)
- Loops (`for`, `range`)
- Loop control (`break`, `continue`)
- Switch statements and their variations

These control structures form the backbone of program flow control in Go. They allow you to make decisions, repeat actions, and handle different cases in your code. In the next tutorial, we'll explore functions in Go. 