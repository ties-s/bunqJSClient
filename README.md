# BunqJSClient
A web based project that is aimed at allowing single page applications to do all interactions with Bunq without proxying through other services. 
All data is stored client-side and requests are created and signed using [forge](https://github.com/digitalbazaar/forge).

On its own the BunqJSClient doesn't do anything, you'll need to build an interface around it like we do with [BunqDesktop](https://github.com/Crecket/BunqDesktop).

This project is focussed on the browser and isn't officially supported yet for usage with NodeJS servers. 

## Installation
Install the library
```bash
yarn add @bunq-community/bunq-js-client
```
Next create a new instance with an optional storage interface as the first parameter. 
This defaults to [store.js](https://github.com/marcuswestin/store.js/) but any class 
with the following synchronous functions: `get(key)`, `set(key, data)`, `remove(key)` should work.
```js
import SomeStorageHelper from "some-storage-handler"; 

const BunqClientCustom = new BunqJSClient(SomeStorageHelper);

// OR use the default store.js
const BunqClient = new BunqJSClient();
```
Next run the setup basic functions to get started
```js
const setup = async () => {
    // run the bunq application with our API key
    await BunqClient.run(apiKey);
    
    // install a new keypair 
    await BunqClient.install();
    
    // register this device
    await BunqClient.registerDevice(deviceName);
    
    // register a new session
    await BunqClient.registerSession();
}
```
Now you can use the API in the bunq client to do requests and get the current users.
```js
// force that the user info is retrieved from the API instead of local cache version
const forceUpdate = true;

// all users connected to the api key
const users = await BunqClient.getUsers(forceUpdate);

// get only the userCompany account if one is set
const userCompany = await BunqClient.getUser("UserCompany", forceUpdate);
```

## Supported APIs
#### Installation/setup
- [POST /v1/installation](https://doc.bunq.com/api/1/call/installation/method/post)
- [POST /v1/device-server](https://doc.bunq.com/api/1/call/installation/method/post)
- [POST /v1/session-server](https://doc.bunq.com/api/1/call/session-server/method/post)

#### Attachements
- [GET /attachment-public/{imageUUID}/content](https://doc.bunq.com/api/1/call/attachment-public-content/method/list)

#### Payments
- [GET /v1/user/${userId}/monetary-account/${monetaryAccountId}/payment/${paymentId}](https://doc.bunq.com/api/1/call/payment/method/get)
- [LIST /v1/user/${userId}/monetary-account/${monetaryAccountId}/payment](https://doc.bunq.com/api/1/call/payment/method/list)

#### MonetaryAccounts
- [GET /v1/user/{userId}/monetary-account/{monetaryAccountId}](https://doc.bunq.com/api/1/call/monetary-account/method/get)
- [LIST /v1/user/{userId}/monetary-account](https://doc.bunq.com/api/1/call/monetary-account/method/list)
