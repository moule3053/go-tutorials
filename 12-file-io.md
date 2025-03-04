# File I/O in Go

## Introduction

File input/output (I/O) is a fundamental aspect of programming that allows you to read from and write to files. Go provides a rich set of libraries for handling file I/O operations, making it easy to work with files in your applications. This tutorial will cover how to read from and write to files in Go.

## Opening and Closing Files

To work with files in Go, you first need to open them using the `os` package. Always remember to close the file after you're done to free up resources.

### Opening a File

You can open a file using `os.Open()`:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    file, err := os.Open("example.txt")
    if err != nil {
        fmt.Println("Error opening file:", err)
        return
    }
    defer file.Close() // Ensure the file is closed when done

    fmt.Println("File opened successfully")
}
```

### Closing a File

Use `defer` to ensure that the file is closed when the function exits:

```go
defer file.Close()
```

## Reading from Files

You can read from files using the `bufio` package, which provides buffered I/O. This is efficient for reading large files.

### Reading All Content

To read the entire content of a file, you can use `ioutil.ReadFile()`:

```go
package main

import (
    "fmt"
    "io/ioutil"
)

func main() {
    data, err := ioutil.ReadFile("example.txt")
    if err != nil {
        fmt.Println("Error reading file:", err)
        return
    }
    fmt.Println("File content:")
    fmt.Println(string(data))
}
```

### Reading Line by Line

To read a file line by line, use `bufio.NewScanner()`:

```go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    file, err := os.Open("example.txt")
    if err != nil {
        fmt.Println("Error opening file:", err)
        return
    }
    defer file.Close()

    scanner := bufio.NewScanner(file)
    for scanner.Scan() {
        fmt.Println(scanner.Text()) // Print each line
    }

    if err := scanner.Err(); err != nil {
        fmt.Println("Error reading file:", err)
    }
}
```

## Writing to Files

You can write to files using the `os` and `bufio` packages.

### Creating a New File

To create a new file or truncate an existing file, use `os.Create()`:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    file, err := os.Create("output.txt")
    if err != nil {
        fmt.Println("Error creating file:", err)
        return
    }
    defer file.Close()

    fmt.Println("File created successfully")
}
```

### Writing to a File

You can write to a file using `file.WriteString()`:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    file, err := os.Create("output.txt")
    if err != nil {
        fmt.Println("Error creating file:", err)
        return
    }
    defer file.Close()

    _, err = file.WriteString("Hello, Go!\n")
    if err != nil {
        fmt.Println("Error writing to file:", err)
        return
    }

    fmt.Println("Data written to file successfully")
}
```

### Writing with Buffered I/O

For more efficient writing, use `bufio.NewWriter()`:

```go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    file, err := os.Create("output.txt")
    if err != nil {
        fmt.Println("Error creating file:", err)
        return
    }
    defer file.Close()

    writer := bufio.NewWriter(file)
    _, err = writer.WriteString("Hello, buffered writing!\n")
    if err != nil {
        fmt.Println("Error writing to file:", err)
        return
    }

    // Flush the buffer to ensure all data is written
    writer.Flush()
    fmt.Println("Data written to file successfully")
}
```

## Example: Reading and Writing Files

1. Create a file named `example.txt` with some sample content.
2. Write a program that reads the content of `example.txt` and writes it to a new file named `copy.txt`.

```go
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    // Open the source file
    sourceFile, err := os.Open("example.txt")
    if err != nil {
        fmt.Println("Error opening source file:", err)
        return
    }
    defer sourceFile.Close()

    // Create the destination file
    destFile, err := os.Create("copy.txt")
    if err != nil {
        fmt.Println("Error creating destination file:", err)
        return
    }
    defer destFile.Close()

    // Create a scanner to read the source file
    scanner := bufio.NewScanner(sourceFile)
    writer := bufio.NewWriter(destFile)

    // Read from source and write to destination
    for scanner.Scan() {
        _, err := writer.WriteString(scanner.Text() + "\n")
        if err != nil {
            fmt.Println("Error writing to destination file:", err)
            return
        }
    }

    // Flush the writer buffer
    writer.Flush()

    if err := scanner.Err(); err != nil {
        fmt.Println("Error reading source file:", err)
    }

    fmt.Println("File copied successfully")
}
```

## Exercises

1. Write a program that reads a text file and counts the number of lines, words, and characters.

2. Create a program that appends text to an existing file.

3. Write a program that reads a CSV file and prints its content in a formatted table.

4. Implement a program that reads a JSON file and unmarshals it into a struct.

5. Create a program that reads a file and writes its content in reverse order to a new file.

## Summary

In this tutorial, you've learned about file I/O in Go:

- How to open and close files
- Reading from files using different methods
- Writing to files using buffered and unbuffered I/O
- Practical examples of reading and writing files

File I/O is an essential skill for any programmer, and Go provides powerful tools for working with files. In the next tutorial, we'll explore concurrency in Go. 