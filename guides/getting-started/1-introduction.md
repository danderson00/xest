# Getting Started With `xest` - Part 1 -  Introduction

This guide will run you through the basic concepts involved in creating an application with xest. It will cover basic 
concepts such as observability and expressions and how they are consumed within a React application.

A basic level of knowledge of React is assumed, but examples should be simple enough for even newcomers to understand.

## What is `xest`?

You can think of xest as a queryable event database. A host process exposes an API that can be used to publish new
events and query those published events. That host might be running inside your browser, or it might be running on a
remote server connected by a web socket.

Publishing events is as simple as calling the `publish` function on the host. It will perform any necessary validation
of the event message, persist the message to a database and broadcast the message to any listening queries.

Querying uses a fluent API similar to that used by [`ReactiveX`](http://reactivex.io/). While 
[`ReactiveX`](http://reactivex.io/) has a very formal API deeply rooted in mathematical theory, xest is focused 
on extensibility, reuse and composition. Event streams can be filtered, transformed and composed into models with an 
extensible vocabulary. Models can then be consumed directly by user interface components. 

## Observability

The core type that underpins all functionality in xest is an `observable`. An observable represents a stream of 
values and is like a window into the most recent value from the stream. You can subscribe to changes in the current
value of a stream by calling the `subscribe` function with a callback.

Observables can also be fully serialized, allowing both the definition and state of observables to be persisted to a
permanent storage medium, or transmitted over a network.

Most of the boilerplate code associated with observability and serialization is abstracted away when using React 
bindings, allowing you to focus on the core business logic and user interface of your application. 

## Stream Expressions

xest extends the observable object with "operators" that can be chained together to form expressions. An expression
typically takes the form of an anonymous function that is passed an observable instance. For example, a simple
expression that calculates the total of all values over 3 might look like:

```javascript
o => o.where(x => x > 3).sum()
```

xest allows you to declare smaller expressions as reusable "vocabulary" and build increasingly complex expressions
from them. More on this later. For now, we'll start building on these simple concepts.

## Let's Get Started!

In the [next part](2-setup.md), we'll show you the minimal setup for simple real time chat with xest.
