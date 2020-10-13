# @xest/react

React higher order components (HoCs) and hooks for accessing xest host functions

## Installation

```shell
yarn add @xest/react
```

## Usage

All HoCs and hooks, with the exception of the `useObservable` hook, require a `Provider` component to be created higher
in the component tree.

A minimal configuration using locally stored data might look like:

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import host from '@xest/host'
import { Provider } from '@xest/react'
import App from './App'

ReactDOM.render(
  <Provider host={host()}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

## Documentation

Documentation is available for the [HoCs](https://gitlab.com/danderson00/xest.react/blob/master/docs/hocs.md) and 
[hooks](https://gitlab.com/danderson00/xest.react/blob/master/docs/hooks.md).