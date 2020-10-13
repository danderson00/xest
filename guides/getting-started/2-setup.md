# Getting Started With `xest` - Part 2 - The Bare Minimum

In this part of the guide, we'll walk you through creating a minimal xest application.

## Install Dependencies

We're going to use the `create-react-app` bootstrapper to create our application and then install the dependencies we 
need. If you haven't already installed `create-react-app` globally, run `yarn global add create-react-app` first.

```shell
create-react-app chat
cd chat
yarn add @xest/bundle @xest/react sqlite3 @cordelta/react-forms-material
```

Let's take a closer look at our dependencies.

`@xest/bundle` bundles all the packages we need to run a Node.js based host and a browser based consumer into a single
package.

`@xest/react` contains higher order components (HoCs) and hooks for React

`sqlite3` is the underlying storage provider we'll use. For production, you'll want to use a more robust database.
xest supports most relational databases such as MySQL, PostgreSQL and Microsoft SQL Server.

`@cordelta/react-forms-material` takes all of the boilerplate code out of building forms, and makes them beautiful with 
the [`Material UI`](https://material-ui.com/) library.

## Set Up `xest`

We only need `index.js` and `App.js` from the bootstrapped application's `src` directory, so remove everything else.

To initialize xest, we need to wrap our main `App` component in xest's `Provider` component. Replace the contents of 
`src/index.js` with the following:
                                                    
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import consumer from '@xest/bundle'
import { Provider } from '@xest/react'
import App from './App'

consumer().connect()
  .then(host => ReactDOM.render(
    <Provider host={host}><App /></Provider>,
    document.getElementById('root')
  ))
```

In a nutshell, just call the `connect` function on the consumer and wrap our app in a `Provider` component - our 
components can now use any of the 
[higher order components](https://danderson00.github.io/xest/#/xest.react/docs/hocs) and
[hooks](https://danderson00.github.io/xest/#/xest.react/docs/hooks) from the
[`xest.react`](https://danderson00.github.io/xest/#/xest.react/) library.

## Our First Component

Replace the contents of `App.js` with the following:

```jsx
import React from 'react'
import { connect, publisher } from '@xest/react'
import { Form, Text, Submit } from '@cordelta/react-forms-material'

export default publisher(connect({
  messages: o => o.all()
})(
  ({ messages, publish }) => (
    <div>
      <ul>
        {messages.map((message, i) => (
          <li key={i}>{message.text}</li>
        ))}
      </ul>

      <Form onSubmit={publish} display="inline" resetOnSubmit>
        <Text name="text" />
        <Submit>Send</Submit>
      </Form>
    </div>
  )
))
```

Let's break it down. The first thing to notice is the import and use of the `publisher` and `connect` higher order
components (HoCs).

### Querying Messages With the `connect` HoC

The `connect` HoC takes a single parameter, an object that contains a set of these stream expressions we mentioned
before. Here, we just have a single expression called `messages` that simply takes every message published and pushes
it on to an array.

When our component is rendered, xest evaluates each expression and exposes the results in a prop of the same name. The
`messages` prop will contain an array of messages, and we simply map each array item into a list item with the `text` 
property from the message.

<p class="yellowTip">
  You can find a reference for all stream operators, along with the <code>all</code> operator, 
  <a href="#/xest.core/docs/stream.md">here</a>.
</p>

### Publishing Messages

The `publisher` HoC exposes a prop to our component called `publish`. Calling this function sends the provided message
to the host, which validates, persists and distributes it. 

You can see the `publish` function being attached to the `onSubmit` handler on the form. The `onSubmit` handler is 
passed the set of values from the form, in our case just a single `text` property, and forwarded on to the `publish` 
function.

## Start It Up!

That's it! To start our application up, open a console window and start up the host by running:

```shell
npx xest
```

<p class="yellowTip">
  There are a number of different ways of configuring and starting the host to suit your needs. See the 
  <a href="#/xest.bundle/?id=host-startup-configuration-options">bundle documentation</a> for more information.
</p>

Once the host has started, open another console window and run the start script:

```shell
yarn start
```

This should open your default browser with our application running! Try opening multiple browser tabs to 
`http://localhost:3000` and sending some messages. xest keeps all of our expressions up to date in real time seamlessly.

The code for this minimal setup is available at https://gitlab.com/danderson00/xest.sample.chat/tree/minimal.

[Next up](3-scope.md), we look at how to keep our components focused on the right set of messages using "scope".
