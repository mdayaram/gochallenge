Go Challenge
============

I've had many requests from people for projects that they could do that would
teach them some fundamentals in Go programming.  Below is a list of problems
that I've come up with which would be relatively useful for
[moovweb](http://github.com/moovweb) but isolated enough that you wouldn't need
to know anything about our internal code base.


## Challenge Projects

### HTTP File Server
--------------------
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


### Jello Server
----------------------
* Difficulty: Medium
* Knowledge Target: Understand how moovweb's server works, go routines,
	channels.

#### Context

This project would not be directly applicable to anything we currently do in the
platform team, however, for those interested, it should help you understand how
our server architecture works.  The server architecture we've decided upon was
birthed through several conversations, discussions, and prototypes.  In the end,
we ended up leveraging a lot of Go's concurrency constructs in order to get
something that is resource efficient.

So what is the architecture like?  Let's start out by defining what our server
needs to do.  When our server receives a request, it needs to perform a series
of tasks on that request:

1. We rewrite the request
2. We forward the rewritten request to the upstream server
3. We rewrite the response

Finally, after rewriting the response we've received from the upstream, we
respond to the original client with the rewritten response.

Now, to understand our constraints better, let's reduce each one of these tasks
down to the type of resource they would need in order to execute:

1. CPU Work<sub>1</sub>
2. IO Block
3. CPU Work<sub>2</sub>


**The Generalist Approach**

Every request that comes in as a new goroutine and that goroutine goes through
each one of the tasks it needs to execute.  This is great in the sense that we
can potentially service unlimited requests at a time (well, technically the
maximum number of live goroutines, but for our intents and purposes that's
unlimited).  This was version 2 of our production server, however, we quickly
moved away from that and onto something else.

Consider a goroutine as a generalist waiter, and every request that comes in is
an order from a customer.  For every new order of jello, we hire a new waiter.
Being the generalists that they are, our waiters can do it all.  Not wasting any
time right after being hired, they go straight to work.  They pick all the
ingredients out, mix everything in a bowl and throw it in the fridge for 20
minutes or so.  Once they're done waiting for the jello to set, they give it a
little jiggle test to make sure it's of the right consistency, plate it
perfectly, and then send it off to be served to our lovely customers.

A small break in the analogy is that once that jello is in our customer's hands
we fire our waiter, because, hey, waiters are cheap, at least in Go they are,
so why bother paying them when you can just keep hiring new ones and fire them
before they unionize?

*What are the major drawbacks of this approach?*  Think about the physical
constraints of being a waiter in the kitchen.  Remember that we hire a new
waiter every time we get an order in, even if there are already several waiters
in the kitchen, each one working on their own order.

One obvious variation to this approach would be to start off by hiring a fixed
number of waiters at the start of the restaurant's opening and only have them
taking orders and cooking jello.

*What new constraints are imposed on our system given this new variation?*
Think about what problems from above this variation solves.  Are any new
problems created?  How does having a fixed number of waiters limit our
restaurant's output?


**The Specialist Approach**

For this approach, every request will come in as a new goroutine just as with
the obvious approach.  However, instead of going ahead and doing the first
batch of work itself, this goroutine queues the necessary parts of the request
that require work to be done onto a channel and then waits for a response.
Listening on the other side of that channel is a fixed pool of goroutines whose
sole job is to do the following:

1. Pick a piece of work from the work channel.
2. Perform the work.
3. Take the result of the work and respond back to the original goroutine that
	 queued the work.
4. Repeat.

After the original goroutine receives a response from a worker, it goes ahead
and performs the IO blocking operation.  Once that is done, it queues the
necessary elements for work onto a different channel where another set of worker
goroutines are all polling from in order to perform Work<sub>2</sub>.  This set
is identical to the first worker pool except for the actual work
being performed.

In our restaurant anology, this is the equivalent of hiring specialist for each
job.  At the start of the restaurant, we basically make a decision of "how many
jello mixers do we want?" and "how many plating specialists and jello testers do
we want?"  Once we've made a decision, let's say, four each, then we hire that
many chefs that specialize in those aspects.

When an order comes in, we again hire a waiter to take that order, but
instead of cooking the jello themselves, the waiter queues up the order to the
jello mixing chefs.  Now, we only have a few of them, so they can't start making
the jello right away, but as soon as one of them is free, they'll take up the
next order and start mixing the ingredients.  Once everything is perfect,
they'll send back a bowl of mixed jello ingredients to the waiter.  The waiter
then goes ahead and sticks the mixture in the fridge and waits for 20 minutes
for it to set.  Once it's set, the waiter takes the bowl and queues it up onto
the group of plating specialist chefs.  Again, we only have a few of them, so it
may take a bit for them to take the order, but once one of them is free, they'll
take the bowl, scoop the jello out, plate it, and hand it back to the waiter.
The waiter can now take the jello back to the customer who ordered it.

A job well done!  Unfortunately for our poor waiter, they will not be taking any
more orders.  We gather their things and let them go.  Goodbye dear waiter,
you'll be missed!

*What are the tradeoffs of this approach compared to the generalist?*  There are
problems that were present in the generalist approach that are no longer present
here, and there are new problems that were not present in the generalist
approach that we have introduced with this new model.  What are some major
differences in terms of rate of output and how many concurrent orders we can
take in?

Again, we can do the obvious variation here that we did in the generalist
approach and hire a fixed number of waiters at the start of our restaurant and
just keep reusing them to take orders.

*What do we gain from adding this new variation?*  Think about what you gained
and lost by adding this variation in the generalist approach.  Are those gains
and losses the same here?  How do they change given our new model.


I've left the above questions somewhat open ended to provide you with the same
thought experiments we had to deal with when deciding our architecture.
I encourage all of you to think about these question and try to come up with
your own answers.  However, engineering (though it may not seem like it) is a
social endeavor, so I encourage you to discuss possible solutions amongst
yourselves.  I'm also available for discussions if you'd like to validate your
answers or have any more questions you'd like to raise.


#### Goal

In addition to participating in the above thought experiments, your goal is to
write a web server in Go with the architecture described in the **Speacialist
Approach**.  For simplicity's sake, you can choose whatever you wish for the
"work" and the "blocking operation" that you need to do.  In our web server, the
work and blocking operations consist of rewriting strings and going upstream.
However, a simple example could be as follows:

1. CPU Work<sub>1</sub>: Generate a list of random words.
2. IO Block: Read a random file from disk.
3. CPU Work<sub>2</sub>: Count all occurrences of each random word in the file.

Anything creative would suffice, as long you maintain the constraints that the
work must be CPU intensive and the blocking operation is...well, just that,
a blocking operation.


### Flag Parsing Package
------------------------
* Difficulty: Medium
* Knowledge Target: Go's [Reflection Standard
	Libraries](http://golang.org/pkg/reflect/)

#### Context

placeholder

#### Goal

placeholder


### Write a Reimplementation of Git in Go
-----------------------------------------
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
