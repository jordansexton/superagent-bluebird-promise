[![Build Status](https://img.shields.io/travis/KyleAMathews/superagent-bluebird-promise/master.svg?style=flat-square)](http://travis-ci.org/KyleAMathews/superagent-bluebird-promise)

superagent-bluebird-promise
===========================

Add promise support to
[Superagent](http://visionmedia.github.io/superagent/) using
[Bluebird](https://github.com/petkaantonov/bluebird).

## Install
`npm install superagent-bluebird-promise`

## Usage
Simply require this package instead of `superagent`. Then you can call `.promise()` instead of `.end()` to get a promise for your requests.

```javascript

var request = require('superagent-bluebird-promise');

request.get('/an-endpoint').promise()
  .then(function(res) {
    console.log(res);
  })
  .catch(function(error) {
    console.log(error);
  })
  ```

An error is thrown for all HTTP errors and responses that have a response code of 400 or above.

The `error` parameter always has a key `error` and for 4xx and 5xx responses, will also have a `status` and `res` key.

## Options

```js
var promise = request.get('/an-endpoint').promise([options]);
```

### options.cancellable (boolean)

```js
var promise = request.get('/an-endpoint').promise({ cancellable: true });

```

#### Cancelling promises

```js
promise.cancel();
```

#### Cancelling promises with a custom reason

**IMPORTANT:** The superagent request won't be [aborted](http://visionmedia.github.io/superagent/#aborting-requests) unless the custom reason class extends bluebird's [CancellationError](https://github.com/petkaantonov/bluebird/blob/master/API.md#cancellationerror).

```js
var Promise = require('bluebird');
class CustomCancellationError extends Promise.CancellationError {
  ...
};

promise.cancel(new CustomCancellationError());
```
