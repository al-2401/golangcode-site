---
title: Serve Static Assets using the Mux Router
author: Edd Turtle
type: post
date: 2016-05-22T11:54:24+00:00
url: /serve-static-assets-using-the-mux-router/
categories:
  - Uncategorized
tags:
  - assets
  - css
  - dir
  - file
  - handler
  - images
  - img
  - js
  - mux
  - router
  - serve
  - server
  - static

---
Using a router is great when passing off incoming requests to functions to handle and return data. Often though, you just want to serve an entire directory and make everything inside it public. This is useful for images, styles and javascript. 

In this example we're using the <a href="https://github.com/gorilla/mux" target="_blank">Gorilla mux router</a> (&#8220;HTTP request multiplexer&#8221;) and setup a new route for the entire directory. We're using `static` as the folder to serve which we pass to `FileServer()` as a new route on the router.

```go
package main

import (
    "log"
    "net/http"

    "github.com/gorilla/mux"
)

const (
    STATIC_DIR = "/static/"
    PORT       = "8080"
)

func main() {
    router := NewRouter()
    err := http.ListenAndServe(":"+PORT, router)
    if err != nil {
        log.Fatal("ListenAndServe Error: ", err)
    }
}

func NewRouter() *mux.Router {
    router := mux.NewRouter().StrictSlash(true)

    // Server CSS, JS & Images Statically.
    router.
        PathPrefix(STATIC_DIR).
        Handler(http.StripPrefix(STATIC_DIR, http.FileServer(http.Dir("."+STATIC_DIR))))

    return router
}
```