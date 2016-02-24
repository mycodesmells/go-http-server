# Simple HTTP Server In Go

When I started working with Go, I thought of that as a language created specifically for small apps used locally, like utility scripts, microservices etc. As it turned out, it is also perfect for creating web applications in NodeJS style - setting up an HTTP server and responing to the requests. Let's take a look how exactly do you do that in Go.

### Server Structure

Building an HTTP server in Go is so simple, that it's hard to believe that this is actually enough. Basically you need two parts: **handlers** that em... handle incoming requests, and a serve function that... serves the application. See, its self-explanatory, so it mus be easy.

Writing a handler comes down to two things: defining a path that needs to match with incoming request, and a function that takes a `http.ResponseWriter` (representing a response) and `*http.Request` parameters.

Serving it all up requires you to define an address (or a port) where the app should be accessible. The most basic server looks like that:

    package main

    import (
        "net/http"
    )

    func main() {
        http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
            w.Write([]byte("Hello, MyCodeSmells Reader!"))
        })

        http.ListenAndServe(":3000", nil)
    }

Simple, right?

### Useful To Know

`http.ResponseWriter` has only three methods, but they offer you probably all that you need to do with your response: set a header, send a response code and write something in the response itelf. Remember that we are talking about a standard library, so that it's the most basic set of functionality.

### Serving static files

The second thing that you would probably like to do is serve some static files. You might be working on some web application and you need to serve an `index.html` alongside with styles and scripts. To do that you need to add a handler to your server:

    fs := http.FileServer(http.Dir("static"))
    http.Handle("/static/", http.StripPrefix("/static/", fs))

This is the way you can server files that are located relatively to your `main.go`.

### Full Example

    package main

    import (
        "net/http"
    )

    func main() {
        http.HandleFunc("/", func(w http.ResponseWriter, req *http.Request) {
            w.Write([]byte("Hello, MyCodeSmells Reader!"))
        })

        fs := http.FileServer(http.Dir("static"))
        http.Handle("/static/", http.StripPrefix("/static/", fs))
        http.ListenAndServe(":3000", nil)
    }

This little example is available [on Github](https://github.com/mycodesmells/go-http-server)

