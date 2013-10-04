HTTP File Server
================
* Difficulty: Easy
* Knowledge Target: Go's [HTTP Standard
	Libraries](http://golang.org/pkg/net/http/).

## Context

Currently we've been using [web.go](https://github.com/hoisie/web) as our file
server when serving local assets in the SDK.  However, web.go was written at
a time when Go's standard libraries lacked a solid http package.  The release of
Go 1.0 included a wealth of very robust and flexible [http
libraries](http://golang.org/pkg/net/http/).

## Goal

Your goal is to write a simple yet robust http file server.  This file server
should take in as input a root directory in which to serve files from.  You
should provide an interface in Go for creating an instance of your server and
starting it programmatically.

