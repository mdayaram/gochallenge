Write a Reimplementation of Git in Go
=====================================
* Difficulty: Hard
* Knowledge Target: Go's [IO Standard Libraries](http://golang.org/pkg/io/),
	Intimate Understanding of Git, API/Interface design.
* Potential for open source!

#### Context

This would've actually been a lot more applicable a year ago.  There's a lot
less emphasis on git now-a-days within the platform, regardless, this would be
a pretty cool project and would earn mad open source points in the community.

Basically, there have been [tons of libraries](http://godoc.org/?q=git) written
in Go to interface Git related operations.  However, all of these libraries are
either Go code executing `git` commands using `exec` or Cgo bindings to the C
library [libgit2](http://libgit2.github.com/).  There's a huge deficit on
a robust go-native library for git operations.

There's a nice [stack overflow
question](http://stackoverflow.com/questions/4034962/which-language-has-the-best-git-api-bindings)
covering the different types of Git implementations in different languages.

What's the benefit of having a Go reimplementation of Git?  Technically
speaking, the cost of jumping from Go to C-land is not negligible.  Having an
implementation of Git that is purely in Go would be theoretically faster.
Culturally speaking, a language's maturity is usually measured by the breadth
and depth of the native libraries written for it.  Writing a reimplementation of
Git in Go helps us push the language towards a higher level of maturity in the
field.

#### Goal

Your goal is to write a reimplementation of Git in Go.  Though there already are
a lot of Go Git packages that bind to libgit2, there's a [very
limited](https://github.com/toqueteos/git) and
[incomplete](https://github.com/edsrzf/go-git) set of packages that reimplement
all of Git in Go.

This will require a deep understanding of how Git works.  In fact, it would
most likely require a deeper knowledge on how Git works more so than how to
write Go code.  One of the major reasons why I decided to label this as "Hard"
was mostly for the amount of research one has to do regarding the way Git works.

I would first approach this by figuring out what the lowest level commands Git
has are and then begin implementing those first.  Once you have those done, you
can go ahead and build on top of them to make the more complex level commands.

In terms of Go, this project is a great case study on code architecture and API
design.  If done properly, you can use Go's structures, types, and interfaces
to your advantage.  I would take a look at some of the above linked open source
projects for inspiration.


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

For pure beginners, a great place to start and get your bearings is in the [How
to Write Go Code](http://golang.org/doc/code.html) tutorial which teaches you
the essentials on how to organize your code, important environment variables to
set, and general code structure.

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

* [How to Write Go Code](http://golang.org/doc/code.html) to get started in Go.
* [Golang Tour](http://tour.golang.org/) to learn the basics of the language.
* [Effective Go](http://golang.org/doc/effective_go.html) for a deeper dive.
* [Golang Playground](http://play.golang.org/) for testing snippets of code.
* [Online API Docs](http://golang.org/pkg/) or the [godoc](http://golang.org/cmd/godoc/)
	tool for standard library references.

### Getting Help

Probably unnecessary to mention, but I'm always available to help you guys out
if you're feeling stuck or just need help getting setup, started, or finishing.
Feel free to drop me an email/chat/call whenever you think you could use some
help.
