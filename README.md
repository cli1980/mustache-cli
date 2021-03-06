mustache-cli
===

Description
---

Mustache's CLI interface.

Usage
---

```sh
npm install mustache-cli --global
mustache-cli -h
```

_./tpl/layout.mustache_

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{title}}</title>
  </head>
  <body>
    <div>
      {{>tpl}}
    </div>
  </body>
</html>
```

_./tpl/page.mustache_

```html
<h1>{{title}}</h1>
{{>content}}
```

_./conf/index.json_
```json
{
    "__root": "layout.mustache",
    "__tpl": "page.mustache",
    "_content": "<strong>Hello World</strong>",
    "title": "Home"
}
```

_./conf/multi.js_
```js
const output = require('mustache-cli').output

module.exports = function(opts){
  return {
    __file: 'multi/index.html',
    __root: 'layout.mustache',
    _tpl: '{{{ html }}}',
    title: 'Multi',
    html: function(){
      const page1 = output({
        __root: 'page.mustache',
        _content: '<p>page1</p>',
        title: this.title,
      }, opts)
      const page2 = output({
        __root: 'page.mustache',
        _content: '<p>page2</p>',
        title: this.title,
      }, opts)
      return page1 + page2
    }
  }
}
```

```sh
mustache-cli -p --color ./
```

_./out/index.html_
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <div>
      <h1>Home</h1>
      <strong>Hello World</strong>
    </div>
  </body>
</html>
```


_./out/multi/index.html_
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Multi</title>
  </head>
  <body>
    <div>
      <h1>Multi</h1>
      <p>page1</p>
      <h1>Multi</h1>
      <p>page2</p>
    </div>
  </body>
</html>
```

Node
---

```js
import express from 'express'
import {renderFile} from 'mustache-cli/lib/express'

const app = express()

app.set('views', 'src/mustache/tpl')
app.set('view engine', 'mustache')
app.engine('mustache', renderFile({
  baseDir: 'src/mustache',
}))

app.use((req, res) => {
  res.render('layout', {
    title: 'Home',
  })
})

app.listen(3000)
```

API
---

### `output(config[, options])`

options:

* `baseDir`

  (Default: `.`)

  Set the root dir for mustache.

* `confDir`

  (Default: `./conf`)

  Set the config dir, include js or json files.

* `tplDir`

  (Default: `./tpl`)

  Set the template dir, include mustache or html files.

* `outDir`

  (Default: `./out`)

  Set the output dir.

* `rootTpl`

  (Default: `__root`)

  Set the key of a main template file path.

* `outFile`

  (Default: `__file`, Since: _2.2.0+_)

  Set the key of a output file path.


* `partialPrefix`

  (Default: `_`)

  Set the prefix, represent a partial.

  ```html
  // HTML
  <div>{{>nav}}</div>

  // JSON
  {
    _nav: '<nav>...</nav>',
    ...
  }
  ```
  
* `tplPrefix`

  (Default: `__`)

  Set the prefix, represent a template file path for a partial.

  ```html
  // HTML
  <div>{{>form}}</div>

  // JSON
  {
    __form: 'form.mustache',
    ...
  }
  ```

* `forceMinify`

  (Default: `__minify`, Since: _2.3.0+_)

  Set the key of minifier, The possible value is `true` or `false`, for config file.

* `forcePretty`

  (Default: `__pretty`, Since: _2.3.0+_)

  Set the key of pretty, The possible value is `true` or `false`, for config file.

* `ext`

  (Default: `html`)

  Set the extname of output files.

* `render`

  (Default: `mustache.render`)

  Set render function.

* `print`

  Set log output function.

* `onError`

  The callback is called when an error occurs.

* `color`

  (Default: `false`)

  Print color text.

* `minify`

  (Default: `false`)

  To minify HTML.

* `pretty`

  (Default: `false`)

  To pretty HTML.

* `watch`

  (Default: `false`)

  To watch working files.

* ~~`config`~~

  (Deprecated)

### `setGlobalData(data)`

  Set default data.

### `getGlobalData()`

  Get default data.

License
---

MIT
