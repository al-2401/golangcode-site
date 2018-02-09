---
title: Writing to a File
author: Edd Turtle
type: post
date: 2015-08-02T11:50:20+00:00
url: /writing-to-file/
categories:
  - Uncategorized
tags:
  - close
  - create
  - defer
  - file
  - fmt
  - fprintf
  - os
  - write
  - writing

---
Often it's very useful being able to write to a file from a script, for logging purposes, saving results or as a data store. This is a little snippet on how to write to a file.

We use the `os` package to create the file, exiting the program and logging if it can't do this and printing a string into the file with Fprintf. We're also using defer to close the file access once the function has finished running (which is a really neat way of remembering to close files).

```go
package main

import (
    "fmt"
    "log"
    "os"
)

func main() {
    file, err := os.Create("result.txt")
    if err != nil {
        log.Fatal("Cannot create file", err)
    }
    defer file.Close()

    fmt.Fprintf(file, "Hello Readers of golangcode.com")
}
```