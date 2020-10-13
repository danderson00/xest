# @xest/bundle

`xest` for Node.js and the browser in a simple package.

## Getting Started

### Install Dependencies

```shell
yarn add @xest/bundle @xest/react sqlite3
```

### Start Host

```shell
npx xest
```

The following command line options are available:

Option||Description
---|---|---
--port|-p|TCP port number to listen on. Defaults to 3001
--scope|-s|Property name that can participate in scopes. Multiple can be specified
--sqliteFile|-f|Specifies the path to a SQLite database to use
--vocabulary|-v|Specifies the path to a Javascript module containing vocabulary definitions

For alternate ways of starting the host, including using other database providers, see the section below.

### Configure Consumer

For applications bootstrapped with `create-react-app`, replace the contents of `src/index.js` with the following:

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

Vocabulary can also be passed in as a prop to the `Provider` component.

An options object can also be passed to the consumer factory. Options are as follows:

Name|Type|Description
---|---|---
url|string|The websocket URL of the host to connect to
reconnectDelay|number|Time in milliseconds between reconnect attempts. Defaults to 1000ms
log|object|Logging configuration options. `level` property can be `error`, `warn`, `debug`, `trace` or `none`

Any components in your application can now use the 
[higher order components](https://danderson00.github.io/xest/#/xest.react/docs/hocs) and
[hooks](https://danderson00.github.io/xest/#/xest.react/docs/hooks) from the
[`xest.react`](https://danderson00.github.io/xest/#/xest.react/) library.

## Host Startup / Configuration Options

The example here uses `npx` to invoke the xest command line interface. A number of other options are available for 
starting and configuring the host.

### Installing Globally

The `@xest/bundle` package can be installed globally by executing:

```shell
npm i -g @xest/bundle
# or
yarn global add @xest/bundle
```

This makes the CLI available globally throughout your system and can be invoked without `npx`.

### Using a Configuration File

Configuration options can be specified in a Javascript or JSON file. By default, this file is called `xest.config.json`.
Options correspond to command line options described above.

### Starting the Host Programmatically

The host process can be started from any Node.js module.

```javascript
const host = require('@xest/bundle')
const vocabulary = require('./vocabulary')

host({
  port: 1234,
  scopes: ['orderId', 'productId'],
  storage: { client: 'sqlite3', connection: { filename: 'data.sqlite' } },
  vocabulary
})
```

Any database supported by [`knex`](https://knexjs.org/) can be used and should be configured accordingly here. An 
in-memory instance of the `sqlite` provider is used by default, but the `sqlite3` package must be installed separately. 

### Starting the Host and UI From `package.json`

The `concurrently` package can be used to start both the host and your application from a single command in
`package.json`. 

For example, in a project bootstrapped with `create-react-app`, after installing the `concurrently` npm 
package, replace the `start` script in `package.json` with the following:

```json
"start": "concurrently xest \"react-scripts start\""
```
