# Getting Started With `xest` - Part 3 - Scope

Scope is a simple filtering mechanism that restricts the messages a component receives and sends. 

For example, if a component has a scope of `{ room: 'General' }`, it will only receive messages that contain a `room` 
property with a value of `'General'`. Additionally, any messages published from a scoped component will have the same 
scope values attached.

Let's illustrate this with a simple example - chat rooms.

## A Simple Lobby

Using the same application we used in the previous part, replace the code in `App.js` with the following:

```jsx
import React, { useState } from 'react'
import { Form, Text, Submit } from '@cordelta/react-forms-material'
import Room from './Room'

export default () => {
  const [room, setRoom] = useState()

  return <div>
    <Form onSubmit={setRoom} display="inline">
      <Text name="user" label="User Name" />
      <Text name="name" label="Room" required />
      <Submit>Join</Submit>
    </Form>

    {room && <Room
      scope={{ room: room.name }}
      username={room.user || 'Anonymous'}
    />}
  </div>
}
```

The first part of the component is a form that captures a user name and a room name and assigns them to a state
variable when the `Submit` button is clicked.

The second part is what we're interested in - if the room state has been set, render a `Room` component, passing a
`scope` prop and a `username` prop from the state. Let's look at a `Room` component and see how it is affected by the
`scope` prop.

## Our First Scoped Component

Create a file called `Room.js` in the `src` directory containing the following code:

```jsx
import React from 'react'
import { connect } from '@xest/react'
import Send from './Send'

export default connect({
  messages: o => o.topic('message').last(5)
})(
  ({ messages, username, room }) => (
    <div>
      <h3>Hello, {username}, welcome to the {room} chat room!</h3>
      <ul>
        {messages.map((message, i) => (
          <li key={i}>
            <b>{message.username}</b> says <b>{message.text}</b>
          </li>
        ))}
      </ul>
      <Send username={username} />
    </div>
  )
)
```

Here we are using the `connect` HoC with another simple expression that provides our component with the last five 
messages with a `topic` property set to `message`. Along with a simple greeting, these messages are rendered as list
items from properties on the message. We'll look at the `Send` component shortly.

The key point here is that our `messages` expression does not need to filter messages based on the room name - this is
done by the scoping mechanism based on the `scope` prop that we passed to the component. 

<p class="yellowTip">
  Scope values are spread on to the props for a component, seen here with the <code>room</code> prop. You can also 
  access the scope object through the <code>scope</code> prop.
</p>

## Publishing From Scoped Components

Let's take a look at the `Send` component.

```jsx
import React from 'react'
import { publisher } from '@xest/react'
import { Form, Text, Submit } from '@cordelta/react-forms-material'

export default publisher(
  ({ publish, username }) => (
    <Form display="inline" resetOnSubmit onSubmit={
      ({ text }) => publish({ topic: 'message', text, username })}
    >
      <Text name="text" label="Message" />
      <Submit>Send</Submit>
    </Form>
  )
)
```

This is pretty much the form from the first part, except we're attaching some additional properties to the published
message. In a nutshell, capture some text and publish a message containing a topic, the provided username and the text.

Again, you'll notice that we're not attaching any `room` property to the published messages - the scope from the parent
component cascades down to our component and is attached to outgoing messages. This implicit filtering and mutating of 
messages *decouples* components from the context in which they are used.

To illustrate a little better how this is useful, imagine a component that displays a summary of a table tennis 
player's performance - point win / loss ratio, longest winning streak, etc. By simply passing a different scope value, 
we can use the same component to display statistics for a single match, a tournament or a player's entire career.

## Starting It Up!

Close down any other host processes that you started in the previous step and start a new one, this time with the
following command:

```shell
npx xest -s room
```

The `-s` option tells the host to set up the relevant database indexing so it can handle requests for scopes with the
`room` property.

## Next up - Getting Around

The code for this part is available at https://gitlab.com/danderson00/xest.sample.chat/tree/scope.

The [next part](4-navigation.md) of the guide looks at using published messages to create a navigation graph for our
application.
