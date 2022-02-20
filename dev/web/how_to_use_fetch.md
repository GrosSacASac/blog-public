# How to use fetch

## With node 17.5.0

Use the cli flag `--experimental-fetch` to have it as a global.

### app.js

```js

fetch(`https://nodejs.org/en/blog/`).then(function (response) {
    if (!response.ok) {
        throw response.statusText;
    }
    return response.text();
})
.then(console.log)
.catch(console.error);
```

`node --experimental-fetch app.js`

## With older version than 17.5.0

Install node-fetch

`npm i node-fetch`

### patch-fetch.js

```js
import fetch from "node-fetch";


globalThis.fetch = fetch;
```
Then import this file in your main entry point

### app.js

```js
import "./patch-fetch.js";

// fetch is available
// ... rest of the app
```
## Do not polyfill if fetch is already existing


### patch-fetch.js

```js
import fetch from "node-fetch";

if (!globalThis.fetch) {
    globalThis.fetch = fetch;
}
```


## Do not polyfill and do not import if fetch is already existing

In the following example the node-fetch code is not even executed if fetch is defined.

### patch-fetch.js

```js
if (!globalThis.fetch) {
    globalThis.fetch = (await import(`node-fetch`)).default;
}
```

## With deno

fetch is available by default in deno