# @xest/core API Reference

## Construction / Default Instance

A default, static instance of the `xest` object is available by requiring or importing `@xest/core`. A new instance can
be created by calling the `construct` function. The default instance is used throughout the platform unless an
alternate is provided through specified mechanisms.

```javascript
const { subject, construct } = require('@xest/core')
const source = subject()

const xestInstance = construct()
const source2 = xestInstance.subject()
```

## Core Functions

### `observable(publisher, options)`

Create a new instance of an observable. `publisher` should be a function that accepts a single parameter. This function
is passed a `publish` function that allows injection of values into the observed stream.

### `subject(options)`

Create a new instance of a `subject` observable. This special observable has an extra property called `publish` that
allows injection of values into the observed stream.

### `proxy(inputObservable, options)`

Wrap an existing observable in an additional observable, allowing injection of additional options. The result 
observable also has an additional property called `disconnect` that allows unsubscribing from the parent observable.

### `fromEmitter(eventEmitter, ...eventNames)`

Create an observable from an event emitter using the specified event names. Both Node.js style 
[EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter)s and browser style
[EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)s are supported.

If a single event name is specified, events are passed on as messages untouched. If more than one event name is 
specified, events are mapped into a structure with a `topic` property matching the event name and a `data` property
containing the event data.

### `merge(...inputObservables)`

Merge several observables into a single observable. All events from all provided observables are published through the
new observable.

## Vocabulary / Operators

### `addOperator(operator)`

Add a new operator to the `xest` instance. The provided `operator` object must have the following properties:

Name|Type|Description
---|---|---
type|string|The type of observable the operator applies to (either `stream` or `aggregate`)
identifier|string|A unique identifier for the operator. The operator is extended onto target observables using the identifier as the property name.
component|object|A function that accepts an observable and returns the operator definition. See below.

A operator definition must have two properties that are both functions:

`create(...args)`

Construct a new instance of an observable with the operator applied from the provided parameters.

`unpack(serialzationInfo)`

Construct a new instance of an observable with the operator applied from the provided serialization information. 

### `addVocabulary(vocabulary)`

Add new vocabulary to the `xest` instance. The function takes two forms, as an object with `name` and `expression`
properties, or as a hash with property names corresponding with vocabulary names and values being the associated
expression.

## Serialization

### `serialize(observable)`

Obtain the [`serialization information`](/xest.core/docs/coreTypes.md?id=serializationinfo) for the provided observable.

### `deserialize(parent, serializationInfo)`

Construct a new instance of an observable from the provided serialization information with the provided parent
observable.

### `unzip(observable)`

Split the definition and state from each serialization frame for the provided observable into an object with 
`definition` and `state` properties.

### `zip(parent, definition, state)`

Construct a new instance of an observable from the provided definition and state with the provided parent.

### `extractDefinition(expression, options)`

Extract the definition of the provided stream expression. Options are forwarded to the artificial parent observable with 
the exception of `parameters`, which are passed as additional parameters to the stream expression. 

### `extractReturnType(expression, options)`

Extract the return type of provided expression.

## Utility Functions

### `unwrap(observable)`

Recursively extract values from an observable into a plain Javascript object.

### `isObservable(any)`

Determine if an object is an observable.
