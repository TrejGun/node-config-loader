#Node config loader

Scan directories, load, parse to js object and merge many configs into single file/object.

# Features
* vinyl-fs compatible
* Extensible via adapters for any config format
* Custom file filters

# Usage

```javascript
//npm install --save vinyl-fs toml js-aml node-config-loader
// examples/example1.js
var fs = require('vinyl-fs');
var Toml = require('toml');
var JsYaml = require('js-yaml');

var ConfigLoader = require('node-config-loader');
var Adapter = ConfigLoader.Adapter;

fs.src(['../tests/config/*.{toml,yaml}'])
  .pipe(ConfigLoader.mergeStream({
    env: 'dev',
    target: 'client',
    fileName: 'config-all.json',
    filter: ConfigLoader.Filters.generic,
    parser: Adapter.strategy({
        yml: Adapter.yaml(JsYaml),
        yaml: Adapter.yaml(JsYaml),
        toml: Adapter.toml(Toml)
    })
  }))
  .on('data', success);

function success(file) {
  console.log('done:', JSON.stringify(file.mergedData, null, '    '));
}
```
