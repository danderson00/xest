# Getting Started With `xest` - Part 5 - Vocabulary

So far, we have been embedding expressions inside our components. While developing your application, this provides a
simple way to quickly build up the required set of expressions. However, this doesn't allow us to share expressions
between components, and from a security perspective, allowing the execution of any arbitrary expression could pose a 
security risk.

Vocabulary allows us to define a set of common expressions that can be reused within other expressions. The host can 
also be configured to only allow requests for vocabulary, greatly restricting the API surface and hiding implementation
details.

## Set Up

If you cloned the repository in the previous part, you simply need to switch to the `vocabulary` branch:

```shell
git checkout vocabulary
```

## The Anatomy of Vocabulary

Looking at the `src` directory, we can see a new file, `vocabulary.js`. Let's take a look:

```javascript
module.exports = {
  message: o => o.topic('message'),
  messages: o => o.message().last(5)
}
```

This looks kind of similar to the expression that we embedded directly in our `Room` component in the previous part, 
`o => o.topic('message').last(5)`. Let's break down the differences.

Aside from being declared in a different file, each expression has been given a name. The first reason for this is to 
allow vocabulary expressions to be reused in other expressions. You will see that the `message` expression is reused
in the `messages` expression. Once declared, vocabulary expressions can be used throughout an application. 

The second reason is to allow components to connect directly to vocabulary expressions.

## Consuming Vocabulary From Components

xest provides an alternate syntax for the `connect` HoC that allows you to specify the names of vocabulary 
expressions to consume. Our `Room` component now looks like:

```jsx
export default connect('messages')(
  ({ messages, username, room }) => (
    <div>
...
```  

Instead of the `messages` expression being declared directly on the component, we are now simply passing the name of
the vocabulary we want to evaluate. You can see how the use of scope and vocabulary effectively creates an API
surface for our UI components to consume. 

## Hooking It Up

The xest CLI will look for any vocabulary in `src/vocabulary.js` by default, but we need to restart to host to load it, 
so close any existing host process and start a new one with:

```shell
npx xest -s room
```

## Last But Not Least - Contextual Validation

In the [last part](6-constraints.md) of the guide, we'll show you a simple but powerful mechanism for ensuring
consistency across your domain.