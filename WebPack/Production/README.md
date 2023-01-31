# Production | Webpack

Let's have a glance in nutshell, in **development** we mainly focuses in the **strong source mapping, localhost server with live reloading, or HMR.** Where-else in **production**, we mainly focuses on **bundled files, lightweight source map, optimized assets to speed up the load time,** etc. Webpack recommends to use the seperate config files for both the modes. We can merge that both config files without duplication by `wepack-merge` package.

> *Initially, you may see something like this.*

```javascript
 webpack-demo
 |- package.json
 |- package-lock.json
 |- webpack.config.js
 |- webpack.common.js
 |- webpack.dev.js
 |- webpack.prod.js
 |- /dist
 |- /src
    |- index.js
    |- math.js
 |- /node_modules
```

Now install the `webpack-merge` in your dev-dependency:
```javascript
npm i --save-dev webpack-merge
```

Your `webpack.common.js` should contain the common webpack configurations like **entry point, output, plugins, loaders which are common,** etc.
```javascript
 const path = require('path');
 const HtmlWebpackPlugin = require('html-webpack-plugin');

 module.exports = {
   entry: {
     main: './src/index.js',
   },
   output: {
     filename: '[name].bundle.js',
     path: path.resolve(__dirname, 'dist'),
     clean: true,
   },
   plugins: [
     new HtmlWebpackPlugin({
       title: 'Production | Webpack',
     }),
   ],
 };
```

And your `webpack.dev.js` should contain all the required devTool, mode, etc.
```javascript
 const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'development',
   devtool: 'inline-source-map',
   devServer: {
     static: './dist',
   },
 });
```

Similarly, for `webpack.prod.js`
```javascript
 const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'production',
 });
```
