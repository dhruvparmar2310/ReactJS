# Webpack guides :

> *Refer the official [documentation](https://webpack.js.org/guides/installation)*


## Webpack Installation :
To install latest or other `version`s of webpack, run the below code
```javascript
npm i --save-dev webpack
// or for other versions
npm i --save-dev webpack@<version>
```

I prefer to install webpack in `development` mode instead of installing it in `production` mode. And If you wanna call `webpack` through command lines then you also need to install `webpack-cli`.

After running the above commands, you will have `node_modules`, `package.json`, `package-lock.json` files with you. `node_modules` will contain all the packages which are needed in the webpack, even webpack dependency packages will be installed automatically. All the dependency packages of webpack will be installed in Development mode automatically, which you can find in `package-lock.json` file.

You can also install webpack globally, but it's not recommended way, because it will lock the webpack version and you may face an issues when your project runs on different versions.

> *installing webpack globally*
```javascript
npm i --global webpack
```
