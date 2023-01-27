# How to Enable CSP ?
To enable CSP, you need to do configuration in your web server to return CSP.

> *Refer this [official documentation of CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) in MDN.*

### Syntax:
```javascript
Content-Security-Policy: <policy-directive>; <policy-directive>
```
where `<policy-directive>` consists of: <directive> and <value>.
  
You can add it inside your `html` page:
```javascript
<meta http-equiv="Content-Security-Policy" content="<directive> <value>;<directive> <value>"
```
      
When you specifies the value of directives in meta tag, only that content will be allowed, if any other website link is used than CSP will block that site.

> *Refer this [video](https://www.youtube.com/watch?v=txHc4zk6w3s) for proper explaination about why there is need of CSP ?*

## What is Directives in CSP ?
Content Security Policy is a directive policy that lets the authors (or server administrators) of a web application restrict from where the application **can load resources**.

There are many types of directive:
  1) Fetch
  2) Document
  3) Navigation
  4) Reporting
  5) Deprecated, etc.
  
### 1) Fetch directive :
Fetch directives control the locations from which certain resource types may be loaded.

`child-src`: It is used when the browser context loads the elements by using `<iframe>` and `<frame>`. When you are working with browser nested context, use `frame-src` and `worker-src` instead of `child-src` respectively.
  
`default-src`: When there is no directives present, then the user agent will search for `default-src`. Its just like in case of **switch-case** `default`.
  
`font-src`: If you want to only load those fonts which your web application uses and block other font sites, then `font-src` is useful feature.
