# @xest/core

Core expression library for the `xest` platform

## Installation

```shell
yarn add @xest/core
```

## Usage

The `xest` core expression language is heavily inspired by [`ReactiveX`](http://reactivex.io/) but with a focus on
extensibility and reuse. Complete, reactive models can be composed from messages published through the expression.

```javascript
const reviews = o => o.groupBy('reviewId',
  o => o.compose(
    o => o.select('reviewId'),
    o => o.topic('review').accumulate('details'),
    o => o.topic('like').groupBy('userId',
      o => o.select('liked'),
      o => o.where(x => x.liked === false)
    ).count(),
    (reviewId, review, likes) => ({ reviewId, ...review, likes })
  ),
  o => o.topic('report').count().where(x => x > 1)
)
```

To create a simple message source:

```javascript
const { subject } = require('@xest/core')

const source = subject()
const result = reviews(source)

source.publish({ topic: 'review', reviewId: 1, details: { text: 'Awesome!' } })
source.publish({ topic: 'review', reviewId: 2, details: { text: 'It is amazing!' } })
source.publish({ topic: 'review', reviewId: 3, details: { text: 'ABUSE! ^%$#!!!!' } })
source.publish({ topic: 'like', reviewId: 1, userId: 1, liked: true })
source.publish({ topic: 'like', reviewId: 2, userId: 1, liked: true })
source.publish({ topic: 'like', reviewId: 1, userId: 2, liked: true })
source.publish({ topic: 'report', reviewId: 3, userId: 1 })
source.publish({ topic: 'report', reviewId: 3, userId: 2 })

console.log(result)
/*
[
  { reviewId: 1, text: 'Awesome!', likes: 2 },
  { reviewId: 2, text: 'It is amazing!', likes: 1 }
]
*/
```

## Documentation

You can find documentation for the `xest` API [here](docs/api.md).

API references are available for [stream operators](docs/stream.md) and [aggregate operators](docs/aggregate.md).

For documentation on the entire `xest` platform, please visit the [documentation site](https://danderson00.github.io/xest/).