## Defining Structs

A struct is defined using the `type` and `struct` keywords:

```go
type Person struct {
    FirstName string
    LastName  string
    Age       int
    Email     string
}
```

In this example, we've defined a `Person` struct with four fields: `FirstName`, `LastName`, `Age`, and `Email`, each with its own type.

## Creating Struct Instances

There are several ways to create instances of a struct:

### 1. Using a struct literal with field names

```go
person1 := Person{
    FirstName: "John",
    LastName:  "Doe",
    Age:       30,
    Email:     "john.doe@example.com",
}
```

### 2. Using a struct literal with values in field order

```go
person2 := Person{"Jane", "Smith", 25, "jane.smith@example.com"}
```

Note: This approach is not recommended as it makes the code less maintainable. If the struct definition changes, this code might break.

### 3. Creating an empty struct and assigning values later

```go
var person3 Person
person3.FirstName = "Bob"
person3.LastName = "Johnson"
person3.Age = 35
person3.Email = "bob.johnson@example.com"
```

### 4. Using the new function

```go
person4 := new(Person)
person4.FirstName = "Alice"
person4.LastName = "Williams"
person4.Age = 28
person4.Email = "alice.williams@example.com"
```

The `new` function allocates memory for the struct and returns a pointer to it.

## Accessing Struct Fields

You can access struct fields using the dot notation:

```go
package main

import "fmt"

type Person struct {
    FirstName string
    LastName  string
    Age       int
    Email     string
}

func main() {
    person := Person{
        FirstName: "John",
        LastName:  "Doe",
        Age:       30,
        Email:     "john.doe@example.com",
    }
    
    // Accessing fields
    fmt.Println("First Name:", person.FirstName)
    fmt.Println("Last Name:", person.LastName)
    fmt.Println("Age:", person.Age)
    fmt.Println("Email:", person.Email)
    
    // Modifying fields
    person.Age = 31
    person.Email = "john.doe.updated@example.com"
    
    fmt.Println("\nAfter modification:")
    fmt.Println("Age:", person.Age)
    fmt.Println("Email:", person.Email)
}
```

## Struct Pointers

When working with large structs or when you need to modify a struct in a function, it's common to use pointers to structs:

```go
package main

import "fmt"

type Person struct {
    FirstName string
    LastName  string
    Age       int
}

func updateAge(p *Person, newAge int) {
    p.Age = newAge // Go automatically dereferences the pointer
}

func main() {
    person := Person{
        FirstName: "John",
        LastName:  "Doe",
        Age:       30,
    }
    
    fmt.Println("Before update:", person.Age)
    
    updateAge(&person, 31)
    
    fmt.Println("After update:", person.Age)
}
```

Note: Go automatically dereferences pointers to structs when accessing fields. You can write `p.Age` instead of `(*p).Age`.

## Anonymous Structs

You can create one-off structs without defining a new type:

```go
package main

import "fmt"

func main() {
    // Anonymous struct
    person := struct {
        Name string
        Age  int
    }{
        Name: "John",
        Age:  30,
    }
    
    fmt.Println("Name:", person.Name)
    fmt.Println("Age:", person.Age)
}
```

Anonymous structs are useful for one-time use cases where you don't need to reuse the struct type.

## Nested Structs

Structs can contain other structs as fields:

```go
package main

import "fmt"

type Address struct {
    Street  string
    City    string
    State   string
    ZipCode string
}

type Person struct {
    FirstName string
    LastName  string
    Age       int
    Address   Address
}

func main() {
    person := Person{
        FirstName: "John",
        LastName:  "Doe",
        Age:       30,
        Address: Address{
            Street:  "123 Main St",
            City:    "Anytown",
            State:   "CA",
            ZipCode: "12345",
        },
    }
    
    fmt.Println("Person:", person.FirstName, person.LastName)
    fmt.Println("Lives at:", person.Address.Street)
    fmt.Println("City:", person.Address.City)
}
```

## Embedded Structs

Go supports struct embedding, which is a form of composition. It allows you to include one struct type within another without giving it a name:

```go
package main

import "fmt"

type Person struct {
    FirstName string
    LastName  string
    Age       int
}

type Employee struct {
    Person        // Embedded struct
    EmployeeID    string
    Position      string
    Department    string
}

func main() {
    employee := Employee{
        Person: Person{
            FirstName: "John",
            LastName:  "Doe",
            Age:       30,
        },
        EmployeeID: "E12345",
        Position:   "Software Engineer",
        Department: "Engineering",
    }
    
    // Access fields from the embedded struct directly
    fmt.Println("Employee:", employee.FirstName, employee.LastName)
    fmt.Println("Age:", employee.Age)
    
    // You can also access them through the embedded struct name
    fmt.Println("Full name:", employee.Person.FirstName, employee.Person.LastName)
    
    // Access fields from the outer struct
    fmt.Println("Employee ID:", employee.EmployeeID)
    fmt.Println("Position:", employee.Position)
    fmt.Println("Department:", employee.Department)
}
```

Embedding allows you to "inherit" fields and methods from the embedded struct, promoting code reuse and composition.

## Struct Tags

Struct tags provide metadata about struct fields. They are commonly used for encoding/decoding data, validation, and other purposes:

```go
package main

import (
    "encoding/json"
    "fmt"
)

type Person struct {
    FirstName string `json:"first_name"`
    LastName  string `json:"last_name"`
    Age       int    `json:"age,omitempty"`
    Email     string `json:"-"` // This field will be ignored during JSON encoding
}

func main() {
    person := Person{
        FirstName: "John",
        LastName:  "Doe",
        Age:       30,
        Email:     "john.doe@example.com",
    }
    
    // Convert struct to JSON
    jsonData, err := json.Marshal(person)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    
    fmt.Println("JSON:", string(jsonData))
    
    // JSON output: {"first_name":"John","last_name":"Doe","age":30}
    // Note that the Email field is omitted due to the "-" tag
}
```

## Methods on Structs

Go doesn't have classes, but you can define methods on structs. A method is a function with a special receiver argument:

```go
package main

import "fmt"

type Rectangle struct {
    Width  float64
    Height float64
}

// Method with a value receiver
func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

// Method with a pointer receiver
func (r *Rectangle) Scale(factor float64) {
    r.Width *= factor
    r.Height *= factor
}

func main() {
    rect := Rectangle{Width: 10, Height: 5}
    
    fmt.Println("Original dimensions:", rect.Width, rect.Height)
    fmt.Println("Area:", rect.Area())
    
    rect.Scale(2)
    
    fmt.Println("After scaling:")
    fmt.Println("New dimensions:", rect.Width, rect.Height)
    fmt.Println("New area:", rect.Area())
}
```

### Value Receivers vs. Pointer Receivers

- **Value Receiver** (`func (r Rectangle) ...`): The method operates on a copy of the struct. Changes to the struct inside the method don't affect the original struct.
- **Pointer Receiver** (`func (r *Rectangle) ...`): The method operates on a pointer to the struct. Changes to the struct inside the method affect the original struct.

Use pointer receivers when:
- You need to modify the struct
- The struct is large and copying it would be inefficient
- Consistency is needed with other methods that use pointer receivers

## Comparing Structs

Structs can be compared with the `==` operator if all their fields are comparable:

```go
package main

import "fmt"

type Person struct {
    FirstName string
    LastName  string
    Age       int
}

func main() {
    person1 := Person{"John", "Doe", 30}
    person2 := Person{"John", "Doe", 30}
    person3 := Person{"Jane", "Smith", 25}
    
    fmt.Println("person1 == person2:", person1 == person2) // true
    fmt.Println("person1 == person3:", person1 == person3) // false
}
```

Note: If a struct contains fields that are not comparable (like slices or maps), the struct itself cannot be compared using `==`.

## Examples

### Example 1: Student Management System

```go
package main

import (
    "fmt"
    "strings"
)

type Course struct {
    Code        string
    Name        string
    CreditHours int
}

type Student struct {
    ID        string
    FirstName string
    LastName  string
    Courses   []Course
    Grades    map[string]float64 // Course code -> grade
}

// Method to get the student's full name
func (s Student) FullName() string {
    return s.FirstName + " " + s.LastName
}

// Method to add a course for the student
func (s *Student) AddCourse(course Course) {
    s.Courses = append(s.Courses, course)
    // Initialize grade as 0
    if s.Grades == nil {
        s.Grades = make(map[string]float64)
    }
    s.Grades[course.Code] = 0
}

// Method to set a grade for a course
func (s *Student) SetGrade(courseCode string, grade float64) bool {
    // Check if the student is enrolled in the course
    for _, course := range s.Courses {
        if course.Code == courseCode {
            s.Grades[courseCode] = grade
            return true
        }
    }
    return false
}

// Method to calculate GPA
func (s Student) GPA() float64 {
    if len(s.Grades) == 0 {
        return 0
    }
    
    totalPoints := 0.0
    totalCredits := 0
    
    for _, course := range s.Courses {
        if grade, exists := s.Grades[course.Code]; exists {
            totalPoints += grade * float64(course.CreditHours)
            totalCredits += course.CreditHours
        }
    }
    
    if totalCredits == 0 {
        return 0
    }
    
    return totalPoints / float64(totalCredits)
}

// Method to print student information
func (s Student) PrintInfo() {
    fmt.Printf("Student: %s (ID: %s)\n", s.FullName(), s.ID)
    fmt.Println("Courses:")
    
    for _, course := range s.Courses {
        grade := s.Grades[course.Code]
        fmt.Printf("  - %s: %s (%.1f)\n", course.Code, course.Name, grade)
    }
    
    fmt.Printf("GPA: %.2f\n", s.GPA())
}

func main() {
    // Create courses
    courses := []Course{
        {Code: "CS101", Name: "Introduction to Programming", CreditHours: 3},
        {Code: "CS102", Name: "Data Structures", CreditHours: 4},
        {Code: "MATH101", Name: "Calculus I", CreditHours: 4},
        {Code: "ENG101", Name: "English Composition", CreditHours: 3},
    }
    
    // Create a student
    student := Student{
        ID:        "S12345",
        FirstName: "John",
        LastName:  "Doe",
    }
    
    // Add courses to the student
    for _, course := range courses {
        student.AddCourse(course)
    }
    
    // Set grades
    student.SetGrade("CS101", 3.7)
    student.SetGrade("CS102", 4.0)
    student.SetGrade("MATH101", 3.5)
    student.SetGrade("ENG101", 3.8)
    
    // Print student information
    student.PrintInfo()
    
    // Try to set a grade for a course the student is not enrolled in
    if !student.SetGrade("PHYS101", 3.0) {
        fmt.Println("\nStudent is not enrolled in PHYS101")
    }
}
```

### Example 2: Shape Hierarchy

```go
package main

import (
    "fmt"
    "math"
)

// Shape interface
type Shape interface {
    Area() float64
    Perimeter() float64
}

// Circle struct
type Circle struct {
    Radius float64
}

func (c Circle) Area() float64 {
    return math.Pi * c.Radius * c.Radius
}

func (c Circle) Perimeter() float64 {
    return 2 * math.Pi * c.Radius
}

// Rectangle struct
type Rectangle struct {
    Width  float64
    Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}

func (r Rectangle) Perimeter() float64 {
    return 2 * (r.Width + r.Height)
}

// Triangle struct
type Triangle struct {
    SideA float64
    SideB float64
    SideC float64
}

func (t Triangle) Area() float64 {
    // Using Heron's formula
    s := (t.SideA + t.SideB + t.SideC) / 2
    return math.Sqrt(s * (s - t.SideA) * (s - t.SideB) * (s - t.SideC))
}

func (t Triangle) Perimeter() float64 {
    return t.SideA + t.SideB + t.SideC
}

// Function to print shape information
func printShapeInfo(s Shape) {
    fmt.Printf("Area: %.2f\n", s.Area())
    fmt.Printf("Perimeter: %.2f\n", s.Perimeter())
}

func main() {
    circle := Circle{Radius: 5}
    rectangle := Rectangle{Width: 10, Height: 5}
    triangle := Triangle{SideA: 3, SideB: 4, SideC: 5}
    
    fmt.Println("Circle:")
    printShapeInfo(circle)
    
    fmt.Println("\nRectangle:")
    printShapeInfo(rectangle)
    
    fmt.Println("\nTriangle:")
    printShapeInfo(triangle)
}
```

## Exercises

1. Create a `Book` struct with fields for title, author, publication year, and price. Write methods to:
   - Format the book information as a string
   - Apply a discount to the book price
   - Check if the book is older than a given year

2. Design a `BankAccount` struct with fields for account number, owner name, and balance. Implement methods to:
   - Deposit money
   - Withdraw money (with validation to prevent overdrafts)
   - Transfer money to another account
   - Print account details

3. Create a `Time` struct with fields for hours, minutes, and seconds. Implement methods to:
   - Add a specified number of seconds to the time
   - Calculate the difference between two times in seconds
   - Format the time as a string (e.g., "14:30:05")

4. Design a `Product` struct and a `ShoppingCart` struct. The `ShoppingCart` should contain a slice of products. Implement methods to:
   - Add products to the cart
   - Remove products from the cart
   - Calculate the total price
   - Apply a discount to all products

5. Create a hierarchy of vehicle types using struct embedding. Start with a base `Vehicle` struct and create specialized types like `Car`, `Truck`, and `Motorcycle`. Implement appropriate methods for each type.

## Summary

In this tutorial, you've learned about structs in Go:

- How to define and create structs
- Accessing and modifying struct fields
- Working with struct pointers
- Using anonymous structs
- Nesting and embedding structs
- Adding metadata with struct tags
- Defining methods on structs
- Comparing structs
- Practical examples of using structs

Structs are a fundamental building block in Go that allow you to create custom data types to represent complex entities. Combined with methods, they provide a powerful way to organize and manipulate data in your Go programs. In the next tutorial, we'll explore pointers in Go. 