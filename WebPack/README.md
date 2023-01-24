# Webpack guides :

> *Refer the official [documentation](https://webpack.js.org/guides/installation)*

## Getting Started
Webpack is used to compile JS modules. Once installed, you can interact with webpack through CLI or an Api.

To get started, create an empty folder called `webpack-demo` and run the below code.
```javascript
npm init -y /* it will create package.json and package-lock.json file */
npm install --save-dev webpack webpack-cli
```

After running above commands, you will have `node_modules` and `package.json`, `package-lock.json` file with you. `node_modules` will contain all the packages which are needed in the webpack, even webpack dependency packages will be installed automatically. All the dependency packages of webpack will be installed in Development mode automatically, which you can find in `package-lock.json` file.

Now create `index.html` file in your root folder and create `/src` directory and inside it create `index.js` file. Write the below code in `index.js` which is present under your `src` directory.
```javascript
function component() {
  const element = document.createElement('div');

  // Lodash, currently included via a script, is required for this line to work
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');

  return element;
}

document.body.appendChild(component());
```

Also write the below code in your `index.html` file:
```javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Getting Started</title>
    <script src="https://unpkg.com/lodash@4.17.20"></script>
  </head>
  <body>
    <script src="./src/index.js"></script>
  </body>
</html>
```

You can config webpack in three ways:
1) Manually creating `dist/index.html`
2) config webpack through `npx webpack --config webpack.config.js`
3) by running `npm run build`

## Asset Management :
Asset Management includes, loading css style, json files, images, fonts, etc.

## Output Management :
If you have multiple entry points you can use:
```javascript
entry: {
  index: './src/index.js',
  about: './src/about.js'
}
```
And change file name to this, to manage it properly:
```javascript
filename: '[name].bundle.js'
```
The about code will create `index.bundle.js` and `about.bundle.js` files.

Now to create bundle file with plugin, use **HtmlWebpackPlugin**. To install it run the below code:
```javascript
npm i --save-dev html-webpack-plugin
```
After that require() in the webpack config file, and include the plugin. It will generate bundle file automatically. If you have created it manually, it will replace with new and in optimized form.

## Development :
It is used to set the modes of your project. it is used to avoid the warnings of mode options during `npm run build`. You need to specifies mode in your `webpack.config.js` file.

```javascript
module.export = {
  mode: 'development',
  entry: ...
}
```

There are two modes **production** and **development**. The plugin used here, **html-webpack-plugin** will build bundle file in optimzed form in production mode by default.

### Source Map files:
___

Source map files are used to find the exact error which is coming after the build process in the bundle file. If you have three bundle files in single bundle.js file, then if the error is occured in third file, to catch the exact location of it with line number, source map file comes in picture. Source map files are used for debugging purpose.

> *There are many ways to use [source map](https://webpack.js.org/configuration/devtool) files.*

If you are bored to run `npm run build` everytime after changing th content of file. There are other ways available which runs `npm run build` automatically, such as :

1) webpack's **[watch mode](https://webpack.js.org/configuration/watch/#watch)**
2) [webpack-dev-server](https://webpack.js.org/guides/development/)
3) webpack-dev-middleware

In most of the cases, you will work with **webpack-dev-server** which will automatically compile your bundle file, if it get changes.

There is one disadvantage of **watch mode**, it will compile automatically if the file content is changed, but we need to reload the page manually. To fix this issue, the best solution is to use **webpack-dev-server**. **webpack-dev-server** will compile and reload the page automatically. It will open in `localhost:8080` by default.


## Code Splitting :
Code splitting can be done in three ways :
  1) Multiple entry points
  2) Prevent Duplication by runtimechunk for single page and SliptChunksPlugin to remove duplication chunks in `/dist` folder.
  3) Dynamic `import`

### Prefetching and Preloading Modules :
---
Prefetching and Preloading modules are used to improve web application performance by code slpitting.

Where,
  - Prefetch = loads the next page resource
  - Preload = loads the current page resource
  
It can be seen in the header of index.html, say for example:
```javascript
<link rel="preload" href="style.css" as="style" />
<link rel="prefetch" href="index.js" as="script" />
```

- When, you see `preload` in `rel` attribute of `<link>` tag, it means you are telling to browser to load this resources assets first. It will have **higher priority** than other resources. It will download the resource when the current page is loaded.

- When you see something like `prefetch` in `rel` attribute of `<link>` tag, it means it will load or download the resources when the browser is free. It has **low priority** than `preload`. `prefetch` can also be used at the time of **page navigation**. And after giving higher priority to the resource, if you are not using that resource at initial stage, than it will warn you in the console too which is a good feature.

- You can also watch the priority of any resource in your developing tool Network's tab. Here, after clicking to one of the column, give the **priority**. Here, you will find which resource has higher priority and which has low. 

### Bundle Analyzer :
---
Once you start splitting your code, bundle analyzer is useful. There are many several bundle analyzers :
  1) webpack-chart: it will display in pie chart.
  2) webpack-visualizer: we can visualize and analyse, it is similar to pie chart.
  3) webpack-bundle-analyzer: it is mostly used to analyze the size of your bundle.
  4) webpack bundle optimizer helper: it also provides the suggestion to improve and reduce your bundle size.
  5) bundle-stats: it will generate bundle report and compare its result with different builds.

To use **webpack-bundle-analyzer**, you need to install it first.
```javascript
npm install --save-dev webpack-bundle-analyzer
```

Now add this in your webpack config file:
```javascript
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;
```

and add this inside plugins:
```javascript
new BundleAnalyzerPlugin()
```

Now run the `npm run build`, it will open in `[localhost:8888](http://127.0.0.1:8888/)` by default, and there you can see the bundle analyzer.
