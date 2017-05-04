# promiscuous-umd
<a href="http://promises-aplus.github.com/promises-spec">
  <img src="https://promisesaplus.com/assets/logo-small.png"
       alt="Promises/A+ logo" title="Promises/A+ 1.0 compliant" align="right" />
</a>

promiscuous-umd is a tiny-ish implementation of the [Promises/A+ spec](http://promises-aplus.github.com/promises-spec/).

It is promise library in JavaScript, **small** (< 1.5kb [minified](https://raw.github.com/mix3d/promiscuous-umd/dist/promiscuous-min.js) / < 0.75kb gzipped) and **fast**.

Forked from [promiscuous](http://github.com/RubenVerborgh/promiscuous/), it trades an additional 0.5kb over the original library to provide a singular package compatible with all platforms and module loaders, including a shim to check if a native Promise is already provided, since it's 2017 and most browsers include them now.

# TODO:publish to NPM

## Installation and usage
### Node
First, install promiscuous with npm.
```bash
$ npm install promiscuous-umd
```

Then, include promiscuous in your code file.
```javascript
var Promise = require('promiscuous-umd');
```

### Browsers
Include [promiscuous](https://raw.github.com/RubenVerborgh/promiscuous/dist/promiscuous-browser.js) in your HTML file.
```html
<script src="promiscuous-umd.js"></script>
```



This a minified version can be built with:
```bash
$ build/build.js
```

## API
### Create a resolved promise
```javascript
var promise = Promise.resolve("one");
promise.then(function (value) { console.log(value); });
/* one */
```

### Create a rejected promise
```javascript
var brokenPromise = Promise.reject(new Error("Could not keep promise."));
brokenPromise.then(null, function (error) { console.error(error.message); });
/* "Could not keep promise." */
```

You can also use the `catch` method if there is no success callback:

```javascript
brokenPromise.catch(function (error) { console.error(error.message); });
/* "Could not keep promise." */
```

### Write a function that returns a promise
```javascript
function promiseLater(something) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      if (something)
        resolve(something);
      else
        reject(new Error("nothing"));
    }, 1000);
  });
}
promiseLater("something").then(
  function (value) { console.log(value); },
  function (error) { console.error(error.message); });
/* something */

promiseLater(null).then(
  function (value) { console.log(value); },
  function (error) { console.error(error.message); });
/* nothing */
```

### Convert an array of promises into a promise for an array
```javascript
var promises = [promiseLater(1), promiseLater(2), promiseLater(3)];
Promise.all(promises).then(function (values) { console.log(values); });
/* [1, 2, 3] */
```
