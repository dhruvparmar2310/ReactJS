# Code Spliting | Webpack

Code splitting can be done in three ways :
  - Multiple entry points
  - Prevent Duplication by runtimechunk for single page and SliptChunksPlugin to remove duplication chunks in `/dist` folder.
  - Dynamic `import`

## 1) Multiple Entry points :
We can have multiple entry points in our project. To define a multiple entry points in `webpack.config.js` file, write the below code:
```javascript
entry: {
  index: './src/index.js',
  another: './src/another-module.js',
}
```

If there are any duplicated modules between entry chunks they will be included in both bundles. As lodash is also imported within `./src/index.js` and will thus be duplicated in both bundles. To seperate that duplication from both the bundles, **prevent duplication** comes in picture.

## Prevent Duplication :
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

## SplitChunksPlugin :
The SplitChunksPlugin allows us to extract common dependencies into an existing entry chunk or an entirely new chunk. Now change the previous code of `webpack.config.js` file to below code:

```javascript
optimization: {
     splitChunks: {
       chunks: 'all',
     },
   },
```
Now you can see the change in `index.bundle.js` and `another.bundle.js` file, the `lodash` duplicate dependency is removed. Even the `runtime.bundle.js` file is remove and that file is included under the `shared.bundle.js` file.

## Prefetching and Preloading Modules :

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

## Bundle Analyzer :

Once you start splitting your code, bundle analyzer is useful. There are many several bundle analyzers :
  - webpack-chart: it will display in pie chart.
  - webpack-visualizer: we can visualize and analyse, it is similar to pie chart.
  - webpack-bundle-analyzer: it is mostly used to analyze the size of your bundle.
  - webpack bundle optimizer helper: it also provides the suggestion to improve and reduce your bundle size.
  - bundle-stats: it will generate bundle report and compare its result with different builds.

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

