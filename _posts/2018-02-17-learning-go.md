---
layout: post
title:  "Learning Go"
author: MJ Rossetti
published: true
img: go-logo.png
categories:
 - reference-materials
technologies:
  - go
  - git
  - homebrew
---

This document provides a [Go](https://golang.org) programming language reference.

Official documentation:

  + [Getting Started Guide](https://golang.org/doc/code.html)
  + [Language Specification](https://golang.org/ref/spec)
  + [Built-in Packages](https://golang.org/pkg/)
  + [Idioms and Best Practices](https://golang.org/doc/effective_go.html)

Third-party resources:

  + [Go By Example: Arrays](https://gobyexample.com/arrays)
  + [Go By Example: For Loops](https://gobyexample.com/for)
  + [Go By Example: Time](https://gobyexample.com/time)
  + [Learn Go in an Hour (video)](https://www.youtube.com/watch?v=CF9S4QZuV30)

## Installation and Setup

Install Go:

```sh
brew install go
```

Add to ~/.bash_profile:

```sh
export GOPATH=$(go env GOPATH)
export PATH=$(go env GOPATH)/bin:$PATH
```

Restart terminal and create a new directory located at the `GOPATH`:

```sh
mkdir -p $GOPATH
```

## Getting Started

### Programs

All Go programs belong in the `$GOPATH/src` directory. The namespaces and subdirectories in this directory generally follow Internet-style namespacing conventions (i.e. `$GOPATH/src/github.com/data-creative/hello`) to uniquely identify a specific program.

#### Making Programs

Make a new project directory called `hello` (where `YOUR_USERNAME` is your GitHub username):

```sh
mkdir -p $GOPATH/src/github.com/YOUR_USERNAME/hello
cd $GOPATH/src/github.com/YOUR_USERNAME/hello
```

Place inside it the `hello.go` file:

```sh
touch hello.go
```

... and paste inside it the following code:

```go
// hello.go

package main

import "fmt"

func main() {
  fmt.Println("Hello World")
}
```

> NOTE: executable programs specify a package name of `main` (whereas importable packages can specify custom package names)

Optionally initialize a new repo:

```sh
git init .
git add .
git commit -m "Create my first go program"
```

#### Executing Programs

Run the program:

```go
go run hello.go
```

Alternatively execute `go install` to create an executable file within `$GOPATH/bin`, and then run that executable:

```sh
$GOPATH/bin/hello # or just `hello`, if you have already added `$GOPATH/bin` to your `$PATH`
```


## Language

### [Comments](https://golang.org/ref/spec#Comments)

Go uses JavaScript-like comments.

```go
// single line comment

/*
multi
line
comment
*/
```

### [Operators](https://golang.org/ref/spec#Operators)

[Comparison Operators](https://golang.org/ref/spec#Comparison_operators):

```go
== // "is equal to?"
!= // "is not equal to?"
```

[Logical Operators](https://golang.org/ref/spec#Logical_operators):

```go
&& // "and"
|| // "or"
! // "not"
```

### Printing / Logging

There are two ways to print. The [recommended](https://stackoverflow.com/a/14680544/670433) way involves importing the `fmt` package.

```go
println("Hello World") // one way

import "fmt"
fmt.Println("Hello World") // recommended way
```

> NOTE: subsequent references to the "fmt" package assume you are importing it


### [Variables](https://golang.org/ref/spec#Variable_declarations)

Variable declaration:

```go
var s string
s = "Hello World"
fmt.Println(s) //> "Hello World"
```

One-liners:

```go
var s string = "Hello World"
fmt.Println(s) //> "Hello World"
```

```go
var s = "Hello World"
fmt.Println(s) //> "Hello World"
```

```go
s := "Hello World"
fmt.Println(s) //> "Hello World"
```

[Constants](https://golang.org/ref/spec#Constant_expressions):

```go
const s string = "Hello World"
fmt.Println(s)
```

### Data Types

Example data types:

datatype | description
--- | ---
`boolean` | Standard boolean values include `true` and `false`.
`string` | ...
`int` | ...
`float64` | ...
`array` | An ordered list of values.
`slice` | A subset of an `array`.
`map` | An object with key-value pairs.
`struct` | A custom data type, or "class".

Checking a variable's datatype:

```go
// BUILT-IN

fmt.Println(reflect.TypeOf("Hello World")) //> string

fmt.Println(reflect.TypeOf(10)) //> int
fmt.Println(reflect.TypeOf(4.5)) //> float64

fmt.Println(reflect.TypeOf(true)) //> bool
fmt.Println(reflect.TypeOf(false)) //> bool

arr := [5]int{1, 2, 3, 4, 5}
fmt.Println(reflect.TypeOf(arr)) //> [5]int
fmt.Println(reflect.TypeOf(arr).Kind()) //> array

m := map[string]string{"Washington": "DC", "San Francisco": "CA"}
fmt.Println(reflect.TypeOf(m)) //> map[string]string
fmt.Println(reflect.TypeOf(m).Kind()) //> map

// PACKAGE-DEFINED

fmt.Println(reflect.TypeOf(time.Now())) //> time.Time

// CUSTOM-DEFINED

type Team struct {
  city string
  name  string
}
t := Team{city: "New York", name: "Yankees"}
fmt.Println(reflect.TypeOf(t)) //> main.Team
```

[Converting between numbers and strings](https://golang.org/pkg/strconv/#hdr-Numeric_Conversions):

```go
import (
  "fmt"
  "reflect"
	"strconv"
)

func main()  {
  s := strconv.Itoa(11)
  fmt.Println(s) //> 11
  fmt.Println(reflect.TypeOf(s)) //> string

  i, _ := strconv.Atoi("11")
  fmt.Println(i) //> 11
  fmt.Println(reflect.TypeOf(i)) //> integer
}
```

#### [Booleans](https://golang.org/ref/spec#Boolean_types)

```go
true == true //> true
true == false //> false
```

#### [Numbers](https://golang.org/ref/spec#Numeric_types)

Perform numeric operations:

```go
fmt.Println(10 + 2) //> 12
fmt.Println(10 - 2) //> 8
fmt.Println(10 * 2) //> 20
fmt.Println(10 / 2) //> 5
```

#### [Strings](https://golang.org/ref/spec#String_types)

[String Concatenation](https://golang.org/ref/spec#String_concatenation):

```go
s := "Hello" + " " + "World"
fmt.Println(s) //> "Hello World"
```

String Interpolation (or the closest thing to it):

```go
w := "World"
n := 123
f := false
m := fmt.Sprintf("Hello %s %d %t", w, n, f) // see: https://golang.org/pkg/fmt/#hdr-Printing
fmt.Println(m) //> "Hello World 123 false"
```

#### [Arrays](https://golang.org/ref/spec#Array_types) and [Slices](https://golang.org/ref/spec#Slice_types)

Instantiate new arrays:

```go
arr1 := []string{"A", "B", "C"}
arr2 := []int{1, 2, 3}
```

Array functions:

```go
arr := []string{"A", "B", "C"}
len(arr) //> 3
```

[Index expressions](https://golang.org/ref/spec#Index_expressions):

```go
arr := []string{"A", "B", "C"}

fmt.Println(arr[0]) //> "A"
fmt.Println(arr[1]) //> "B"
fmt.Println(arr[2]) //> "C"
fmt.Println(arr[3]) //> panic: runtime error: index out of range
fmt.Println(arr[len(arr)-1]) //> "C"
```

[Slice expressions](https://golang.org/ref/spec#Slice_expressions):

```go
arr := []string{"A", "B", "C", "D", "E", "F"}
fmt.Println(arr[2:4]) //> [C D]
fmt.Println(arr[2:]) //> [C D E F]
fmt.Println(arr[:4]) //> [A B C D]
```

Adding items:

```go
arr := []string{"A", "B", "C"}
arr = append(arr, "D")
fmt.Println(arr) //> [A B C D]
```

Removing items:

```go
arr := []string{"A", "B", "C"}
i := 1
arr = append(arr[:i], arr[i+1:]...)
	fmt.Println(arr) //> [A C]
```

Mutating items:

```go
arr := []string{"A", "B", "C"}
arr[1] = "Z"
fmt.Println(arr) //> [A Z C]
```

Concatenating arrays/slices:

```go
arr1 := []string{"A", "B", "C"}
arr2 := []string{"D", "E", "F"}

fmt.Println(append(arr1, arr2...)) //> [A B C D E F]
```

Iteration ([For statements with Range clause](https://golang.org/ref/spec#For_range), see also: "For Loops"):

```go
arr := []string{"A", "B", "C"}

for index, element := range arr {
  fmt.Println(i, element)
}
//> 0 A
//> 1 B
//> 2 C
```

#### [Structs](https://golang.org/ref/spec#Struct_types)

Definition:

```go
type Team struct {
  city string
  name string
}
```

Instantiation:

```go
t := Team{city: "New York", name: "Yankees"}
```

Accessing attributes:

```go
t := Team{city: "New York", name: "Yankees"}
fmt.Println(t.city, t.name) //> "New York Yankees"
```

Mutating attributes:

```go
t := Team{city: "New York", name: "Yankees"}
t.name = "Mets"
fmt.Println(t.city, t.name) //> "New York Mets"
```

Virtual attributes (see also: "Functions"):

```go
type Team struct {
	city string
	name string
}

func (t *Team) fullName() string {
  return t.city + " " + t.name
}

func main()  {

	t := Team{city: "New York", name: "Yankees"}

	fmt.Println(t.fullName()) //> "New York Yankees"
}
```

### [For Statements](https://golang.org/ref/spec#For_statements)

The following loops are equivalent:

```go
i := 0
for i < 5 {
  fmt.Println(i)
	i++ // increments (equivalent to: `i = i + 1` and `i+=1`
}
```

```go
for i := 0; i < 5; i++ {
	fmt.Println(i)
}
```

### [Functions](https://golang.org/ref/spec#Function_declarations)

Example function which declares parameter and return datatypes:

```go
// DEFINITION
func enlarge(x int) int {
	return x * 100
}

// INVOCATION
enlarge(5) //> 500
```

### Conditionals

#### [If Statements](https://golang.org/ref/spec#If_statements)

Example with all clauses:
```go
val := 15

if val == 3 {
  fmt.Println("THREE")
} else if val == 5 {
	fmt.Println("FIVE")
} else {
	fmt.Println("OTHER")
}
```

#### [Switch Statements](https://golang.org/ref/spec#Switch_statements)

Switch statements with values:

```go
val := 15

switch val {
  case 0, 1, 2, 3: fmt.Println("LOW")
  case 4, 5, 6, 7: fmt.Println("HIGH")
  default: fmt.Println("OTHER")
}
```

Switch statements with expressions:

```go
val := 15

switch {
  case val < 4: fmt.Println("LOW")
  case val < 8: fmt.Println("HIGH")
  default: fmt.Println("OTHER")
}
```

### [Errors](https://golang.org/ref/spec#Errors)

Raising errors:

```go
panic("OOPS")
```

Handling errors:

```go
// todo: not the most straightforward...
```
