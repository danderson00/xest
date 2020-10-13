# @x/socket.auth

Authentication feature for @x/socket

## API

Adds the following functions to the @x/socket API surface:

### authenticate(payload, persistent)

`payload` must contain a property called `provider` that is one of either 
`password` or `facebook`. Other properties depend on which provider is being 
used.

#### Password Authentication

|Property|Description|
|-|-|
|username|Unique identifier for the user
|password|Password to authenticate with
|token|A valid JWT token issued by @x/socket

#### Facebook Authentication

|Property|Description|
|-|-|
|facebookToken|A valid JWT token issued by facebook, as returned from a successful login attempt
|token|A valid JWT token issued by @x/socket

### createUser({ username, password, data }, persistent)

### updateUserData({ data })

### logout()

### getAuthenticationStatus()

Returns an observable containing an object with the following properties:

|Property|Description|
|-|-|
|authenticated|True if the user is authenticated|
|user|The user object, if authenticated|

### requestUserVerification({ type })

Only `sms` verification type is currently supported, provided by Twilio.

### verifyUser({ type, verification = { code } })