# base-plugins [![NPM version](https://badge.fury.io/js/base-plugins.svg)](http://badge.fury.io/js/base-plugins)

> Upgrade's plugin support in base-methods to allow plugins to be called any time after init.

**What does this do?**

This lib itself is a actually a plugin for base-methods. Natively, base methods supports plugins that registered with the `use` method and are only called once upon init.

By adding this plugin, plugins that return a function will be pushed onto a `plugins` array, and may be called later with the `run` method.

## Install

Install with [npm](https://www.npmjs.com/)

```sh
$ npm i base-plugins --save
```

## Usage

```js
var plugins = require('base-plugins');
var Base = require('base-methods');
var base = new Base();
base.use(plugins);
```

Once the `use` method is called:

1. a `fns` array is added to the instance for storing plugin functions
2. a `run` method is added to the instance for running stored plugins
3. the `use` method is modified so that anytime a function is returned by a plugin it will automatically be pushed onto the `fns` array. Besides that, you shouldn't see any behavioral changes in how the `use` method works.

## `run` example

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

### [.use](index.js#L42)

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

### [.run](index.js#L64)

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

* [base-data](https://www.npmjs.com/package/base-data): adds a `data` method to base-methods. | [homepage](https://github.com/jonschlinkert/base-data)
* [base-methods](https://www.npmjs.com/package/base-methods): Starter for creating a node.js application with a handful of common methods, like `set`, `get`,… [more](https://www.npmjs.com/package/base-methods) | [homepage](https://github.com/jonschlinkert/base-methods)
* [base-options](https://www.npmjs.com/package/base-options): Adds a few options methods to base-methods, like `option`, `enable` and `disable`. See the readme… [more](https://www.npmjs.com/package/base-options) | [homepage](https://github.com/jonschlinkert/base-options)

## Running tests

Install dev dependencies:

```sh
$ npm i -d && npm test
```

## Contributing

Pull requests and stars are always welcome. For bugs and feature requests, [please create an issue](https://github.com/jonschlinkert/base-plugins/issues/new).

## Author

**Jon Schlinkert**

+ [github/jonschlinkert](https://github.com/jonschlinkert)
+ [twitter/jonschlinkert](http://twitter.com/jonschlinkert)

## License

Copyright © 2015 Jon Schlinkert
Released under the MIT license.

***

_This file was generated by [verb-cli](https://github.com/assemble/verb-cli) on October 24, 2015._