# Getting Started With `xest` - Part 6 - Constraints

Constraints are a simple but powerful mechanism for ensuring contextual consistency across your domain. They allow you
to write expressions that must evaluate to true before a message is allowed to be published.

As a simple example, imagine a bank account that must not drop below a zero balance. We could write a simple expression
to achieve this effect:

```javascript
o => o.sum('value').map(x => x >= 0)
```

Notice the use of the `map` operator - this ensures that each value that is passed through the expression is mapped to
a true or false value.

Constraints allow you to perform validation of user input based on the context in which it is received, providing much
more power and flexibility over traditional static input validation.

Let's look at an example in our chat application.

## Limiting User Activity

Lets say we wanted to limit the number of messages a user can publish over a certain time. In our `Send` component, 
we've added a new property to the message that's being published - `publishedAt` - which is a simple time stamp.

Let's have a look at the newly added `src/constraints.js` file:

```javascript
module.exports = [
  {
    expression: o => o
      .topic('message')
      .last(4)
      .where(x => x.publishedAt > Date.now() - 5000)
      .count()
      .map(x => x <= 3),
    scopeProps: ['room', 'username'],
    topics: ['message'],
    failureMessage: `You cannot publish more than 3 messages per 5 seconds`
  }
]
``` 

The `expression` property specifies the expression to use when validating each message. This is a moderately complex
expression that counts the number of previous messages that have been published within the last 5 seconds and returns 
true if there are 3 or less.

<p class="yellowTip">
  For a guide on the fundamentals of expressions, please check out  
  <a href="#/guides/expressions/1-introduction.md">Modelling With Expressions</a>.
</p>

The `scopeProps` property specifies how the expression should be scoped. With this configuration, any message with
`room` and `username` properties will trigger validation, and the expression will be scoped by those properties.

Without a `topics` property, every message in the scope will trigger validation. Providing a list of topics here limit
the messages that trigger validation. Note that the expression will still be fed every message in the scope, not just
those with specified topics.

The `failureMessage` property specifies a custom message to return if validation fails. This can also be a function 
that is passed the message that triggered validation.

<p class="yellowTip">
  You can also specify a <code>filter</code> property that allows you to provide an arbitrary function that determines
  when the constraint should be triggered.
</p>

## Starting It Up

Similar to vocabulary, xest will look for constraints declared in `src/constraints/js` by default, but we need to 
restart to host to load these. We also need to add the additional scope declared in the constraint, `username`:

```shell
npx xest -s room -s username
``` 

## Conclusion

That's it for getting started with xest! 

We've got plenty more resources for learning how to build applications with xest, and we highly recommend you check out 
the [next guide](/guides/expressions/1-introduction.md) on modelling with expressions. It introduces you to three 
powerful operators that are fundamental for transforming your events into concise, expressive models for your 
applications.

- [Modelling With Expressions guide](/guides/expressions/1-introduction.md)
- [Stream operator reference](/xest.core/docs/stream.md)
- [Aggregate operator reference](/xest.core/docs/aggregate.md)
- [React HoC reference](/xest.react/docs/hocs.md)
- [React hooks reference](/xest.react/docs/hooks.md)
- [Bundle documentation and CLI reference](/xest.bundle/)
