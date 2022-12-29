# ReactJS with Dhruv Parmar
It will cover all you need to know in ReactJS.

## What is CRA in React JS ?

CRA stands for Create-react-app through npm or npx. Create-react-app is an official way to create react single page application. [`npx`](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) means Node Package eXecute. And it also provides to build `create-react-app`, in the `npm` versions which are higher than `5.2.+`. 

By creating react app through `npx create-react-app`, there is no need to create config file of [`webpack`](https://webpack.js.org/concepts/#entry) and [`Babel`](https://babeljs.io/docs/en/). It provides the hidden feature of `webpack` and `Babel`, which is installed by `npx` when you run `npx create-react-app my-app` command.

> I prefer to read the official documentation of [Create-react-app](https://create-react-app.dev/docs/getting-started/).


### To create your first react app with `npx` :
```javascript
npx create-react-app my-first-app
cd my-first-app
npm start
```

After running the above commands, you will have some files and directory created automatically by it. It will create `node_modules` directory, in which you will find all the packages installed and what's ever that packages will have `dependencies` will be all installed with it. Other files and directories are `src`, [`package.json`](https://docs.npmjs.com/cli/v9/configuring-npm/package-json), `.gitignore`, `public`, and `README.md` file.


## Folder Structure :
After running `npx create-react-app my-first-app`, your folder structure should looks something like this:

```
my-app/
  README.md
  node_modules/
  package.json
  public/
    index.html
    favicon.ico
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
```

Here `public/index.html` is your default page template. If you wanna create your own templates you can [visit here](https://create-react-app.dev/docs/custom-templates) for more info. You will also find `src/index.js` which is your entry point of the app. Only files inside `src` are processed by the webpack. So, we also creates `assets` folder inside it.

## Available Scripts :
After running `npx create-react-app my-first-app` command, you will have few scripts with you to perform it:
1. start
2. test
3. build
4. eject

### 1.  `npm start` :
It is used to run your app in development mode or you can say, it is used to start your packages. If the `scripts` object inside `package.json` doesn't defines a `start` property, npm will run `node server.js` by default.

> *By default it runs on `3000` port of localhost.*

### Whats the difference between `npm run start` and `npm start` ?
---
`npm start` is the short form for `npm run start`.

### 2.  `npm test` :

