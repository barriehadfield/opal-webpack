# opalrb-loader
> Opal is a compiler for writing JavaScript in Ruby.

This package allows transpiling Ruby files using [Opal](http://opalrb.org) and [webpack](https://github.com/webpack/webpack).

Check out [this blog post](https://medium.com/@zetachang/from-sprockets-to-webpack-5f3d1afbd1b0) if you are interested in the project background.

[![npm version](https://img.shields.io/npm/v/opal-webpack.svg?style=flat-square)](https://www.npmjs.com/package/opal-webpack)
[![npm downloads](https://img.shields.io/npm/dt/opal-webpack.svg?style=flat-square)](https://www.npmjs.com/package/opal-webpack)
[![Quality](http://img.shields.io/codeclimate/github/cj/opal-webpack.svg?style=flat-square)](https://codeclimate.com/github/cj/opal-webpack)
[![Build Status](http://img.shields.io/travis/cj/opal-webpack/master.svg?style=flat)](http://travis-ci.org/cj/opal-webpack)

## Installation

```bash
npm install opal-webpack --save-dev
```
## Requirements

* Node/Webpack obviously
* Opal 0.9.2 or 0.10 (see below for information on this)

## Usage

[Documentation: Using loaders](http://webpack.github.io/docs/using-loaders.html)

```javascript
// webpack.config.js
module: {
  loaders: [
    {
      test: /\.rb?$/,
      loader: 'opalrb-loader'
    }
  ]
}
```

### Options

See `Opal::Compiler` [options](https://github.com/opal/opal/blob/master/lib/opal/compiler.rb) for possible options.

```javascript
// webpack.config.js
module: {
  loaders: [
    {
      test: /\.rb?$/,
      loader: 'opalrb-loader',
      query: {
        requirable: false,
        freezing: false,
      }
    }
  ]
}
```

#### Requires

With your Ruby/Opal `require` statements, you can either use the Sprockets/Ruby/Opal convention or you can use the node convention of `require './some_file'`, which will be relative to path of the file doing the require. If you do this though, you must consistently require the file that way. A require to 'some_file' will include the module a second time.

#### Stubs

To tell the Opal compiler to stub out certain dependencies, do this

```js
{
  module: {
    loaders: [
      {
        test: /\.rb?$/,
        loader: 'opalrb-loader'
      }
    ]
  },
  opal: {
    stubs: ['dependency']
  }
}
```

#### Caching

Just like the Babel loader, you can cache compilation results on the filesystem to improve load times
between invocations of webpack.

```js
{
  module: {
    loaders: [
      {
        test: /\.rb?$/,
        loader: 'opalrb-loader'
      }
    ]
  },
  opal: {
    cacheDirectory: './tmp/cache'
  }
}
```

#### Opal version

When you `require 'opal'` in any asset, this loader will use the version of Opal bundled with this tool.
opal/mini, opal/full are not supported. Currently Opal 0.9.2 is bundled with the tool. Unlike previous versions,
it's all or none. You cannot (and should not given rapid Opal development) mix a compiler version with a different
runtime.

#### OPAL_LOAD_PATH

By passing `OPAL_LOAD_PATH` environment variable to webpack, the loader will correctly resolve file other than relative path.

See the example [Rakefile](https://github.com/cj/opal-webpack/blob/master/examples/complex/Rakefile) for how to integrate using other Opal gems.

### Compared to `Opal::Builder`
* Relative `require` is correctly resolved.
* erb is not supported (which should be implemented as separate loader).

### Known issues
* First time compiling is relatively slow compared to Ruby one, use `--watch` option for webpack to speed up dev iteration.
* **stdlib** and some gems may not be correctly compiled, please file an issue if you encounter one.

### Examples

It's under [Examples](https://github.com/cj/opal-webpack/tree/master/examples) folder.

* simple: Basic setup without further dependency.
* complex: Compile opal/corelib and other gems.

## Development

* `npm install`
* `npm run build_compiler` to build compiler
* `npm start` to compile & watch `index.js`

## Contact

[CJ Lazell](http://github.com/cj)
[@ceej](https://twitter.com/cj)

## License

Available under the MIT license. See the LICENSE file for more info.
