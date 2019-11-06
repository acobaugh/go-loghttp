# go-loghttp

[![GoDoc](https://godoc.org/github.com/acobaugh/go-loghttp?status.svg)](https://godoc.org/github.com/acobaugh/go-loghttp)

Log http.Client's requests and responses automatically.

Forked from [github.com/motemen/go-loghttp](http://godoc.org/github.com/motemen/go-loghttp) to add additional request details (headers, request body, etc) and add module support.

## Synopsis

To log all the HTTP requests/responses, import `github.com/acobaugh/go-loghttp/global`.

```go
package main

import (
	"io"
	"log"
	"net/http"
	"os"

	_ "github.com/acobaugh/go-loghttp/global" // Just this line!
)

func main() {
	resp, err := http.Get(os.Args[1])
	if err != nil {
		log.Fatal(err)
	}

	io.Copy(os.Stdout, resp.Body)
}
```

```
% go run main.go http://example.com/
2014/12/02 13:36:27 ---> GET http://example.com/
2014/12/02 13:36:27 <--- 200 http://example.com/
<!doctype html>
...
```

Or set `loghttp.Transport` to `http.Client`'s `Transport` field.

```go
import "github.com/acobaugh/go-loghttp"

client := &http.Client{
	Transport: &loghttp.Transport{},
}
```

You can modify [loghttp.Transport](http://godoc.org/github.com/acobaugh/go-loghttp#Transport)'s `LogRequest` and `LogResponse` to customize logging function.

## Authors

acobaugh <andrew.cobaugh@gmail.com>
motemen <motemen@gmail.com>
