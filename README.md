# base-plugins [![NPM version](https://img.shields.io/npm/v/base-plugins.svg)](https://www.npmjs.com/package/base-plugins) [![Build Status](https://img.shields.io/travis/jonschlinkert/base-plugins.svg)](https://travis-ci.org/jonschlinkert/base-plugins)

> Upgrade's plugin support in base-methods to allow plugins to be called any time after init.

## Install

Install with [npm](https://www.npmjs.com/):

```sh
$ npm i base-plugins --save
```

**What does this do?**

This plugin augments the generic plugin functionality that ships with [base](https://github.com/node-base/base).

* Without this plugin, any plugins that registered with the `use` method and are only called once upon init.
* With this plugin, other plugins that return a function will be pushed onto a `plugins` array, and can be called again later with the `run` method.

## Usage

```js
var plugins = require('base-plugins');
var Base = require('base-methods');
var base = new Base();

// register the `plugins` plugin
base.use(plugins());
```

## Examples

### .use example

Once the `use` method is called:

1. a `fns` array is added to the instance for storing plugin functions
2. a `run` method is added to the instance for running stored plugins
3. the `use` method is modified so that anytime a function is returned by a plugin it will automatically be pushed onto the `fns` array. Besides that, you shouldn't see any behavioral changes in how the `use` method works.

## .run example

The `run` method iterates over the `fns` array and calls each stored plugin function on the given object.

```js
var collection = {};
base.use(function(app) {
  app.x = 'y';
  return function(obj) {
    obj.a = 'b';
  };
});
base.run(collection);

console.log(base.x);
//=> 'y'
console.log(collection.a);
//=> 'b'
```

## API

### [.use](index.js#L46)

Define a plugin function to be called immediately upon init. The only parameter exposed to the plugin is the application instance.

Also, if a plugin returns a function, the function will be pushed
onto the `fns` array, allowing the plugin to be called at a
later point, elsewhere in the application.

**Params**

* `fn` **{Function}**: plugin function to call
* `returns` **{Object}**: Returns the item instance for chaining.

**Example**

```js
// define a plugin
function foo(app) {
  // do stuff
}

// register plugins
var app = new Base()
  .use(foo)
  .use(bar)
  .use(baz)
```

### [.run](index.js#L61)

Run all plugins

**Params**

* `value` **{Object}**: Object to be modified by plugins.
* `returns` **{Object}**: Returns the item instance for chaining.

**Example**

```js
var config = {};
app.run(config);
```

## Related projects

* [base-cli](https://www.npmjs.com/package/base-cli): Plugin for base-methods that maps built-in methods to CLI args (also supports methods from a… [more](https://www.npmjs.com/package/base-cli) | [homepage](https://github.com/jonschlinkert/base-cli)
* [base-config](https://www.npmjs.com/package/base-config): base-methods plugin that adds a `config` method for mapping declarative configuration values to other 'base'… [more](https://www.npmjs.com/package/base-config) | [homepage](https://github.com/jonschlinkert/base-config)
* [base-data](https://www.npmjs.com/package/base-data): adds a `data` method to base-methods. | [homepage](https://github.com/jonschlinkert/base-data)
* [base-fs](https://www.npmjs.com/package/base-fs): base-methods plugin that adds vinyl-fs methods to your 'base' application for working with the file… [more](https://www.npmjs.com/package/base-fs) | [homepage](https://github.com/jonschlinkert/base-fs)
* [base-methods](https://www.npmjs.com/package/base-methods): base-methods is the foundation for creating modular, unit testable and highly pluggable node.js applications, starting… [more](https://www.npmjs.com/package/base-methods) | [homepage](https://github.com/jonschlinkert/base-methods)
* [base-options](https://www.npmjs.com/package/base-options): Adds a few options methods to base-methods, like `option`, `enable` and `disable`. See the readme… [more](https://www.npmjs.com/package/base-options) | [homepage](https://github.com/jonschlinkert/base-options)

## Generate docs

Generate readme and API documentation with [verb](https://github.com/verbose/verb):

```sh
$ npm i -d && npm run docs
```

Or, if [verb](https://github.com/verbose/verb) is installed globally:

```sh
$ verb
```

## Running tests

Install dev dependencies:

```sh
$ npm i -d && npm test
```

## Contributing

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](https://github.com/jonschlinkert/base-plugins/issues/new).

## Author

**Jon Schlinkert**

* [github/jonschlinkert](https://github.com/jonschlinkert)
* [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

## License

Copyright © 2016 [Jon Schlinkert](https://github.com/jonschlinkert)
Released under the [MIT license](https://github.com/jonschlinkert/base-plugins/blob/master/LICENSE).

***

_This file was generated by [verb](https://github.com/verbose/verb), v0.9.0, on February 19, 2016._