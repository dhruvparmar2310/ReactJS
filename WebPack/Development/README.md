# Development | Webpack

It is used to set the modes of your project. it is used to avoid the warnings of mode options during `npm run build`. You need to specifies mode in your `webpack.config.js` file.

```javascript
module.export = {
  mode: 'development',
  entry: ...
}
```

There are three modes **none**, **production** and **development**. The plugin used here, **html-webpack-plugin** will build bundle file in optimzed form in production mode by default. 

There are two ways to define modes :
  - inside the script in `package.json`
  - inside your `webpack.config.js` file

## 1) inside the script in `package.json`
```javascript
"scripts": {
  "build": "webpack --mode=development"
}
```

## 2) inside your `webpack.config.js` file
```javascript
"mode": "development"
```


## Source Map files:

Source map files are used to find the exact error which is coming after the build process in the bundle file. If you have three bundle files in single bundle.js file, then if the error is occured in third file, to catch the exact location of it with line number, source map file comes in picture. Source map files are used for debugging purpose.

> *There are many ways to use [source map](https://webpack.js.org/configuration/devtool) files.*

If you are bored to run `npm run build` everytime after changing the content of file. There are other ways available which runs `npm run build` automatically, such as :

  - webpack's **[watch mode](https://webpack.js.org/configuration/watch/#watch)**
  - [webpack-dev-server](https://webpack.js.org/guides/development/)
  - webpack-dev-middleware

In most of the cases, you will work with **webpack-dev-server** which will automatically compile your bundle file, if it get changes.

There is one disadvantage of **watch mode**, it will compile automatically if the file content is changed, but we need to reload the page manually. To fix this issue, the best solution is to use **webpack-dev-server**. **webpack-dev-server** will compile and reload the page automatically. It will open in `localhost:8080` by default.
