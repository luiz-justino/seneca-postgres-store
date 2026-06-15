![Seneca](http://senecajs.org/files/assets/seneca-logo.png)

> A [Seneca.js](http://senecajs.org) data storage plugin

# seneca-postgres-store

[![npm version][npm-badge]][npm-url]
[![Build Status][travis-badge]][travis-url]
[![Dependency Status][david-badge]][david-url]
[![Coveralls][BadgeCoveralls]][Coveralls]
[![Gitter][gitter-badge]][gitter-url]

| ![Voxgig](https://www.voxgig.com/res/img/vgt01r.png) | This open source module is sponsored and supported by [Voxgig](https://www.voxgig.com). |
|---|---|

seneca-postgres-store is a [PostgreSQL][postgresqlorg] database plugin for the [Seneca][seneca] MVP toolkit.
The plugin uses the [node-postgres][nodepg] driver. For query generation it uses internally the
[seneca-standard-query][standard-query] plugin. The standard functionality can be extended by using
the [seneca-store-query][store-query] plugin.

### Seneca compatibility
Supports Seneca versions **1.x** - **3.x**

### Supported functionality
All Seneca data store supported functionality is implemented in [seneca-store-test](https://github.com/senecajs/seneca-store-test) as a test suite.

## Install

```sh
npm install seneca-postgres-store
```

## Quick Example

```js
var Seneca = require('seneca')
var store = require('seneca-postgres-store')

var DBConfig = {
  name: 'senecatest',
  host: 'localhost',
  username: 'senecatest',
  password: 'senecatest',
  port: 5432
}

var si = Seneca(DBConfig)
si.use(require('seneca-postgres-store'), DBConfig)
si.ready(function() {
  var product = si.make('product')
  // ...
})
```

## More Examples

### Seneca Entity API

```js
var entity = seneca.make$('typename')
entity.someproperty = "something"
entity.anotherproperty = 100

entity.save$(function (err, entity) { ... })
entity.load$({id: ...}, function (err, entity) { ... })
entity.list$({property: ...}, function (err, entity) { ... })
entity.remove$({id: ...}, function (err, entity) { ... })
```

### Column name transformation

In seneca-postgres-store 2.0 the internal CamelCase to snake_case conversion was removed.
To update from 1.x to 2.x you must provide conversion functions:

```js
var DefaultConfig = {
  fromColumnName: function (attr) {
    return attr.toUpperCase()
  },
  toColumnName: function (attr) {
    return attr.toLowerCase()
  }
}
seneca.use(require('seneca-postgres-store'), DefaultConfig)
```

### Custom ID generator

```js
seneca.add({role: 'sql', hook: 'generate_id', target: '<store name>'}, function (args, done) {
  return done(null, {id: idPrefix + Uuid()})
})
```

### Native Driver

```js
entity.native$(function (err, client, releaseConnection) {
  // use client
  releaseConnection()
})
```

## Motivation

You don't use this module directly. It provides an underlying data storage engine for the Seneca entity API.
It supports the standard Seneca query format via [seneca-standard-query][standard-query] and can be extended
with [seneca-store-query][store-query].

### Limits

By default queries are limited to 20 values. This can be bypassed by passing the `nolimit` option.

### Fields

To filter the fields returned from `list`, pass a `fields$` array:

```js
query.fields$ = ['id', 'name']
```

Note: The implicit id generated on `save$` has uuid value.

## Support

If you have any questions, [open an issue](https://github.com/senecajs/seneca-postgres-store/issues)
or contact the [Senecajs team](https://senecajs.org/).

### Query Support

The standard Seneca query format is supported. See [seneca-standard-query][standard-query] for details.

### Extended Query Support

Use [seneca-store-query][store-query] to extend query capabilities.

## API

See the [seneca-store-test](https://github.com/senecajs/seneca-store-test) suite for the full
store functionality specifications.

## Contributing

The [Senecajs org][senecajs-org] encourages open participation. If you feel you
can help in any way, be it with documentation, examples, extra testing,
or new features please get in touch.

### Running tests with Docker

Build the PostgreSQL Docker image:

```sh
npm run build
```

Start the PostgreSQL container:

```sh
npm run start
```

Stop the PostgreSQL container:

```sh
npm run stop
```

Run the tests:

```sh
npm run test
```

#### Testing for Mac users

Run `docker-machine env default` and copy the docker host address (e.g. `192.168.99.100`).
Insert it into `test/default_config.json` as the `host` value.

## Background

This plugin was created to provide PostgreSQL storage for the Seneca microservices framework.
It is part of the [Voxgig](https://www.voxgig.com) open source initiative.

## License

Copyright (c) 2012 - 2016, Marian Radulescu and other contributors.
Licensed under [MIT][].

[MIT]: ./LICENSE
[npm-badge]: https://img.shields.io/npm/v/seneca-postgres-store.svg
[npm-url]: https://npmjs.com/package/seneca-postgres-store
[travis-badge]: https://api.travis-ci.org/senecajs/seneca-postgres-store.svg
[travis-url]: https://travis-ci.org/senecajs/seneca-postgres-store
[david-badge]: https://david-dm.org/senecajs/seneca-postgres-store.svg
[david-url]: https://david-dm.org/senecajs/seneca-postgres-store
[gitter-badge]: https://badges.gitter.im/Join%20Chat.svg
[gitter-url]: https://gitter.im/senecajs/seneca
[standard-query]: https://github.com/senecajs/seneca-standard-query
[store-query]: https://github.com/senecajs/seneca-store-query
[postgresqlorg]: http://www.postgresql.org/
[seneca]: http://senecajs.org/
[nodepg]: https://github.com/brianc/node-postgres
[senecajs-org]: https://github.com/senecajs/
[Coveralls]: https://coveralls.io/github/senecajs/seneca-postgres-store?branch=master
[BadgeCoveralls]: https://coveralls.io/repos/github/senecajs/seneca-postgres-store/badge.svg?branch=master
