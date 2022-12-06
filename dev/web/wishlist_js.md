# My wish-list for JS

Draft

## Spread operator to grab all non-listed items

```js
const [...rest, last] = array;
```

Taking the rest at the end already works:

```js
const [first, second, ...rest] = array;
```

### Alternative before it exists

```js
const rest = array.slice(0, array.length - 1);
const last = array[array.length - 1];
```
