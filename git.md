Reimplementation of Git in Go
=====================================
* Difficulty: Hard
* Knowledge Target: Go's [IO Standard Libraries](http://golang.org/pkg/io/),
	Intimate Understanding of Git, API/Interface design.
* Potential for open source!

## Context

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

## Goal

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


