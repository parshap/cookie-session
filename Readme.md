# cookie-session

[![NPM Version][npm-image]][npm-url]
[![NPM Downloads][downloads-image]][downloads-url]
[![Build Status][travis-image]][travis-url]
[![Test Coverage][coveralls-image]][coveralls-url]

  Simple cookie-based session middleware.

## Semantics

  This module provides "guest" sessions, meaning any visitor will have a session,
  authenticated or not. If a session is _new_ a `Set-Cookie` will be produced regardless
  of populating the session.

## Install

```bash
$ npm install cookie-session
```

## API

  View counter example:

```js
var express = require('express')
var session = require('cookie-session')

var app = express()

app.use(session({
  keys: ['key1', 'key2'],
  secureProxy: true // if you do SSL outside of node
}))

app.use(function (req, res, next) {
  var n = req.session.views || 0
  req.session.views = ++n
  res.end(n + ' views')
})

app.listen(3000)
```

### Options

  - `name` - The cookie name. Defaults to `express:sess`.
  - `keys` - Keys with which to sign the cookie. See `signed` in cookie options. Multiple keys allows for using rotating credentials.
  - `secret` - A string which will be used as single key if `keys` is not found.

  Other options are passed to `cookies.get()` and
  `cookies.set()` allowing you to control security, domain, path,
  and signing among other settings.

#### Cookie Options

  - `maxage` - a number representing the milliseconds from `Date.now()` for expiry.
  - `expires` - a `Date` object indicating the cookie's expiration date (expires at the end of session by default).
  - `path` - a string indicating the path of the cookie (`/` by default).
  - `domain` - a string indicating the domain of the cookie (no default).
  - `secure` - a boolean indicating whether the cookie is only to be sent over HTTPS (`false` by default for HTTP, `true` by default for HTTPS).
  - `secureProxy` - a boolean indicating whether the cookie is only to be sent over HTTPS (use this if you handle SSL outside your node process).
  - `httpOnly` - a boolean indicating whether the cookie is only to be sent over HTTP(S), and not made available to client JavaScript (`true` by default).
  - `signed` - a boolean indicating whether the cookie is to be signed (`true` by default). If this is true, another cookie of the same name with the `.sig` suffix appended will also be sent, with a 27-byte url-safe base64 SHA1 value representing the hash of _cookie-name_=_cookie-value_ against the first [Keygrip](https://github.com/jed/keygrip) key. This signature key is used to detect tampering the next time a cookie is received.
  - `overwrite` - a boolean indicating whether to overwrite previously set cookies of the same name (`true` by default). If this is true, all cookies set during the same request with the same name (regardless of path or domain) are filtered out of the Set-Cookie header when setting this cookie.

  Read more here: https://github.com/jed/cookies

### Session.isNew

  Is `true` if the session is new.

### Destroying a session

  To destroy a session simply set it to `null`:

```js
req.session = null
```

## License

[MIT](LICENSE)

[npm-image]: https://img.shields.io/npm/v/cookie-session.svg?style=flat
[npm-url]: https://npmjs.org/package/cookie-session
[travis-image]: https://img.shields.io/travis/expressjs/cookie-session.svg?style=flat
[travis-url]: https://travis-ci.org/expressjs/cookie-session
[coveralls-image]: https://img.shields.io/coveralls/expressjs/cookie-session.svg?style=flat
[coveralls-url]: https://coveralls.io/r/expressjs/cookie-session?branch=master
[downloads-image]: https://img.shields.io/npm/dm/cookie-session.svg?style=flat
[downloads-url]: https://npmjs.org/package/cookie-session
