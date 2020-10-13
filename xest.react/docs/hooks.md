# @xest/react Hooks

## `useObservable(observable)`

Manages the component's subscription to the observable, rerendering the component as the observable value changes and
unsubscribing from the observable when the component is unmounted. The hook returns the current value of the 
observable.

### Example

```jsx
import React from 'react'
import { useObservable } from '@xest/react'

export default ({ observable }) => {
  const value = useObservable(observable)
  return (
    <div>{value}</div>
  )
}
```

## `useEvaluate(expression, scope, initialClosures)`

Returns an array, the first element being the results of any evaluation, the second being a function that accepts
closures as an optional parameter and triggers evaluation of the supplied expression with closures.

If `initialClosures` are specified, the expression is evaluated with the specified closures when the component is 
mounted.

Unlike higher order components, evaluations are not subject to ambient scope. Scope must be manually passed to the
`useEvaluate` hook. You can use the [`scoped`](./hocs.md?id=scoped) HoC to obtain the ambient scope.

### Example

```jsx
import React from 'react'
import { useEvaluate } from '@xest/react'
import { Form, Select, Submit } from '@cordelta/react-forms-material'

export default () => {
  const [results, search] = useEvaluate(
    o => o.topic('product').accumulate('details').where({ category })
  )

  return (
    <div>
      <Form onSubmit={search}>
        <Select values={['Components', 'Widgets']} label="Category" />
        <Submit>Search</Submit>
      </Form>
      <ul>
        {results.map(product =>
          <li key={product.productId}>
            <h2>{product.name}</h2>
            <h3>{product.category}</h3>
            <p>{product.description}</p>
          </li>
        )}
      </ul>
    </div>
  )
}
```

## `useEvaluate(operator, scope, ...initialParameters)`

This form of the `useEvaluate` hook operates identically to the above form, except the `operator` parameter specifies
the operator or vocabulary name to execute. Similarly to the other form, if `initialParameters` are specified, the
evaluation is executed when the component mounts.