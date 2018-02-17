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
  + [Go By Example: Time](https://gobyexample.com/time)

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

[Converting between datatypes](https://golang.org/ref/spec#Conversions):

```go
import (
  "fmt"
  "reflect"
	"strconv"
)

func main()  {
	i := 11
	s := strconv.Itoa(i)

	fmt.Println(s) //> 11
	fmt.Println(reflect.TypeOf(s)) //> string
}
```
