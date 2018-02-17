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
