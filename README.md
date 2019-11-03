<h1 align="center">
    Laravel Mix Ignore
    <br>
    <a href="https://www.npmjs.com/package/laravel-mix-ignore"><img src="https://img.shields.io/npm/v/laravel-mix-ignore.svg?style=for-the-badge" alt="npm" /></a> <a href="https://www.npmjs.com/package/laravel-mix-ignore"><img src="https://img.shields.io/npm/dt/laravel-mix-ignore.svg?style=for-the-badge" alt="npm" /></a>
</h1>

Ignore specific module(s) when bundling assets, using [Laravel Mix](https://laravel-mix.com/).

Read more about how ignoring module(s) work with the [IgnorePlugin](https://webpack.js.org/plugins/ignore-plugin/).

## Installation

```bash
npm install laravel-mix-ignore --save-dev
```

## Parameters

| Parameter | Type |
|---|---|
| resource | `String` &#124; `RegExp` &#124; `Function` |
| context (optional) | `String` &#124; `RegExp` &#124; `Function` |

When a `String` is given for either parameter, it's automatically converted to a `RegExp` instance.

So, `target/with/slashes` is converted to: `/target\/with\/slashes/`.

## Usage

The resource parameter is tested against the string passed to require or import within the source code where the import is taking place.

```js
const mix = require('laravel-mix');
require('laravel-mix-ignore');

// ignore bundling the following module
mix.ignore('import-or-require-statement-to-ignore', 'only-when-in-this-context');
```

Following with the official documentation on the IgnorePlugin, you can also pass in filter functions for either the resource or context parameters.

As stated in the documentation, the filter function(s) must return a `Boolean` value for the plugin to evaluate.

```js
mix.ignore(
    resource => resource == 'import-or-require-statement-to-ignore',
    context => context == 'when-in-this-folder' // optional (without will be unconstrained)
);
```

Adapting the official documentation's examples when dealing with [moment](https://momentjs.com/) locales, in order to properly ignore bundling them when importing or requiring moment, do the following:

With the following import statement with how moment imports its locale(s)...

```js
require('./locale/' + name);
```

...the first resource parameter should match against `./locale` and the second context parameter should specify the directory or directories from where the import took place. The following will cause the locale files to be ignored in the proper context:

```js
mix.ignore(
    /^\.\/locale$/,
    /moment$/
);
```