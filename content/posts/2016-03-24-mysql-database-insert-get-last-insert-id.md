---
title: 'MySQL Database Insert & Get Last Insert ID'
author: Edd Turtle
type: post
date: 2016-03-24T18:31:09+00:00
url: /mysql-database-insert-get-last-insert-id/
categories:
  - Uncategorized
tags:
  - database
  - driver
  - exec
  - id
  - injection
  - insert
  - lastinsertid
  - param
  - query
  - sql

---
This shows how we can use the `go-sql-driver` (which uses the `database/sql` interface) to form a connection to our database server using the `Open` method.

Once we have our connection, we can create a statement of our SQL query, and to prevent SQL injection we send the parameters (values which should correspond with to the question marks in our query) separately along with the `Exec` method. Finally, calling the `LastInsertId()` we can get the id of the row we've just inserted - and in our example, we output this value to screen.

```go
package main

import (
    "database/sql"
    "fmt"
    "log"

    _ "github.com/go-sql-driver/mysql"
)

const (
    DB_USER    = ""
    DB_PASS    = ""
    DB_NAME    = ""
    DB_CHARSET = "utf8"
)

func main() {
    db, err := sql.Open("mysql", DB_USER+":"+DB_PASS+"@/"+DB_NAME+"?charset="+DB_CHARSET)
    if err != nil {
        log.Fatal("Cannot open DB connection", err)
    }

    stmt, err := db.Prepare("INSERT data SET content=?")
    if err != nil {
        log.Fatal("Cannot prepare DB statement", err)
    }

    res, err := stmt.Exec("value")
    if err != nil {
        log.Fatal("Cannot run insert statement", err)
    }

    id, _ := res.LastInsertId()

    fmt.Printf("Inserted row: %d", id)
}
```