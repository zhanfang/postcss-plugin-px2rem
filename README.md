# postcss-plugin-px2rem

[![NPM version](https://img.shields.io/npm/v/postcss-plugin-px2rem.svg?style=flat)](https://npmjs.org/package/postcss-plugin-px2rem)
[![Build Status](https://img.shields.io/travis/ant-tool/postcss-plugin-px2rem.svg?style=flat)](https://travis-ci.org/ant-tool/postcss-plugin-px2rem)
[![Coverage Status](https://img.shields.io/coveralls/ant-tool/postcss-plugin-px2rem.svg?style=flat)](https://coveralls.io/r/ant-tool/postcss-plugin-px2rem)
[![NPM downloads](http://img.shields.io/npm/dm/postcss-plugin-px2rem.svg?style=flat)](https://npmjs.org/package/postcss-plugin-px2rem)
[![Dependency Status](https://david-dm.org/ant-tool/postcss-plugin-px2rem.svg)](https://david-dm.org/ant-tool/postcss-plugin-px2rem)

postcss plugin px2rem.

<img align="right" width="135" height="95"
     title="Philosopher’s stone, logo of PostCSS"
     src="http://postcss.github.io/postcss/logo-leftp.svg">

## Features

A plugin for PostCSS that generates rem units from pixel units.

## Installation

```bash
$ npm i --save postcss-plugin-px2rem
```

## Usage

### input and output

```css
// input
h1 {
  margin: 0 0 20px;
  font-size: 32px;
  line-height: 1.2;
  letter-spacing: 1px;
}

// output
h1 {
  margin: 0 0 0.2rem;
  font-size: 0.32rem;
  line-height: 1.2;
  letter-spacing: 0.01rem;
}
```

### original

```javascript
import { writeFile, readFileSync } from 'fs';
import postcss from 'postcss';
import pxtorem from 'postcss-plugin-px2rem';

const css = readFileSync('/path/to/test.css', 'utf8');
const options = {
  replace: false
};
const processedCss = postcss(pxtorem(options)).process(css).css;

writeFile('/path/to/test.rem.css', processedCss, err => {
  if (err) throw err
  console.log('Rem file written.');
});
```

### with webpack

```javascript
import px2rem from 'postcss-plugin-px2rem';
 
module.exports = {
  module: {
    loaders: [
      {
        test: /\.css$/,
        loader: 'style-loader!css-loader!postcss-loader',
      }
    ]
  },
  postcss: function () {
    return [px2rem];
  }
}
```

## Configuration

Default:
```js
{
  rootValue: 100,
  unitPrecision: 5,
  propWhiteList: [],
  selectorBlackList: [],
  ignoreIdentifier: false,
  replace: true,
  mediaQuery: false,
  minPixelValue: 0
}
```

- `rootValue` (Number) The root element font size. Default is 100.
- `unitPrecision` (Number) The decimal numbers to allow the REM units to grow to.
- `propWhiteList` (Array) The properties that can change from px to rem.
    - Default is an empty array that means disable the white list and enable all properties.
    - Values need to be exact matches.
- `selectorBlackList` (Array) The selectors to ignore and leave as px.
    - If value is string, it checks to see if selector contains the string.
        - `['body']` will match `.body-class`
    - If value is regexp, it checks to see if the selector matches the regexp.
        - `[/^body$/]` will match `body` but not `.body`
- `ignoreIdentifier` (Boolean/String)  a way to have a single property ignored, when ignoreIdentifier enabled, then `replace` would be set to `true` automatically.
- `replace` (Boolean) replaces rules containing rems instead of adding fallbacks.
- `mediaQuery` (Boolean) Allow px to be converted in media queries.
- `minPixelValue` (Number) Set the minimum pixel value to replace.

### License
MIT