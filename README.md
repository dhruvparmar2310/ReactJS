# ReactJS with Dhruv Parmar
It will cover all you need to know in ReactJS.

## What is CRA in React JS ?

CRA stands for Create-react-app through npm or npx. Create-react-app is an official way to create react single page application. [`npx`](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b) means Node Package eXecute. And it also provides to build `create-react-app`, in the `npm` versions which are higher than `5.2.+`. 

By creating react app through `npx create-react-app`, there is no need to create config file of [`webpack`](https://webpack.js.org/concepts/#entry) and [`Babel`](https://babeljs.io/docs/en/). It provides the hidden feature of `webpack` and `Babel`, which is installed by `npx` when you run `npx create-react-app my-app` command. `npx` will install all the latest versions of package.

> *I prefer to read the official documentation of [Create-react-app](https://create-react-app.dev/docs/getting-started/).*


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
Launches the test runner in the interactive watch mode.


### 3. `npm run build` :
`npm run build` creates a `build` directory with a production build of your app. It does the same tasks which `npm start` does. Instead of checking for available ports and running a development server, the script will execute `build` function which is available in '**react-srcipts/script/build.js**' which will bundle all your seperate files into one **bundle.js** file. It will ensure, wheather your code is optimized and minified to make better performance.

### 4. `npm run eject` :

> *Note: This is one way operation. If you run `npm run eject` once time, you cannot go back.*

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project (react-scripts).

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc.) into your project as dependencies in `package.json`. All the configuration files from `react-scripts` will be copied into your project roots **react-scripts/config/** folder. And then, the **scritps** to run the build will be copied into the **scripts/** folder.

The dependency will also be moved into your root's `package.json` file. Once you have `ejected` from CRA, you can't undo it. It is mostly used to make your own configuration work on your project.

![Screen shot-1](SS-1.png)
![Screen shot-2](SS-2.png)

## Development

### Analyzing Bundle Size

Source map explorer analyzes JavaScript bundles using the source maps. This helps you understand where code bloat is coming from. Analyze and debug JavaScript (or Sass or LESS) code bloat through source maps.

The source map explorer determines which file each byte in your minified code came from. It shows you a treemap visualization to help you debug where all the code is coming from. `.js.map` file is that which is used to debug, and it is present after the build is created '**build/static/js/.js.map**' files.

What's ever the error comes, sometimes we can't get from were it is coming, so here the source map comes in picture. We can get that error through source map files.


> *[Read this blog to understand it in a better way.](https://www.bugsnag.com/blog/source-maps)*


To add `Source map explorer` to your CRA project, run this :
```
npm i --save-dev source-map-explorer
```
Then add `analyze` in `scripts` inside `package.json`:
```javascript
"analyze": "source-map-explorer 'build/static/js/*.js'",
```
Then to analyze the bundle run the below commands:
```
npm run build
npm run analyze
```


## Using HTTPS in Development
By default your app will work on **http://localhost:3000/**, but if you wanna work on **HTTPS**, then follow the steps :

### For Windows : 
Add this to `start` scripts
```javascript
set HTTPS=true&&npm start
```

### For Linux/MacOS :
Add this to `start` scripts
```javascripts
HTTPS=true npm start
```

> *Note that the server will use a self-signed certificate, so your web browser will almost definitely display a warning upon accessing the page.*

![Screen shot-3](SS-3.png)

### To generate SSL Certificate :
> *[Read this Blog](https://medium.com/swlh/how-to-make-react-js-use-https-in-development-4ead560eff10)*

### So for Custom SSL Certificate
Add this to `start` scripts

```javascript
HTTPS=true SSL_CRT_FILE=cert.crt SSL_KEY_FILE=cert.key npm start
```

## Adding a Stylesheet
- [Official Documentation](https://create-react-app.dev/docs/adding-a-stylesheet)
- [Blog of BEM](https://medium.com/seek-blog/block-element-modifying-your-javascript-components-d7f99fcab52b)
- [Explore What is BEM Methodology by its Official Site](https://en.bem.info/methodology/)

## Adding a CSS Module :
CSS Module is a **CSS file** in which all classNames and animation names are scoped locally by default. CSS Modules are convienient for components that are placed in seperate files. The CSS inside a module is available only for the components that imported it. CSS Modules allows the scoping of CSS by automatically creating a unique classname of the format `[filename]\_[classname]\_\_[hash]`. It will not **clashes** if other components with simple `.css` files have same `className`.

### How to create CSS Module file :
Create CSS Module with `.module.css` extension.

For example : create the CSS module file named as `card.module.css`
```javascript
.card {
  width: 300px;
  color: #f3f3f3;
  background-color: #2596be;
  padding: 30px;
  border-radius: 5px;
}

.card-img {
  object-fit: cover;
  width: 300px;
  height: 300px;
}

.card-content {
  background: #f3f3f3;
  color: #666;
  object-fit: cover;
}
```

Now `import` the style module in your component, say for example, I have shopping cart website, now I need this style in my `card` component:

> *Card.js*
```javascript
import styles from './card.module.css'

const Card = () => {
  return <div className={styles.card}>
            <img src={} alt={} className={styles.card-img} />
            <div className={styles.card-content}>
              <h1>Title</h1>
            </div>
         </div>
}

export default Card;
```

### What is Styled Components :
The name itself says, there will be an styled component which can be used like React Components. Styled Component is an library and its unpacked size is **3.17 MB** and there are total **325** files. It also have theme concept, by wrapping your code under `<ThemeProvider>`...`</ThemeProvider>`. With the help of this provider, you can also create a dark mode themes.

For example: I have an styled component of Title

> *title.css*
```javascript
const Title = styled.h1`
  font-size: 1.5em;
  color: red;
`;
```

> *Home.js*
```javascript
import Title from './style/Title/title.css'

const Home = () => {
  return (
    <Title>A styled component</Title>
  )
}
```

## Adding a Sass Stylesheet :
> *[Official Sass Documentation](https://sass-lang.com/documentation/syntax)*

Sass stands for **Syntactically Awesome Style-Sheet**. It is an extension of CSS. It reduce the repetition of CSS code. It was designed by Hampton Catlin. Sass lets you to use features that don't exist in CSS, like `variables`, `nested rules`, `mixin`, `@use`, `@include`, etc. The file extension can be `.scss` or `.sass`.

To use Sass, you need to install sass:
```
npm install sass
```

for example: my website runs on 3 colors than I can create variables in `_variables.scss` files and use it.
> *_variables.scss*
```javascript
$primary: #a2b9bc;
$secondary: #b2ad7f;
$main: #fff;
```

### How does Sass Works ?
A browser does not understand a Sass Code. Therefore, you will need a Sass pre-processor to convert Sass code into css. This process is known as **Transpiling**. 

Transpiling translate the sass code automatically at the time of `build`.

### What is Sass Variable ?
With Sass, you can store information in variables like `string`, `number`, `colors`, `booleans`, `list`, `nulls`, etc.

syntax:
```
$[variableName]: value;
```

## Adding a CSS Reset :
There is an option in css which is built-in feature: (*not for CSS reset, but this developer's should know.*)
```javascript
.home{
  background: red;
  margin: 30px auto;
  padding: 30px;
  all: unset; /* it will not apply the style even you have given your styles to home */
}
```

There are many ways to Reset CSS, One of the best option I found is [`normalize.css`](https://necolas.github.io/normalize.css/). All the thing you need to do is :
```
npm i normalize.css
```
 It will install normalize.css file in your project, or you can create `normalize.css` file in the root folder, and paste the content [this](https://necolas.github.io/normalize.css/8.0.1/normalize.css). The main concept of it is, it will remove duplication, reset the style to default.
 
 To use css reset, `@import` it on the `style.css` at the top:
 ```
 @import-normalize /* it will bring to normalize.css file. */
 ```

## What is the use of Public Folder ?
Generally, creating the app by `npx create-react-app`, you will be able to see something like `%PUBLIC_URL%` under the link tags in `public/` folder. 

For this you need to know few thnigs:
- `webpack` will only pre-process the `src` folder, but not the `public` folder.
- The `webpack` will only minifies the `src` folder, rather than `public`.
- To use `public` folder, we need to specifiy `%  PUBLIC_URL%` to the link tags

for example: before the `npm run build`
```javascript
<link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
```

after running `npm run build`
```javascript
<link rel="manifest" href="/manifest.json" /> 
```

It means, if we don't specify `%PUBLIC_URL%` than it will not use that file during `build` process.

## What is Code Splitting ?
Instead of downloading the entire app before users can use it, code splitting allows you to split your code into small `chunks` which you can then load on demand.

Code-splitting your app can help you “lazy-load” just the things that are currently needed by the user, which can dramatically improve the performance of your app. While you haven’t reduced the overall amount of code in your app, you’ve avoided loading code that the user may never need, and reduced the amount of code needed during the initial load.

### import()
import() is the concept of code-splitting, we can use it dynamically.

> *Before:*
```javascript
import { add } from './math'

console.log(add(10, 20))
```

> *After:*
```javascript
import('./math').then(math => {
  console.log(math.add(10, 20))
})
```

### What is React.lazy() ?
lazy() is the concept of code splitting, and this function will render the dynamic `import` as a regular component.

> *Before:*
```javascript
import Card from './card'
```

> *After:*
```javascript
const Card = React.lazy(() => import('./card'));
```

`React.lazy()` takes a function which must call a dynamic `import()`. This must return a Promise which resolves to a module with a default export containing a React component.

The lazy component should then be rendered inside a `Suspense` component, which allows us to show some **fallback** content (such as a *loading indicator*) while we’re waiting for the lazy component to load.

### Few things to know about Suspense Component :
- `<Suspense>` is a first-party React component which is used to wrap other components that might make asynchronous requests. Any time a child component performs some action resulting in a loading state, such as a network request, a wrapping <Suspense> component can toggle its rendering to show a loading UI, like a `<Spinner />`.

```javascript
  <Suspense fallback={<Loading />}>
    <FetchApi />
  </Suspense>
```

### What is Error Boundary Component ?
Error Boundary component is the concept of catching the error just like an `try()` and `catch()` method. But it can't catch the error's like *event handler*, *async code*, *server side rendering*. Error Boundary component is used only during development and it should be disabled during production. With the help of it, we can trace the perfect error in console by the perfect path, line number, etc. 

It is default when we are working with `create-react-app`. If you are not using `create-react-app`, you need to specify the plugin in your `Bebal` configuration file. 

They are components used to wrap other components which may throw errors.
```javascript
<ErrorBoundary fallback={<ErrorMessage />}>
  <App />
</ErrorBoundary>
```

Suspense and ErrorBoundary component will work together. Your Suspense component is wrapped by ErrorBoundary component.

When working with Routers, you may write the suspense component as example given below :
```javascript
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  </Router>
);
```

Also cover other blog of [Webpack](/WebPack/README.md)
