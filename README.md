# vue3-sfc-loader

Vue3 Single File Component loader.  
Load .vue files directly from your html/js. No node.js environment, no (webpack) build step.  


## Features

 * Only requires [Vue3 runtime-only](https://unpkg.com/vue@next/dist/vue.runtime.global.prod.js)
 * Focuses on component compilation. Network, styles injection and cache are up to you (see [options](docs/api/interfaces/options.md#index))
 * You can [build your own version](#build-your-own-version) and easily customize browser support
 * Properly reports template, styles or script errors through the [log callback](docs/api/interfaces/options.md#log)
 * Support custom CSS, HTML and Script language, see [pug example](docs/examples.md#using-another-template-language-pug)


## Example

```html
<html>
<body>
  <div id="app"></div>
  <script src="https://unpkg.com/vue@next"></script>
  <script src="https://cdn.jsdelivr.net/npm/vue3-sfc-loader"></script>
  <script>

    const { loadModule } = window['vue3-sfc-loader'];

    const options = {

      moduleCache: {
        vue: Vue
      },

      getFile(url) {

        return fetch(url).then(response => response.ok ? response.text() : Promise.reject(response));
      },

      addStyle(styleStr) {

        const style = document.createElement('style');
        style.textContent = styleStr;
        const ref = document.head.getElementsByTagName('style')[0] || null;
        document.head.insertBefore(style, ref);
      }

    }

    const app = Vue.createApp({
      components: {
        'my-component': Vue.defineAsyncComponent( () => loadModule('./myComponent.vue', options) )
      },
      template: '<my-component></my-component>'
    });

    app.mount('#app');

  </script>
</body>
</html>
```


## Try it

  https://codepen.io/franckfreiburger/project/editor/AqPyBr


## dist

  [![latest bundle version](https://img.shields.io/npm/v/vue3-sfc-loader?label=version)](https://github.com/FranckFreiburger/vue3-sfc-loader/blob/main/CHANGELOG.md)
  [![bundle minified size](https://img.shields.io/bundlephobia/min/vue3-sfc-loader?label=min)](#dist)
  [![bundle minified+zip size](https://img.shields.io/bundlephobia/minzip/vue3-sfc-loader?label=min%2Bzip)](#dist)
  [![bundle minified+bz2 size](https://img.shields.io/badge/min%2Bbz2-even%20smaller-blue)](#dist)
  [![Snyk Vulnerabilities for GitHub vue3-sfc-loader](https://img.shields.io/snyk/vulnerabilities/github/FranckFreiburger/vue3-sfc-loader)](#dist)
  [![browser support](https://img.shields.io/github/package-json/browserslist/FranckFreiburger/vue3-sfc-loader)](https://github.com/browserslist/browserslist#query-composition)

  latest minified version at :
  - [jsDelivr CDN](https://www.jsdelivr.com/package/npm/vue3-sfc-loader): https://cdn.jsdelivr.net/npm/vue3-sfc-loader
  - [UNPKG CDN](https://unpkg.com/browse/vue3-sfc-loader/): https://unpkg.com/vue3-sfc-loader


## Public API

  **[loadModule](docs/api/README.md#loadmodule)**(`path`: string, `options`: [Options](docs/api/interfaces/options.md)): Promise\<Module>


## Build your own version

  `webpack --config ./build/webpack.config.js --mode=production --env targetsBrowsers="> 1%, last 2 versions, Firefox ESR, not dead, not ie 11"`

  see [`package.json`](https://github.com/FranckFreiburger/vue3-sfc-loader/blob/main/package.json) "build" script  
  see [browserslist queries](https://github.com/browserslist/browserslist#queries)  


## How it works

  `vue3-sfc-loader.js` = `Webpack`( `@vue/compiler-sfc` + `@babel` )


### More details

  1. load the `.vue` file
  1. parse and compile template, script and style sections (`@vue/compiler-sfc`)
  1. transpile script and compiled template to es5 (`@babel`)
  1. parse scripts for dependencies (`@babel/traverse`)
  1. recursively resolve dependencies
  1. merge all and return the component


## Any questions ?

  ask here: https://stackoverflow.com/questions/ask?tags=vue3-sfc-loader

  [see previous questions](https://stackoverflow.com/questions/tagged/vue3-sfc-loader)


## More examples

  see [examples](docs/examples.md)


## Previous version (for vue2)

  see [http-vue-loader](https://github.com/FranckFreiburger/http-vue-loader)
