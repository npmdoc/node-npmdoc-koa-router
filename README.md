# api documentation for  [koa-router (v7.1.1)](https://github.com/alexmingoia/koa-router#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-koa-router.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-koa-router) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-koa-router.svg)](https://travis-ci.org/npmdoc/node-npmdoc-koa-router)
#### Router middleware for koa. Provides RESTful resource routing.

[![NPM](https://nodei.co/npm/koa-router.png?downloads=true)](https://www.npmjs.com/package/koa-router)

[![apidoc](https://npmdoc.github.io/node-npmdoc-koa-router/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-koa-router_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-koa-router/build..beta..travis-ci.org/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-koa-router/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-koa-router/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Alex Mingoia",
        "email": "talk@alexmingoia.com"
    },
    "bugs": {
        "url": "https://github.com/alexmingoia/koa-router/issues"
    },
    "dependencies": {
        "debug": "^2.2.0",
        "http-errors": "^1.3.1",
        "koa-compose": "^3.0.0",
        "methods": "^1.0.1",
        "path-to-regexp": "^1.1.1"
    },
    "description": "Router middleware for koa. Provides RESTful resource routing.",
    "devDependencies": {
        "expect.js": "^0.3.1",
        "gulp": "^3.8.11",
        "gulp-mocha": "^2.0.0",
        "jsdoc-to-markdown": "^1.1.1",
        "koa": "^2.0.0-alpha.3",
        "mocha": "^2.0.1",
        "should": "^6.0.3",
        "supertest": "^1.0.1"
    },
    "directories": {},
    "dist": {
        "shasum": "38cce6b1381fada18cfa47a3b3323e995ca231cf",
        "tarball": "https://registry.npmjs.org/koa-router/-/koa-router-7.1.1.tgz"
    },
    "engines": {
        "node": ">= 4"
    },
    "files": [
        "lib"
    ],
    "gitHead": "b378984f0466c4cb1dbe25f874ba3bf46594e634",
    "homepage": "https://github.com/alexmingoia/koa-router#readme",
    "keywords": [
        "koa",
        "middleware",
        "router",
        "route"
    ],
    "license": "MIT",
    "main": "lib/router.js",
    "maintainers": [
        {
            "name": "amingoia",
            "email": "talk@alexmingoia.com"
        }
    ],
    "name": "koa-router",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/alexmingoia/koa-router.git"
    },
    "scripts": {
        "docs": "NODE_ENV=test node node_modules/gulp/bin/gulp.js docs",
        "test": "NODE_ENV=test node node_modules/gulp/bin/gulp.js test"
    },
    "version": "7.1.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module koa-router](#apidoc.module.koa-router)
1.  [function <span class="apidocSignatureSpan">koa-router.</span>layer (path, methods, middleware, opts)](#apidoc.element.koa-router.layer)
1.  [function <span class="apidocSignatureSpan">koa-router.</span>url (path, params)](#apidoc.element.koa-router.url)
1.  object <span class="apidocSignatureSpan">koa-router.</span>layer.prototype

#### [module koa-router.layer](#apidoc.module.koa-router.layer)
1.  [function <span class="apidocSignatureSpan">koa-router.</span>layer (path, methods, middleware, opts)](#apidoc.element.koa-router.layer.layer)

#### [module koa-router.layer.prototype](#apidoc.module.koa-router.layer.prototype)
1.  [function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>captures (path)](#apidoc.element.koa-router.layer.prototype.captures)
1.  [function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>match (path)](#apidoc.element.koa-router.layer.prototype.match)
1.  [function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>param (param, fn)](#apidoc.element.koa-router.layer.prototype.param)
1.  [function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>params (path, captures, existingParams)](#apidoc.element.koa-router.layer.prototype.params)
1.  [function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>setPrefix (prefix)](#apidoc.element.koa-router.layer.prototype.setPrefix)
1.  [function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>url (params)](#apidoc.element.koa-router.layer.prototype.url)



# <a name="apidoc.module.koa-router"></a>[module koa-router](#apidoc.module.koa-router)

#### <a name="apidoc.element.koa-router.layer"></a>[function <span class="apidocSignatureSpan">koa-router.</span>layer (path, methods, middleware, opts)](#apidoc.element.koa-router.layer)
- description and source-code
```javascript
function Layer(path, methods, middleware, opts) {
  this.opts = opts || {};
  this.name = this.opts.name || null;
  this.methods = [];
  this.paramNames = [];
  this.stack = Array.isArray(middleware) ? middleware : [middleware];

  methods.forEach(function(method) {
    var l = this.methods.push(method.toUpperCase());
    if (this.methods[l-1] === 'GET') {
      this.methods.unshift('HEAD');
    }
  }, this);

  // ensure middleware is a function
  this.stack.forEach(function(fn) {
    var type = (typeof fn);
    if (type !== 'function') {
      throw new Error(
        methods.toString() + " '" + (this.opts.name || path) +"': 'middleware' "
        + "must be a function, not '" + type + "'"
      );
    }
  }, this);

  this.path = path;
  this.regexp = pathToRegExp(path, this.paramNames, this.opts);

  debug('defined route %s %s', this.methods, this.opts.prefix + this.path);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.koa-router.url"></a>[function <span class="apidocSignatureSpan">koa-router.</span>url (path, params)](#apidoc.element.koa-router.url)
- description and source-code
```javascript
url = function (path, params) {
    return Layer.prototype.url.call({path: path}, params);
}
```
- example usage
```shell
...
      * [.get|put|post|patch|delete](#module_koa-router--Router+get|put|post|patch|delete) ⇒ <code>Router</code>
      * [.routes](#module_koa-router--Router+routes) ⇒ <code>function</code>
      * [.use([path], middleware, [...])](#module_koa-router--Router+use) ⇒ <code>Router</code>
      * [.prefix(prefix)](#module_koa-router--Router+prefix) ⇒ <code>Router</code>
      * [.allowedMethods([options])](#module_koa-router--Router+allowedMethods) ⇒ <code>function</code>
      * [.redirect(source, destination, code)](#module_koa-router--Router+redirect) ⇒ <code>Router</code>
      * [.route(name)](#module_koa-router--Router+route) ⇒ <code>Layer</code> &#124; <code>false</code>
      * [.url(name, params)](#module_koa-router--Router+url) ⇒ <code>String</code> &#124; <code>Error</code>
      * [.param(param, middleware)](#module_koa-router--Router+param) ⇒ <code>Router</code>
    * _static_
      * [.url(path, params)](#module_koa-router--Router.url) ⇒ <code>String</code>

<a name="exp_module_koa-router--Router"></a>
### Router ⏏
**Kind**: Exported class
...
```



# <a name="apidoc.module.koa-router.layer"></a>[module koa-router.layer](#apidoc.module.koa-router.layer)

#### <a name="apidoc.element.koa-router.layer.layer"></a>[function <span class="apidocSignatureSpan">koa-router.</span>layer (path, methods, middleware, opts)](#apidoc.element.koa-router.layer.layer)
- description and source-code
```javascript
function Layer(path, methods, middleware, opts) {
  this.opts = opts || {};
  this.name = this.opts.name || null;
  this.methods = [];
  this.paramNames = [];
  this.stack = Array.isArray(middleware) ? middleware : [middleware];

  methods.forEach(function(method) {
    var l = this.methods.push(method.toUpperCase());
    if (this.methods[l-1] === 'GET') {
      this.methods.unshift('HEAD');
    }
  }, this);

  // ensure middleware is a function
  this.stack.forEach(function(fn) {
    var type = (typeof fn);
    if (type !== 'function') {
      throw new Error(
        methods.toString() + " '" + (this.opts.name || path) +"': 'middleware' "
        + "must be a function, not '" + type + "'"
      );
    }
  }, this);

  this.path = path;
  this.regexp = pathToRegExp(path, this.paramNames, this.opts);

  debug('defined route %s %s', this.methods, this.opts.prefix + this.path);
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.koa-router.layer.prototype"></a>[module koa-router.layer.prototype](#apidoc.module.koa-router.layer.prototype)

#### <a name="apidoc.element.koa-router.layer.prototype.captures"></a>[function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>captures (path)](#apidoc.element.koa-router.layer.prototype.captures)
- description and source-code
```javascript
captures = function (path) {
  if (this.opts.ignoreCaptures) return [];
  return path.match(this.regexp).slice(1);
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.koa-router.layer.prototype.match"></a>[function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>match (path)](#apidoc.element.koa-router.layer.prototype.match)
- description and source-code
```javascript
match = function (path) {
  return this.regexp.test(path);
}
```
- example usage
```shell
...
* @param {String} path
* @returns {Array.<String>}
* @private
*/

Layer.prototype.captures = function (path) {
 if (this.opts.ignoreCaptures) return [];
 return path.match(this.regexp).slice(1);
};

/**
* Generate URL for route using given 'params'.
*
* @example
*
...
```

#### <a name="apidoc.element.koa-router.layer.prototype.param"></a>[function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>param (param, fn)](#apidoc.element.koa-router.layer.prototype.param)
- description and source-code
```javascript
param = function (param, fn) {
  var stack = this.stack;
  var params = this.paramNames;
  var middleware = function (ctx, next) {
    return fn.call(this, ctx.params[param], ctx, next);
  };
  middleware.param = param;

  var names = params.map(function (p) {
    return p.name;
  });

  var x = names.indexOf(param);
  if (x > -1) {
    // iterate through the stack, to figure out where to place the handler fn
    stack.some(function (fn, i) {
      // param handlers are always first, so when we find an fn w/o a param property, stop here
      // if the param handler at this part of the stack comes after the one we are adding, stop here
      if (!fn.param || names.indexOf(fn.param) > x) {
        // inject this param handler right before the current item
        stack.splice(i, 0, middleware);
        return true; // then break the loop
      }
    });
  }

  return this;
}
```
- example usage
```shell
...
      * [.routes](#module_koa-router--Router+routes) ⇒ <code>function</code>
      * [.use([path], middleware, [...])](#module_koa-router--Router+use) ⇒ <code>Router</code>
      * [.prefix(prefix)](#module_koa-router--Router+prefix) ⇒ <code>Router</code>
      * [.allowedMethods([options])](#module_koa-router--Router+allowedMethods) ⇒ <code>function</code>
      * [.redirect(source, destination, code)](#module_koa-router--Router+redirect) ⇒ <code>Router</code>
      * [.route(name)](#module_koa-router--Router+route) ⇒ <code>Layer</code> &#124; <code>false</code>
      * [.url(name, params)](#module_koa-router--Router+url) ⇒ <code>String</code> &#124; <code>Error</code>
      * [.param(param, middleware)](#module_koa-router--Router+param) ⇒ <code>Router</code>
    * _static_
      * [.url(path, params)](#module_koa-router--Router.url) ⇒ <code>String</code>

<a name="exp_module_koa-router--Router"></a>
### Router ⏏
**Kind**: Exported class
<a name="new_module_koa-router--Router_new"></a>
...
```

#### <a name="apidoc.element.koa-router.layer.prototype.params"></a>[function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>params (path, captures, existingParams)](#apidoc.element.koa-router.layer.prototype.params)
- description and source-code
```javascript
params = function (path, captures, existingParams) {
  var params = existingParams || {};

  for (var len = captures.length, i=0; i<len; i++) {
    if (this.paramNames[i]) {
      var c = captures[i];
      params[this.paramNames[i].name] = c ? safeDecodeURIComponent(c) : c;
    }
  }

  return params;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.koa-router.layer.prototype.setPrefix"></a>[function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>setPrefix (prefix)](#apidoc.element.koa-router.layer.prototype.setPrefix)
- description and source-code
```javascript
setPrefix = function (prefix) {
  if (this.path) {
    this.path = prefix + this.path;
    this.paramNames = [];
    this.regexp = pathToRegExp(this.path, this.paramNames, this.opts);
  }

  return this;
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.koa-router.layer.prototype.url"></a>[function <span class="apidocSignatureSpan">koa-router.layer.prototype.</span>url (params)](#apidoc.element.koa-router.layer.prototype.url)
- description and source-code
```javascript
url = function (params) {
  var args = params;
  var url = this.path;
  var toPath = pathToRegExp.compile(url);

  // argument is of form { key: val }
  if (typeof params != 'object') {
    args = Array.prototype.slice.call(arguments);
  }

  if (args instanceof Array) {
    var tokens = pathToRegExp.parse(url);
    var replace = {};
    for (var len = tokens.length, i=0, j=0; i<len; i++) {
      if (tokens[i].name) replace[tokens[i].name] = args[j++];
    }
    return toPath(replace);
  }
  else {
    return toPath(params);
  }
}
```
- example usage
```shell
...
      * [.get|put|post|patch|delete](#module_koa-router--Router+get|put|post|patch|delete) ⇒ <code>Router</code>
      * [.routes](#module_koa-router--Router+routes) ⇒ <code>function</code>
      * [.use([path], middleware, [...])](#module_koa-router--Router+use) ⇒ <code>Router</code>
      * [.prefix(prefix)](#module_koa-router--Router+prefix) ⇒ <code>Router</code>
      * [.allowedMethods([options])](#module_koa-router--Router+allowedMethods) ⇒ <code>function</code>
      * [.redirect(source, destination, code)](#module_koa-router--Router+redirect) ⇒ <code>Router</code>
      * [.route(name)](#module_koa-router--Router+route) ⇒ <code>Layer</code> &#124; <code>false</code>
      * [.url(name, params)](#module_koa-router--Router+url) ⇒ <code>String</code> &#124; <code>Error</code>
      * [.param(param, middleware)](#module_koa-router--Router+param) ⇒ <code>Router</code>
    * _static_
      * [.url(path, params)](#module_koa-router--Router.url) ⇒ <code>String</code>

<a name="exp_module_koa-router--Router"></a>
### Router ⏏
**Kind**: Exported class
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
