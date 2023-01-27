# How to Enable CSP ?
To enable CSP, you need to do configuration in your web server to return CSP.

> *Refer this [official documentation of CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy) in MDN.*

### Syntax:
```javascript
Content-Security-Policy: <policy-directive>; <policy-directive>
```
where `<policy-directive>` consists of: <directive> and <value>.

## What is Declaratives in CSP ?
Content Security Policy is a declarative policy that lets the authors (or server administrators) of a web application restrict from where the application **can load resources**.

There are many types of declaratives:
  1) Fetch
  2) Document
  3) Navigation
  4) Reporting
  5) Deprecated, etc.
  
### 1) Fetch Declaratives :
Fetch directives control the locations from which certain resource types may be loaded.

`child-src`: It is used when the browser context loads the elements by using `<iframe>` and `<frame>`. When you are working with browser nested context, use `frame-src` and `worker-src` instead of `child-src` respectively.
