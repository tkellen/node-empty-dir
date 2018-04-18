# empty-dir [![Build Status](https://secure.travis-ci.org/js-cli/js-empty-dir.svg?branch=master)](http://travis-ci.org/js-cli/js-empty-dir)
> Check if a directory is empty.

[![NPM](https://nodei.co/npm/empty-dir.png)](https://nodei.co/npm/empty-dir/)

## Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm install --save empty-dir
```

If your version of Node does not support native Promises, you'll need to also install an [`any-promise`](https://github.com/kevinbeaty/any-promise) compatible Promise polyfill.

## Usage

```js
var emptyDir = require('empty-dir');

// Using an error-back
emptyDir('./', function (err, result) {
  if (err) {
    console.error(err);
  } else {
    console.log('Directory is empty:', result);
  }
});

// Using a Promise
emptyDir('./').then(function (result) {
  console.log('Directory is empty:', result);
})

var result = emptyDir.sync('./test/empty');
console.log('Directory is empty:', result);
```

## Filter function

Both async and sync take a filter function as the second argument, to ignore files like `.DS_Store` on mac or `Thumbs.db` on windows from causing false-negatives.


```js
var emptyDir = require('empty-dir');

function filter(filepath) {
  return !/(Thumbs\.db|\.DS_Store)$/i.test(filepath);
}

emptyDir('./', filter, function (err, isEmpty) {
  if (err) {
    console.error(err);
  } else {
    console.log('Directory is empty:', isEmpty);
  }
});

var isEmpty = emptyDir.sync('./test/empty', filter);
console.log('Directory is empty:', isEmpty);
```

## Release History

* 2018-03-09 - v1.0.0 - refactored "isEmpty" logic so that it returns early, as soon as a non-filtered file is encountered, instead of filtering the entire list and comparing against length. Also allows an array to be passed (this avoids having to call `fs.readdir()` multiple times).
* 2016-02-07 - v0.2.0 - add filter support
* 2014-05-08 - v0.1.0 - initial release
