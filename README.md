# Ember-cli-amd

The Ember loader is conflicting with the AMD loader. The Ember loader is not async and doesn't comply with the RequireJS or AMD specs. 

This addon will:
* Update the code generated by Ember to avoid conflicts with the AMD loader
* Update the index.html:
  * load any pure AMD modules found in the code first using the AMD loader. 
  * load the Ember code as AMD modules (app and vendor)
  * reference the AMD modules inside the Ember loader

[View it live](http://esri.github.io/ember-cli-amd/) using the [ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/).

## Breaking changes
The version 2.x has removed some old and unecessary features.

## Features
* Load AMD modules in parallel with [ember-cli/loader.js](https://github.com/ember-cli/loader.js).
* Works with AMD CDN libraries, such as [Dojo](https://dojotoolkit.org/download/) or the [ArcGIS API for JavaScript](https://developers.arcgis.com/javascript/).

## Usage

`ember install ember-cli-amd`

Update the ember-cli-build file. See configuration below as an example:
```javascript
var app = new EmberApp({
  amd : {
    // Specify the loader path. Can be a CDN path or a relative path in the dist folder
    // - CDN: loader: 'https://js.arcgis.com/3.15/'
    // - Local: loader: 'assets/jsapi/dojo.js'
    loader: 'https://js.arcgis.com/3.15/',
    // User defined AMD packages used in the application
    // import foo from 'esri/foo';
    packages: ['esri','dojo','dojox','dijit','put-selector','xstyle','dgrid'],
    // The AMD configuration file path relative to the project root.
    // The file will be copied to the output directory (./dist) and the configuration file
    // will be loaded before the loader is loaded. The configuration file must define the global variable used by the specific loader.
    // For dojo, the supported global variable name are `dojoConfig`, `djConfig` or `require`.
    // For requirejs, the global variable is called `require`.
    // Please refer to the documentation for the correct use of the configuration object.
    configPath: 'config/dojo-config.js',
    // Optional: a list of relative paths from the build directory that should not be parsed by ember-cli-amd.
    // This is useful for:
    // - when using an AMD api locally and copied under public folder. The files will be copied under the build folder. These files are pure AMD
    //   modules and should not be converted.
    // - when copying from public to build directory files that are pure JS
    excludePaths: ['assets/jsapi', 'assets/myLibThatDontUseEmberDefineOrRequire']
  }
});
```

## Example using the CDN resources

```javascript
// ember-cli-build.js
module.exports = function(defaults) {

  var app = new EmberApp(defaults, {
    amd :{
      loader: 'https://js.arcgis.com/3.15/',
      configPath: 'config/dojo-config.js',
      packages: ['esri','dojo','dojox','dijit','put-selector','xstyle','dbind','dgrid'],
      excludePaths: ['assets/workers']
    }
  });

  return app.toTree();
};
```

```javascript
// config/dojo-config.js if using dojo
var dojoConfig = {
  async: true
};
```

## Resources
* For more information on using ember-cli, visit [http://www.ember-cli.com/](http://www.ember-cli.com/).
* To learn more about the ArcGIS API for JavaScript, visit [the developers pages](https://developers.arcgis.com/javascript/).

## Issues

Find a bug or want to request a new feature?  Please let us know by submitting an issue.

## Contributing

Esri welcomes contributions from anyone and everyone. Please see our [guidelines for contributing](https://github.com/esri/contributing).

## Licensing
Copyright 2018 Esri

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

A copy of the license is available in the repository's [LICENSE](./LICENSE.md) file
