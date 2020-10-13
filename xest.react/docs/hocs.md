# @xest/react Higher Order Components

Each higher order component exposes an additional set of props to the wrapped component, as described below. 

Each component is also implicitly wrapped in a `Scoped` component, meaning that any scope provided with a `scope` prop
is merged into the scope for the current component hierarchy. Scope cascades down to any child components.

A `Provider` component must exist higher in the component hierarchy, as discussed [here](https://danderson00.github.io/xest/#/xest.react/?id=usage). 

## `auth`

Exposes authentication related props to the wrapped component. Requires that the `@x/socket.auth` middleware is
registered. 

prop|Type|Description
---|---|---
authenticate|`function({ username, password })`|Attempt to authenticate. Returns a `Promise` that resolves to the authenticated user. 
createUser|`function(user)`|Create a new user with the corresponding details.
updateUserData|`function(user)`|Update user details.
requestUserVerification|`function({ type, username })`|Request a verification of the specified type. `username` is mandatory if not logged in.
verifyUser|`function({ type, verification })`|Verify the currently logged in user with the specified verification details.
resetPassword|`function({ type, verification, username, password })`|Reset the password of the specified user if the verification details are correct.
user|object|An object containing all user details
authenticated|boolean|True if the user is currently logged in.
logout|`function()`|Log the current user out.

Most functions require you to specify a `provider` property. For more detail, see the 
[`@x/socket.auth` documentation](https://danderson00.github.io/xest/#/socket/).

## `connect(model)`

The `model` parameter is required and must be:

- an object containing key / value pairs where the key is the name of the prop to expose and the value is a stream 
expression to evaluate.
- the vocabulary name to execute. Multiple vocabulary names can be specified.
- a function that receives the component props and returns vocabulary names.

If the `allowRawQueries` host option is set to `false`, the first format will throw an exception. 

### Example

```jsx
import React from 'react'
import { connect } from '@xest/react'

export default connect({
  count: o => o.count(),
  totalValue: o => o.sum('value')
})(
  ({ count, totalValue }) => (
    <div>
      <p>Number of items: {count}</p>
      <p>Total value: {totalValue}</p>
    </div>
  )
)
```

```jsx
import React from 'react'
import { connect } from '@xest/react'

export default connect('sum', 'average')(
  ({ sum, average }) => (
    <div>
      <p>Sum: {sum}</p>
      <p>Average: {average}</p>
    </div>
  )
)
```

## `consumer`

Exposes the ambient `host` object from the closest `Provider` component up the hierarchy as a prop.

For more information on the host, please see the [`@xest/host` documentation](https://danderson00.github.io/xest/#/xest.host/).

## `evaluate`

Exposes an `evaluate` function as a prop. The function has two forms as described below. Both return a `Promise` that
resolves to an `Observable`.

### `evaluate(expression, closures)`

Parameter|Type|Description
---|---|---
expression|[`streamExpression`](https://danderson00.github.io/xest/#/xest.core/docs/coreTypes?id=streamexpression)|A valid `streamExpression` to evaluate.
closures|object|Optional object containing property name / value pairs to expose as closures when evaluating.

If the `allowRawQueries` host option is set to `false`, an exception will be thrown.

### `evaluate(operator, ...parameters)`

Parameter|Type|Description
---|---|---
operator|string|The name of the operator or vocabulary to evaluate.
parameters|any|Parameters to be passed to the operator or vocabulary.

## `publisher`

Exposes a `publish` function to the component as a prop. It accepts a single parameter containing the payload to 
publish and returns a `Promise` that resolves on successful publication.

## `scoped`

Merges any scope provided to the component with a `scope` prop into the ambient scope. This scope cascades to any
child component.