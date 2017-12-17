---
title: HTTP Response Status Codes
author: Edd Turtle
type: post
date: 2017-08-06T14:00:00+00:00
url: /get-the-http-response-status-code/
categories:
  - Uncategorized
tags:
  - net
  - http
  - response
  - status
  - code
  - error handling
---

When making http requests with Go it is almost always necessary to check the status code of the response which is returned. Generally if the status code is between 200 and 300 you should receive a successful response. But anything except a 200-300 status, we often need to handle.

<!--more-->

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "strconv"
)

func main() {

    resp, err := http.Get("https://golangcode.com")
    if err != nil {
        log.Fatal(err)
    }

    fmt.Println("HTTP Response Status: " + strconv.Itoa(resp.StatusCode))

    if resp.StatusCode >= 200 && resp.StatusCode <= 299 {
        fmt.Println("HTTP Status OK!")
    } else {
        fmt.Println("Argh! Broken")
    }
}

```

StatusCode will be an integer, so in our example we convert it with `strconv` to a string before printing it to screen.