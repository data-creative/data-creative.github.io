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
  fmt.Printf("Hello World")
	fmt.Printf("\n")
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

### Printing / Logging

There are two ways to print. The [recommended](https://stackoverflow.com/a/14680544/670433) way involves importing the `fmt` package.

```go
println("Hello World") // one way

import "fmt"
fmt.Println("Hello World") // recommended way
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


### [Variables](https://golang.org/ref/spec#Variable_declarations)

Variable declaration options, from static to dynamic typing:

```go
var s string
s = "Hello World"
fmt.Printf(s) //> "Hello World"
```

```go
var s
s = "Hello World"
fmt.Printf(s)
```

One-liners:

```go
var s string = "Hello World"
fmt.Printf(s) //> "Hello World"
```

```go
var s = "Hello World"
fmt.Printf(s) //> "Hello World"
```

```go
s = "Hello World"
fmt.Printf(s) //> "Hello World"
```

```go
s := "Hello World"
fmt.Printf(s) //> "Hello World"
```

[Constants](https://golang.org/ref/spec#Constant_expressions):

```go
const s string = "Hello World"
fmt.Printf(s)
```
