## Nano Request

A tiny promise wrapper around xmlHttpRequest ...

### Signature

```javascript
/**
 * Check if value is undefined
 *
 * @param {Object} value
 * @returns {boolean} true if undefined, false if not
 */

function isUndefined(val) {
  return typeof val === 'undefined'
}

/**
 * @param  {} options
 * @param  {} timeout
 * @todo: add timeout specific to cancel the request
 *
 * @returns promise over xmlhttprequest
 */

function nanoRequest(options, timeout) {
  if (!options) {
    /*
{
  method: String,
  url: String,
  headers: String | Object
  params: Object
}
*/
    options = {
      method: 'GET',
      url: 'https://swapi.dev/api/people/1/',
      params: {},
      headers: {},
    }
  }

  return new Promise(function (resolve, reject) {
    const xhr = new XMLHttpRequest()
    const params = options.params

    xhr.open(options.method, options.url, true)

    xhr.onload = function () {
      if (this.status >= 200 && this.status < 400) {
        resolve(xhr.response)
      } else {
        reject({
          status: this.status,
          statusText: xhr.statusText,
        })
      }
    }

    xhr.onerror = function () {
      setTimeout(function () {
        reject(new TypeError('Request failed'))
      }, 0)
    }

    xhr.ontimeout = function () {
      setTimeout(function () {
        reject(new TypeError('Request timed-out'))
      }, 0)
    }

    xhr.onabort = function () {
      setTimeout(function () {
        reject(new TypeError('Request aborted'))
      }, 0)
    }

    if (options.headers) {
      Object.keys(opts.headers).forEach(function (name) {
        xhr.setRequestHeader(name, opts.headers[name])
      })
    }

    if (params && typeof params === 'object') {
      params = Object.keys(params)
        .map(function (key) {
          return encodeURIComponent(key) + '=' + encodeURIComponent(params[key])
        })
        .join('&')
    }

    xhr.send(params)
  })
}
```

### Usage

```javascript
// a. promise
nanoRequest({
  method: 'GET',
  url: 'https://swapi.dev/api/people/1/',
})
  .then(function (data) {
    console.log(data)
  })
  .catch(function (error) {
    console.error('Uggh, there was an error!', error.status)
  })

// b. async / await
async function fetchData() {
  try {
    const response = await nanoRequest({method: 'GET', url: 'https://swapi.dev/api/people/1/'})
  } catch(error) {
      console.error('Uggh, there was an error!', err.status)
  }
```
