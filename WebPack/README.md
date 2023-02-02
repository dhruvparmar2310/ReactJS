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

  - Manually creating `dist/index.html`
  - config webpack through `npx webpack --config webpack.config.js`
  - by running `npm run build`

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
> *Refer this blog for [Development](./Development/README.md) | Webpack.*

## Code Splitting :
> *Refer this blog for [Code Spliting](./CodeSpliting/README.md) | Webpack.*

## Caching :

This allows sites to load faster with less unnecessary network traffic.

In short, we can say that, if I open Webpack website and close and again open that webpack, it may download the source content due to this it may take time. So to solve this issue, there is a technique called caching.

Caching is used to store few needed details. Which will speed the performance instead of downloading the content again. But if there is any changes made in that website, than it may lack due to that.

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

DLL stands for **Dynamic Link Library**. It help in code reuse, reduce disk space, efficient memory usage, etc. So, the OS load faster and run faster. To use DLL plugin, two plugins must be installed in your webpack config :
  - DllReferencePlugin: it is used for un-vendor code, which can be modifed. It is your primary webpack config file (i.e.: `webpack.config.js`).
  - DllPlugin: it is used for vendor code, which does not change, it remains as it is such as all libraries in your *node_modules* folder. It will config your `webpack.vendor.config.js`.

> *Refer this [blog](https://blog.logrocket.com/speed-up-your-webpack-build-with-the-dll-plugin/) for better understanding.*

### Build Performance - Development Mode:
There are few things which you must keep in mind during Development mode, such as, incremental build, compile in memory, devTools, avoid production specific toolings, minimal entry chunk, output without path info, typescript loader, etc.

*Incremental Build* :
- Use webpack watch mode as much as possible.
- Avoid using other tools to watch your files.
- In some setups, watching falls back to polling mode.
- If there are so many watch files, this can cause a issue to load CPU. In these cases, you can increase the polling interval with `watchOptions.poll`.


## Content Security Policy (CSP):
Before going further, you need to know what is `nonce` in CSP. Content-Security-Policy is the name of a HTTP response header that modern browsers use to enhance the security of the document (or web page).

### What is `Nonce` in CSP ?
A nonce is a **random number** used only once per page load, which can make ```<script>``` tag as trusted. A nonce-based CSP can only mitigate/reduce XSS(Cross Site Scripting) if the nonce value is not guessable by an attacker.

> *Refer this [blog](https://web.dev/strict-csp/) for more details.*
> *[Click here](CSP/README.md) to Read more.*


## Development - Vagrant :
If you have a more advanced project and use Vagrant to run your development environment in a Virtual Machine, you'll often want to also run webpack in the VM.

Developers describe Vagrant as "A tool for building and distributing development environments". Vagrant provides the framework and configuration format to create and manage complete portable development environments. These development environments can live on your computer or in the cloud, and are portable between Windows, Mac OS X, and Linux.

Vagrant belongs to "Virtual Machine Management" category of the tech stack, while Webpack can be primarily classified under "JS Build Tools / JS Task Runners".

Vagrant is a tool which simplifies the management of Virtual Machine.

> *Refer this [video](https://youtu.be/oo7RhkQ_DLQ) of Vagrant.*

### What is Virtual Machine ?
If I am using Mac OS in my computer and for project development, I need Windows OS in some case, than I can build my own OS in Virtual Machine for the orgainization and can use that Virtual Machine in other computers too, instead of downloading Windows in other computers.


## Hot Module Replacement :
HMR is the most commonly used feature of webpack. It allows the feature like, all the modules should be updated during the runtime without refreshing the whole module. HMR exchanges, adds, or removes modules while an application is running, without a full reload. 

HMR should only be used in **development mode** rather than in production mode.

## Tree Shaking :

```javascript
        | Application |
| node-modules | | un-used node-modules |
| node-modules |
```
Tree shaking is commonly used in the JavaScript context for **dead-code elimination**.

Let's move further, with an example. Initially you will have few things with you:
```javascript
webpack-demo
|- package.json
|- package-lock.json
|- webpack.config.js
|- /dist
 |- bundle.js
 |- index.html
|- /src
 |- index.js
 |- cake.js
|- /node_modules
```

> *src/cake.js*

```javascript
export function cake () {
  console.log('Cake is being ordered.')
}

export function pestryCake () {
  console.log('Pestry Cake is being ordered.')
}
```

Now, setup the mode of your `webpack.config.js` file:
```javascript
const path = require('path');

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
 },
 mode: 'development', /* set mode to development */
 optimization: {
   usedExports: true, /* set usedExports to true because, it will check all the used and un-used exports and make a list of it during build process. */
 },
};
```

> *src/index.js*

```javascript
 import { cake } from './cake.js';

 function component() {
   const element = document.createElement('pre');

   element.innerHTML = [
     'Hello webpack!', 
     'calling ', 'cake.js'].join('\n\n');

    return element;
  }

  document.body.appendChild(component());
```

Now, run the `npm run build` to see the `bundle.js` file. You will find the un-used export is also included in your `bundle.js` file. You will find pestryCake function is also included in your `bundle.js` file but with that you will also find something like:

```javascript
/* unused harmony export square */ /* <-- */
/* ... */
function cake () {
  console.log('Cake is being ordered.')
}

function pestryCake () {
  console.log('Pestry Cake is being ordered.')
}
```

There are few rules to be followed for tree shaking:
- use the ES5 syntax of `import` and `export`.
- use `sideEffects` in your `package.json` file. SideEffects means **free from un-used code**.

We can also set `sideEffects` in `webpack.config.js` file through **module.rules.sideEffetcs**. The sideEffects and usedExports (**more known as tree shaking**) optimizations are two different things. `usedExports` relies on terser to detect side effects in statements. Terser is used by the plugin, it will minimize your javascript code. Now to finally remove the dead-code from the `bundle.js` file too, remove the `optimization.useExports` and make mode to `production`.

Now run again `npm run build`, you will be able to see the changes. You will find your code is optimized. Now try to find `pestryCake` function in your `bundle.js` file in your text-editior, there is nothing like `pestryCake` in it.

## Production :
The few things which must do in **production** mode. It mainly focuses on '**bundled files**'.

> Refer this blog for [Production](./Production/README.md) | Webpack

## Lazy Loading :
Lazy loading is one of the most common design patterns used in web and mobile development. It is widely used with frameworks like Angular and React to increase an application's performance by reducing initial loading time.

> Refer this blog for more information of [Lazy Loading](./LazyLoading/README.md) | Webpack

> Also Refer [this blog](https://www.cloudflare.com/en-gb/learning/performance/what-is-lazy-loading/) for getting clear idea about Lazy Loading.

> [How to implement Lazy Loading in React](https://www.cloudflare.com/en-gb/learning/performance/what-is-lazy-loading/) ?

## ECMAScript Modules :
ECMAScript stands for **European Computer Manufacturers Association Script**. It is a scripting language based on JavaScript. Invented by **Brendan Eich** at Netscape, ECMAScript made its first appearance in the **Navigator 2.0** browser. The ECMAScript specification is a blueprint for creating a scripting language. JavaScript is an implementation of that blueprint.

**ECMAScript Modules** (ESM) is a specification for using Modules in the Web. It's supported by all modern browsers and the recommended way of writing modular code for the Web.

Webpack supports processing ECMAScript Modules to optimize them. ESM are the official standard format to **package JavaScript code** for reuse. Modules are defined using a variety of **import** and **export** statements.


A file with `.mjs` extension is a JavaScript source code file that is used as an ECMA Module (ECMAScript Module) in Node. Refer this [blog](https://docs.fileformat.com/web/mjs/) for more.

## Shimming :
A shim is a piece of code **used to correct the behavior of code** that already exists, usually by adding new API that works around the problem. This differs from a polyfill, which implements a new API that is **not supported** by the stock browser as shipped.


## Web Worker :
Web Workers are a simple means for web content to **run scripts in background threads**. The worker thread can perform tasks without interfering with the **user interface**.

WebSockets allow web applications to open a channel to interact with web services. Web Workers permit them to run nontrivial tasks without locking the browser.

A worker is an object created using a constructor (**e.g. Worker()**) that runs a named JavaScript file — this file contains the code that will run in the worker thread; workers run in another global context that is different from the current window. 

All you wanna do is to wrap your worker to access code in your `main.js` file:
```javascript
if (window.Worker) {
  /* ... */
}
```

Now, create a new worker by just calling `Worker()` constructor, which accepts the URL script file, which is going to work in workers thread without blocking the UI. 

There are mainly three types of web workers :
- Dedicated
- Shared
- Service

### Service Worker:
Service workers only run over HTTPS, for security reasons. Service workers essentially act as proxy servers that sit between web applications, the browser, and the network (when available). They are intended, among other things, to enable the creation of effective offline experiences, intercept network requests and take appropriate action based on whether the network is available, and update assets residing on the server.

Your service worker will observe the following lifecycle:
- Download
- Install
- Activate

The service worker is immediately downloaded when a user first accesses a service worker–controlled site/page.

> *Refer this [official docs](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) for more details.*
