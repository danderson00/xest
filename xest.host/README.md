# @xest/host

Host process for xest

## Standalone Installation

`xest` can be configured to run in-process in any Javascript environment with a supported data store 
([IndexedDB](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) and any of the SQL databases supported by 
[`knex`](https://knexjs.org/)). Use the following steps to install for a browser in a React application using IndexedDB.

### Install Dependencies

```
yarn add @xest/host @xest/react
```

### Configure React Bindings

Modify `src/index.js` to wrap your `App` component in a `Provider`:

```
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from '@xest/react'
import App from './App'
import host from '@xest/host'

ReactDOM.render(
  <Provider host={host()}><App /></Provider>,
  document.getElementById('root')
)
```
