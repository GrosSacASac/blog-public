## With node >=18.0.0 or deno

Use an import with `assert` inside the import

### app.js

```js
import object from "./x.json" assert { type: "json" };
console.log(object);
```

### x.json

```json
{
    "key": "value"
}
```

`node app.js`

or with deno

`deno run app.js`

## With node >=17.3.0

Use the cli flag `--experimental-json-modules`.

`node --experimental-json-modules app.js`

## With older version than 17.3.0

Another way is to use a top level await and open the file as text and use `JSON.parse`

### app.js


```js
import fsPromises from "node:fs/promises";

const jsonString = await fsPromises.readFile(`./x.json`, `utf-8`);
const object  = JSON.parse(jsonString);
    
console.log(object);
```

Note that the file path is resolved with the curent working directory here instead of relative to the js file.

## How to import a text file

Maybe in the future there will be assert text. But for, do the same as JSON without the parsing:


```js
import fsPromises from "node:fs/promises";

const text = await fsPromises.readFile(`./example.txt`, `utf-8`);
    
console.log(text);
```
