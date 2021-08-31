# 01-revealing-module-pattern

This example demonstrates the revealing module pattern.

## Run

To run the example launch:

```bash
node index.js
```

```nodejs
exports.loaded = false
const b = require('./b')

exports.test = "a"

module.exports = {
  b,
  loaded: true // overrides the previous export
}

```
