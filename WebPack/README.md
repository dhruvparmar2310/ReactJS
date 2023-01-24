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
