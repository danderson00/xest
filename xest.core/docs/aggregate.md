# Aggregate Operators

## `asHash()`

Convert the aggregate to an object hashed by the aggregate item key. Only works with aggregates created by the
[`groupBy`](stream.md?id=groupbyexpression-groupexpression-removeexpression) operator.

### Result Type

`stream`

## `average(expression)`

Calculate the average of values in each member in the aggregate.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Expression to determine value to average|

### Result Type

`stream`

## `count()`

Count the number of members in the aggregate.

### Result Type

`stream`

## `map(expression)`

Map each member of the aggregate to a new value using the provided expression.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Expression to transform each element of the aggregate|

### Result Type

`aggregate`

## `mapAll(expression)`

Map the entire aggregate into a new value using the provided expression. Treats the aggregate as a stream value.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Expression to transform the aggregate|

### Result Type

`stream`

## `orderBy(expression)`

Order members in the aggregate by the value returned by the provided expression.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Expression to determine sort order|

### Result Type

`aggregate`

## `orderByDescending(expression)`

Order members in the aggregate in a descending order by the value returned by the provided expression.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Expression to determine sort order|

### Result Type

`aggregate`

## `sum(expression)`

Calculate the sum of values from each member of the aggregate.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Predicate to determine value to sum|

### Result Type

`stream`

## `where(expression)`

Filter members of the aggregate by the specified predicate.

### Parameters

|Name|Type|Description|
|-|-|-|
|expression|propertyRef\|predicate\|undefined|Predicate to filter by|

### Result Type

`aggregate`
