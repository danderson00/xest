# Stream Operators

Stream operators apply to normal streams of values and are the default set of operators applied to observables.

## `accumulate(expression)`

Performs a shallow merge of objects in a stream into a single object.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Determine object to merge|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.accumulate()
source.publish({ p1: 1 })
source.publish({ p2: 2 })
// { p1: 1, p2: 2 }
source.publish({ p1: 3, p3: 3 })
// { p1: 3, p2: 2, p3: 3 }
```

## `all()`

Aggregates all stream values into an array.

### Parameters

None

### Result Type

`aggregate`
### Examples

```javascript
const source = subject()
const o = source.all()
source.publish({ p1: 1 })
source.publish({ p2: 2 })
// [{ p1: 1 }, { p2: 2 }]
source.publish({ p1: 3 })
// [{ p1: 1 }, { p2: 2 }, { p1: 3 }]
```

## `average(expression)`

Calculates the average of stream values. An expression or property name can be 
supplied to map stream values to input values.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate|Expression to determine value to average|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.average()
source.publish(1)
source.publish(3)
source.publish(5)
// 3
```

```javascript
const source = subject()
const o = source.average('value')
source.publish({ value: 1 })
source.publish({ value: 3 })
source.publish({ value: 5 })
// 3
```

```javascript
const source = subject()
const o = source.average(x => x.value)
source.publish({ value: 1 })
source.publish({ value: 3 })
source.publish({ value: 5 })
// 3
```

## `combine(...operators)`

Combine the specified operators / vocabulary into an object.

### Parameters

|Name|Type|Description|
|-|-|-|
|operators|[string]|Operator or vocabulary names to apply|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.combine('count', 'sum')
source.publish(1)
source.publish(2)
source.publish(3)
// { count: 3, sum: 6 }
```

## `compose(expression, expression2, ..., aggregator)`

Allows multiple expressions over the same stream to be composed into a single stream. It has two signatures - a number 
of stream expressions that are evaluated and passed into the last function parameter supplied, or an object containing 
properties with expressions to be evaluated and mapped into another object of the same structure.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|streamExpression\|object|Stream expressions are evaluated and passed to the aggregator.\Each property of an object is evaluated and mapped to another object|
|aggregator|function|Transforms evaluated stream expressions. Each expression is passed as a parameter.

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.compose(
  o => o.sum(),
  o => o.count(),
  (sum, count) => sum() / count()
)
source.publish(1)
source.publish(3)
// 2
source.publish(5)
// 3
```

```javascript
const source = subject()
const o = source.compose(
  sumOfEvens: o => o.where(x => x % 2 === 0).sum(),
  double: o => o.map(x => x * 2)
)
source.publish(2)
source.publish(3)
// { sumOfEvens: 2, double: 6 }
source.publish(4)
// { sumOfEvens: 6, double: 8 }
```

## `count()`

Counts the number of stream values that have been published.

### Parameters

None

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.count()
source.publish(1)
source.publish(1)
source.publish(1)
// 4
```

## `groupBy(expression, groupExpression, removeExpression)`

Splits a single stream into multiple streams by a key generated by an expression or property name.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate|Determines the key used to group by|
|groupExpression|streamExpression|A stream expression to apply to each group|
|removeExpression|streamExpression|A stream expression that removes the group when it evaluates to be truthy|

### Result Type

`stream`

### Examples

Split a stream into multiple streams based on the `category` property:

```javascript
const source = subject()
const o = source.groupBy('category')
source.publish({ category: 'Components', name: 'Combobulator' })
source.publish({ category: 'Components', name: 'Reionizer' })
source.publish({ category: 'Widgets', name: 'Decoupler' })
// [{ category: 'Components', name: 'Reionizer' }, { category: 'Widgets', name: 'Decoupler' }]
```

Extract all `name` properties for each group:

```javascript
const source = subject()
const o = source.groupBy('category', o => o.select('name').all())
source.publish({ category: 'Components', name: 'Combobulator' })
source.publish({ category: 'Components', name: 'Reionizer' })
source.publish({ category: 'Widgets', name: 'Decoupler' })
// [['Combobulator', 'Reionizer'], ['Decoupler']]
```

Accumulate details for each group, removing when deleted:

```javascript
const source = subject()
const o = source.groupBy('productId', 
  o => o.topic('product').accumulate('details'),
  o => o.topic('productRemoved')
)
source.publish({ topic: 'product', productId: 1, details: { name: 'Nail', price: 0.02 } })
source.publish({ topic: 'product', productId: 2, details: { name: 'Hammer', price: 1.23 } })
source.publish({ topic: 'product', productId: 3, details: { name: 'Toilet paper', price: 6.01 } })
source.publish({ topic: 'product', productId: 1, details: { price: 0.03, note: 'Price increase!' } })
source.publish({ topic: 'productRemoved', productId: 3, reason: 'Too expensive...' })
// [{ name: 'Nail', price: 0.03, note: 'Price increase!' }, { name: 'Hammer', price: 1.23 }] 
```

Group based on a number being odd or even, calculating the average of each group:

```javascript
const source = subject()
const o = source.groupBy(
  x => x % 2 === 0,
  o => o.average()
)
source.publish(1)
source.publish(2)
source.publish(3)
source.publish(4)
source.publish(5)
// [9, 6]
```

## `last(count)`

Aggregate the specified number of most recently published messages into an array.

### Parameters

|Name|Type|Description|
|-|-|-|
|count|number|The number of messages to retain|

### Result Type

`aggregate`

### Examples

```javascript
const source = subject()
const o = source.last(2)
source.publish({ p1: 1 })
// [{ p1: 1 }]
source.publish({ p1: 2 })
source.publish({ p1: 3 })
// [{ p1: 3 }, { p1: 2 }]
```

## `map(expression)`

Transform messages from one form to another.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|predicate|An expression to transform an input value|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.map(x => x * 2)
source.publish(1)
// 2
source.publish(6)
// 12
```

```javascript
const source = subject()
const o = source.map(orderLine => ({ ...orderLine, total: orderLine.quantity * orderLine.price }))
source.publish({ productId: 1, price: 1.01, quantity: 2 })
// { productId: 1, price: 1.01, quantity: 2, total: 2.02 }
```

## `reduce(reducer, initialValue)`

Perform a reduce operation over each stream value.

### Parameters

|Name|Type|Description|
|-|-|-|
|reducer|function|A function that accepts the reduced value and next stream value|
|initialValue|any|The initial reduced value to pass to the reducer|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.reduce((sum, nextValue) => sum + nextValue, 0)
source.publish(1)
source.publish(2)
source.publish(3)
// 6
```

## `select(property)`

Pluck a value from an object by the specified property.

### Parameters

|Name|Type|Description|
|-|-|-|
|property|propertyRef|Reference to the property to select|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.select('value')
source.publish({ id: 1, value: 2 })
// 2
```

## `sum(expression)`

Sum the values yielded by the specified expression.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate|Determines the value to sum|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.sum()
source.publish(1)
source.publish(2)
source.publish(3)
// 6
```

```javascript
const source = subject()
const o = source.sum(x => x.value)
source.publish({ value: 1 })
source.publish({ value: 2 })
source.publish({ value: 3 })
// 6
```

## `topic(name)`

Filter messages based on the `topic` property. Shorthand for `where(x => x.topic === value)`.

### Parameters

|Name|Type|Description|
|-|-|-|
|topic|string|Message topic to include|

### Result Type

`stream`

### Examples

```javascript
const source = subject()
const o = source.topic('product')
source.publish({ topic: 'product', productId: 112 })
source.publish({ topic: 'order', orderId: 772 })
// { topic: 'product', productId: 112 }
```

## `where(expression)`

Filter messages based on an expression or hash of values.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|predicate\|object|Predicate to filter by, or an hash of values to match|

### Result Type

`stream`

### Examples

Filter based on a predicate

```javascript
const source = subject()
const o = source.where(x => x.value > 3)
source.publish({ id: 1, value: 1 })
source.publish({ id: 2, value: 12 })
source.publish({ id: 3, value: 4 })
source.publish({ id: 4, value: 2 })
// { id: 3, value: 4 }
```
