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

### Manifest :
How webpack and its plugins knows what files are being generated. All the thing is done through **manifest files**. To create manifest file automatically through the plugin, install the below plugin
```javascript
npm install webpack-nano webpack-manifest-plugin --save-dev
```
Here, webpack-nano allows users to use file types like webpack.config.babel.js , webpack.config.es6 , webpack.config.mjs , and webpack.config.ts, etc.

Add the below code in your `webpack.config.js` file
```javascript
const { WebpackManifestPlugin } = require('webpack-manifest-plugin');

// also add this too
plugins: [
    new WebpackManifestPlugin()
  ]
```
And run the `npm run build` again, you will find `manifest.json` file generated automatically. It contains the json-object of filename's as a key and build filename as its value.


## Development :
It is used to set the modes of your project. it is used to avoid the warnings of mode options during `npm run build`. You need to specifies mode in your `webpack.config.js` file.

```javascript
module.export = {
  mode: 'development',
  entry: ...
}
```

There are three modes **none**, **production** and **development**. The plugin used here, **html-webpack-plugin** will build bundle file in optimzed form in production mode by default. 

There are two ways to define modes :
  1) inside the script in `package.json`
  2) inside your `webpack.config.js` file

### 1) inside the script in `package.json`
```javascript
"scripts": {
  "build": "webpack --mode=development"
}
```

### 2) inside your `webpack.config.js` file
```javascript
"mode": "development"
```


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

### 1) Multiple Entry points :
We can have multiple entry points in our project. To define a multiple entry points in `webpack.config.js` file, write the elow code:
```javascript
entry: {
  index: './src/index.js',
  another: './src/another-module.js',
}
```

If there are any duplicated modules between entry chunks they will be included in both bundles. As lodash is also imported within `./src/index.js` and will thus be duplicated in both bundles. To seperate that duplication from both the bundles, **prevent duplication** comes in picture.

### Prevent Duplication :
By default, every entry chunk stores all the modules that it uses. The `dependOn` option allows to share the modules between the chunks:
```javascript
   index: {
      import: './src/index.js',
      dependOn: 'shared',
    },
    another: {
      import: './src/another-module.js',
      dependOn: 'shared',
    },
    shared: 'lodash',
```
If we are working with multiple entry points in single Html file, code splitting provides `optimization.runtimeChunk` option:
```javascript
optimization: {
  runtimeChunk: "single",
}
```
After adding the above code you will have two more files generated with you, `runtime.bundle.js` and `shared.bundle.js` file. Now you can see the duplication of `lodash` is removed from both the bundles. Now that `lodash node_modules` is seperated from both the bundles and that `lodash.js` file is generated/loaded in `shared.bundle.js` file. `runtime.bundle.js` file contains, all the loaded chunks/modules in it. 

### SplitChunksPlugin :
The SplitChunksPlugin allows us to extract common dependencies into an existing entry chunk or an entirely new chunk. Now change the previous code of `webpack.config.js` file to below code:

```javascript
optimization: {
     splitChunks: {
       chunks: 'all',
     },
   },
```
Now you can see the change in `index.bundle.js` and `another.bundle.js` file, the `lodash` duplicate dependency is removed. Even the `runtime.bundle.js` file is remove and that file is included under the `shared.bundle.js` file.

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

## Caching :

This allows sites to load faster with less unnecessary network traffic.

In short, we can say that, if I open Webpack website and close and again open that webpack, it may download the source content due to this it may take time. So to solve this issue, there is a technique called caching.

Caching is used to store few needed details. Which will speed the performance instead of downloading the content again. But we there a changes made in that website, than it may lack due to that.

In caching, there is a concept of `contenthash`. It will create an unqiue key at the time of build process. If there is any change made in the file, this key automatically changes during build.

### Extracting Boilerplate :

As we know SplitChunkPlugin was used for code splitting purpose. We can also get a runtime chunk file by adding the below code in your `webpack.config.js` file. Set it to `single` to create a single runtime bundle for all chunks.
```javascript
plugin: [
...
],
optimization: {
  runtimeChunk: 'single',
}
```

To remove all the `node_modules` dependency from the `main.[contenthash].js` file, add the below code in `webpack.config.js` file:
```javascript
optimization: {
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          name: 'vendors',
          chunks: 'all',
        },
      },
    },
  },
```
After running `npm run build`, now you will have `main.[contenthash].js`, `runtime.[contenthash].js` and `vendor.[contenthash].js` file with you. You will find all the `node_modules` dependency libraries are seperated in `vendor.[contenthash].js` file from the other bundle files. You can also observe the previous file size and recent file size of `main.[contenthash].js` file.

### Module Identifier:
Initially, you should have:
```javascript
webpack-demo
|- package.json
|- package-lock.json
|- webpack.config.js
|- /dist
|- /src
  |- index.js
|- /node_modules
```

Now create one `print.js` file under `src` folder.

> *print.js*
```javascript
 export default function print(text) {
   console.log(text);
 };
```

Now update the `index.js` file
```javascript
 import _ from 'lodash';
 import Print from './print';

  function component() {
    const element = document.createElement('div');

    // Lodash, now imported by this script
    element.innerHTML = _.join(['Hello', 'webpack'], ' ');
   element.onclick = Print.bind(null, 'Hello webpack!');

    return element;
  }

  document.body.appendChild(component());
```

### What is actually runtimeChunk file ?
Runtime files contains code which enables loading of your chunks. If you open any of those runtime files, you will see code which loads your chunks via Jsonp. You are now free to load any chunks at any time. 

RuntimeChunk consists of all the code webpack needs to connect your modularized application while it's running in the browser. It contains the loading and resolving logic needed to connect your modules as they interact. A JSONP is a javascript file which just contains a function wrapping JSON data.

Here the `main.[contenthash].js` file contents all the `node_modules` libraries and `index.js` file code. After writting the below code in your `webpack.config,js` file, you will get an `vendor.[contenthash].js` file generated, which seperates the `node_modules` libraries from the `main.[contenthash].js` file.

```javascript
optimization: {
  runtimeChunk: 'single',
  splitChunks: {
    cacheGroups: {
      vendor: {
        test: /[\\/]node_modules[\\/]/,
        name: 'vendors',
        chunks: 'all',
      },
    },
  }, 
},
```

## Environment Variables :
Basically, environment variables are used to store your secure credential data like api link, api keys, path, etc. 

There are few steps to be followed to get access of `.env` files:
```javascript
/*
Step-1: You need to create ".env" file in your root folder. Also add it in your ".gitignore" file, because it will keep your data secure, no one will be able to see the api links. ".gitignore" will ignore this file during build process.

Step-2: The variable should start with prefix of "REACT_APP_" 
Step-3: Now restart your server to reflect the data.
*/
```

To use `env` in your webpack, add this in `package.json` files `scripts`.
```javascript
"build": "webpack --env production"
```

To get access of `env` in your `webpack.config.js` file, make a few changes:
```javascript
const path = require('path');

module.exports = (env) => {
  // Use env.<YOUR VARIABLE> here:
  console.log('Production: ', env.production); // true

  return {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist'),
    },
  };
}
```

Lets creates an `.env` file in the root folder.
> *.env*

```javascript
REACT_APP_SECRET_KEY=123456
REACT_APP_PASSWORD='abc@123'
```

Now, to get access of this file in your react component,
```javascript
<>
  {process.env.REACT_APP_SECRET_KEY}
</>
```
Now restart the server, so that you can get reflected data.

## Build Performance :
There are few things which you must know at the time of build process such as, the package must be up-to-date, use minimal loaders, DLL Plugin, etc.

### What is DLL Plugin ?

DLL is a webpack plugin which it creates the bundle configurations as a part of build process. It will keep track of what's ever changes made in the package and re-build the bundles accordingly.

DLL stands for Dynamic Link Library. It help in code reuse, reduce disk space, efficient memory usage, etc. So, the OS load faster and run faster. To use DLL plugin, two plugins must be installed in your webpack config :
  1) DllReferencePlugin: it is used for un-vendor code, which can be modifed. It is your primary webpack config file (i.e.: `webpack.config.js`).
  2) DllPlugin: it is used for vendor code, which does not change, it remains as it is such as all libraries in your *node_modules* folder. It will config your `webpack.vendor.config.js`.

> *Refer this [blog](https://blog.logrocket.com/speed-up-your-webpack-build-with-the-dll-plugin/) for better understanding.*

### Build Performance - Development Mode:
There are few things which you must keep in mind during Development mode, such as, incremental build, compile in memory, devTools, avoid production specific toolings, minimal entry chunk, output without path info, typescript loader, etc.

*Incremental Build* :
- Use webpack watch mode as much as possible.
- Avoid using other tools to watch your files.
- In some setups, watching falls back to polling mode.
- If there are so many watch files, this can cause a issue to load CPU. In these cases, you can increase the polling interval with `watchOptions.poll`.


## Content Security Policy (CSP):
Before going further, you need to know what is `nonce` in CSP.

### What is `Nonce` in CSP ?
A nonce is a **random number** used only once per page load. A nonce-based CSP can only mitigate XSS(Cross Site Scripting) if the nonce value is not guessable by an attacker.

> *Refer this [blog](https://web.dev/strict-csp/) for more details.*
