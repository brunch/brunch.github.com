# Brunch: Using JS modules and NPM

<div class="toc-placeholder"></div>

## Intro

Organizing your JS code by modules is a central idea in Brunch.
It allows your files to each have a separate scope, and also execute only when needed.

File name is also it module name.
To make things a bit nicer, Brunch cuts the `app` portion from the module name.
For example, `app/config.js` will have a module name of `config.js`.
With CommonJS, your modules will return values by putting them into `module.exports`, just like you would with Node.

```javascript
// app/config.js
module.exports = {
  api: {
    host: 'brunch.io'
  }
};
```

To use it in other modules, just `require` it! Note that you can require using both a name with extension (like `config.js`) or a name without extension (`config`):

```javascript
var config = require('config');

makeRequest(config.api, 'GET', 'plugins');
```

## Kinds of modules

Brunch supports several JS module styles:

* CommonJS (default)
* AMD
* custom wrapper & definition

You can use either of these in your projects, however CommonJS is becoming the universal standard, and certain Brunch features, namely the NPM integration, only work with CommonJS.
Additionally, most of the docs will assume you use CommonJS.

## NPM integration

Brunch supports handling client-side dependencies using the [NPM](https://npmjs.com) package manager.

### Using node modules

NPM integration is enabled by default starting Brunch 2.3 so there's no additional setup!
Simply `npm install --save` your front-end packages as you normally would, `require` them in your app, and Brunch will figure out the rest.

Just make sure that your don't forget to join `/^node_modules/` somewhere!

```coffeescript
files:
  javascripts:
    joinTo:
      'js/app.js': /^app/
      'js/vendor.js': /^node_modules/
```

### Including modules' styles

Brunch can also handle styles of client-side libraries, by providing `styles` attribute which is key-value object where key is package name and value is an array with relative to package path of styles which should be included.

```coffeescript
npm:
  styles:
    leaflet: [ 'dist/leaflet.css' ]
files:
  javascripts:
    joinTo:
      'js/vendor.js': /^node_modules/
  stylesheets:
    joinTo:
      'css/vendor.css': /^node_modules/
```

Note that for other assets that come from NPM packages (like images, fonts, etc), you will have to manually copy them to your public folder. You can use the npm's `postinstall` hook to do the copying. See [FAQ](/docs/faq.html).

### Making packages global

It's possible to expose npm packages to `window` — so that you can access the module without requiring it. See [docs](/docs/config.html#-npm-).

## Hot Module Replacement

Brunch also supports Hot Module Replacement with the [`hmr-brunch`](http://github.com/brunch/hmr-brunch) plugin. Refer to its README for more details.
