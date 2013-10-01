Go Challenge
============

I've had many requests from people for projects that they could do that would
teach them some fundamentals in Go programming.  Below is a list of problems
that I've come up with which would be relatively useful for
[moovweb](http://github.com/moovweb) but isolated enough that you wouldn't need
to know anything about our internal code base.

## Challenge Projects

### HTTP File Server

* Difficulty: Easy
* Knowledge Target: Go's [HTTP Standard
	Libraries](http://golang.org/pkg/net/http/).

#### Context

Currently we've been using [web.go](https://github.com/hoisie/web) as our file
server when serving local assets in the SDK.  However, web.go was written at
a time when Go's standard libraries lacked a solid http package.  The release of
Go 1.0 included a wealth of very robust and flexible [http
libraries](http://golang.org/pkg/net/http/).

#### Goal

Your goal is to write a simple yet robust http file server.  This file server
should take in as input a root directory in which to serve files from.  You
should provide an interface in Go for creating an instance of your server and
starting it programmatically.

### Flag Parsing Package

* Difficulty: Medium
* Knowledge Target: Go's [Reflection Standard
	Libraries](http://golang.org/pkg/reflect/)

#### Context

placeholder

#### Goal

placeholder

### Write a Reimplementation of Git in Go

* Difficulty: Hard
* Knowledge Target: Go's [IO Standard Libraries](http://golang.org/pkg/io/),
	Intimate Understanding of Git, API/Interface design.

#### Context

placeholder

#### Goal

placeholder

## Resources

### Download and Install Go

If you're new to Go, you'll probably need to install it on your computer before
you can get started working with it.  Unlike most popular languages such as C,
ruby, and Python, Go does not come pre-installed in most systems.

You can download and install Go using a variety of methods:

* Follow the install instructions at Go's [official site](http://golang.org/doc/install).
* If running Mac OSX, install using `brew install go`.
* If running Ubuntu, install using `sudo apt-get install golang`.
* Use [GVM](https://github.com/moovweb/gvm) to install Go in a sandboxed
	environment.

### Learning about Go

If you haven't already, I would strongly recommend you take the [Golang
Tour](http://tour.golang.org/).  It's a very well done tour that teaches you the
essentials of the language without bogging you down with any of the details.

Another really good resource to have is the [Golang
Playground](http://play.golang.org/) which lets you try things out quickly to
get immediate feedback.  It is very much akin to our very own [Tritium
Tester](http://tester.tritium.io/).

For those interested in a deeper dive, [Effective
Go](http://golang.org/doc/effective_go.html) provides more details with regards
to how things work and what good idiomatic Go code looks like as well as a lot
of `gotchas`.

Finally, Go's online [API documentation](http://golang.org/pkg/) is very
thorough and essential when working with any of the standard libraries.  They do
a really good job letting you know the itty gritty, as well as even showing you
the source code for everything.  Along these lines, you can also use the command
line tool [godoc](http://golang.org/cmd/godoc/) for all your API doc needs.

So, to summarize:

* [Golang Tour](http://tour.golang.org/) to get your started in Go.
* [Effective Go](http://golang.org/doc/effective_go.html) for a deeper dive.
* [Golang Playground](http://play.golang.org/) for testing snippets of code.
* [Online API Docs](http://golang.org/pkg/) or the [godoc](http://golang.org/cmd/godoc/)
	tool for standard library references.

### Getting Help

Probably unnecessary to mention, but I'm always available to help you guys out
if you're feeling stuck or just need help getting setup, started, or finishing.
Feel free to drop me an email/chat/call whenever you think you could use some
help.
