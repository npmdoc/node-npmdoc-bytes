# api documentation for  [bytes (v2.5.0)](https://github.com/visionmedia/bytes.js#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-bytes.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-bytes) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-bytes.svg)](https://travis-ci.org/npmdoc/node-npmdoc-bytes)
#### Utility to parse a string bytes to bytes and vice-versa

[![NPM](https://nodei.co/npm/bytes.png?downloads=true)](https://www.npmjs.com/package/bytes)

[![apidoc](https://npmdoc.github.io/node-npmdoc-bytes/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-bytes_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-bytes/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-bytes/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-bytes/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "TJ Holowaychuk",
        "email": "tj@vision-media.ca",
        "url": "http://tjholowaychuk.com"
    },
    "bugs": {
        "url": "https://github.com/visionmedia/bytes.js/issues"
    },
    "component": {
        "scripts": {
            "bytes/index.js": "index.js"
        }
    },
    "contributors": [
        {
            "name": "Jed Watson",
            "email": "jed.watson@me.com"
        },
        {
            "name": "Théo FIDRY",
            "email": "theo.fidry@gmail.com"
        }
    ],
    "dependencies": {},
    "description": "Utility to parse a string bytes to bytes and vice-versa",
    "devDependencies": {
        "mocha": "1.21.5",
        "nyc": "10.1.2"
    },
    "directories": {},
    "dist": {
        "shasum": "4c9423ea2d252c270c41b2bdefeff9bb6b62c06a",
        "tarball": "https://registry.npmjs.org/bytes/-/bytes-2.5.0.tgz"
    },
    "engines": {
        "node": ">= 0.6"
    },
    "files": [
        "History.md",
        "LICENSE",
        "Readme.md",
        "index.js"
    ],
    "gitHead": "a4b9af2bf289175f12b3538eb172f2489844b1ec",
    "homepage": "https://github.com/visionmedia/bytes.js#readme",
    "keywords": [
        "byte",
        "bytes",
        "utility",
        "parse",
        "parser",
        "convert",
        "converter"
    ],
    "license": "MIT",
    "maintainers": [
        {
            "name": "dougwilson",
            "email": "doug@somethingdoug.com"
        },
        {
            "name": "tjholowaychuk",
            "email": "tj@vision-media.ca"
        }
    ],
    "name": "bytes",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/visionmedia/bytes.js.git"
    },
    "scripts": {
        "test": "mocha --check-leaks --reporter spec",
        "test-ci": "nyc --reporter=text npm test",
        "test-cov": "nyc --reporter=html --reporter=text npm test"
    },
    "version": "2.5.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module bytes](#apidoc.module.bytes)
1.  [function <span class="apidocSignatureSpan">bytes.</span>format (value, options)](#apidoc.element.bytes.format)
1.  [function <span class="apidocSignatureSpan">bytes.</span>parse (val)](#apidoc.element.bytes.parse)



# <a name="apidoc.module.bytes"></a>[module bytes](#apidoc.module.bytes)

#### <a name="apidoc.element.bytes.format"></a>[function <span class="apidocSignatureSpan">bytes.</span>format (value, options)](#apidoc.element.bytes.format)
- description and source-code
```javascript
function format(value, options) {
  if (!numberIsFinite(value)) {
    return null;
  }

  var mag = Math.abs(value);
  var thousandsSeparator = (options && options.thousandsSeparator) || '';
  var unitSeparator = (options && options.unitSeparator) || '';
  var decimalPlaces = (options && options.decimalPlaces !== undefined) ? options.decimalPlaces : 2;
  var fixedDecimals = Boolean(options && options.fixedDecimals);
  var unit = (options && options.unit) || '';

  if (!unit || !map[unit.toLowerCase()]) {
    if (mag >= map.tb) {
      unit = 'TB';
    } else if (mag >= map.gb) {
      unit = 'GB';
    } else if (mag >= map.mb) {
      unit = 'MB';
    } else if (mag >= map.kb) {
      unit = 'kB';
    } else {
      unit = 'B';
    }
  }

  var val = value / map[unit.toLowerCase()];
  var str = val.toFixed(decimalPlaces);

  if (!fixedDecimals) {
    str = str.replace(formatDecimalsRegExp, '$1');
  }

  if (thousandsSeparator) {
    str = str.replace(formatThousandsRegExp, thousandsSeparator);
  }

  return str + unitSeparator + unit;
}
```
- example usage
```shell
...

## Usage

'''js
var bytes = require('bytes');
'''

#### bytes.format(number value, [options]): string｜null

Format the given value in bytes into a string. If the value is negative, it is kept as such. If it is a float, it is
 rounded.

**Arguments**

| Name    | Type   | Description        |
...
```

#### <a name="apidoc.element.bytes.parse"></a>[function <span class="apidocSignatureSpan">bytes.</span>parse (val)](#apidoc.element.bytes.parse)
- description and source-code
```javascript
function parse(val) {
  if (typeof val === 'number' && !isNaN(val)) {
    return val;
  }

  if (typeof val !== 'string') {
    return null;
  }

  // Test if the string passed is valid
  var results = parseRegExp.exec(val);
  var floatValue;
  var unit = 'b';

  if (!results) {
    // Nothing could be extracted from the given string
    floatValue = parseInt(val, 10);
    unit = 'b'
  } else {
    // Retrieve the value and the unit
    floatValue = parseFloat(results[1]);
    unit = results[4].toLowerCase();
  }

  return Math.floor(map[unit] * floatValue);
}
```
- example usage
```shell
...
// output: '2kB'

bytes(1024, {unitSeparator: ' '});
// output: '1 kB'

'''

#### bytes.parse(string｜number value): number｜null

Parse the string value into an integer in bytes. If no unit is given, or 'value'
is a number, it is assumed the value is in bytes.

Supported units and abbreviations are as follows and are case-insensitive:

* 'b' for bytes
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
