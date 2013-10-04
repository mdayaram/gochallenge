Flag Parsing Package
====================
* Difficulty: Medium
* Knowledge Target: Parsing args, pointers, defining structures and types.
* Potential for open source!

## Context

Go comes with a [flag parsing package](http://golang.org/pkg/flag/) in its
standard library.  However, after much use, we've found it to be very limiting.
Below is a list of things we *wished* Go's flag parsing package would have but
that we haven't really had the time to implement.

1. A mode where we can continue parsing flags even if we reach a non-flag
	 argument.
2. Be able to set a flag variable to a given numbered argument (rather than just
	 by specifying the flag explicitly.
	* Example: the project path can be set by using the `--project-path=<path>`
		flag or will default to the first non-flag argument.
3. Let a single flag variable be assignable by multiple command line flags via
	 some logic.
	* Example: make `--ssl` set the `ssl` variable to true, but also have
		`--no-ssl` set the `ssl` value to false.
4. Have the concept of a "hidden" flag that can be used to set variables but
	 that is hidden from the user (it does not show up in the help message/usage).

We've created a tiny framework around Go's current flag parsing package which
allows us to do all these things, but it would be much more pleasant if we had
a flexible flag parsing package which came with all these options by default.

Assuming we're not the only ones suffering through the default flag's package
inadequacies, this would make a great open source project.

## Goal

Your goal is to implement a flag parsing package with as many of the above
listed features (perhaps even more if you can think of any!) in Go.  It's
definitely possible to leverage Go's flag parsing package when implementing your
own.  I would also do some research on what the defacto flag parsing packages
for other programming languages are and how they are used.  There's a lot of
research that can be done here, and though the initial project's scope is
somewhat small, it has great potential of becoming a powerful package.

