## The Date object

### Creation

There are 3 main ways to create a date object

1. With no argument, it will be the current time
2. With a string representing the date
3. With a number in ms elapsed since the epoch

```js
let date;
date = new Date(); // 1
date = new Date('2022-02-22T09:53:11.600Z'); // 2
date = new Date(); // 3
date.setTime(2000000000000); // 3
// (2000000000000 corresponds to 2033)
```

### Storage

A date can be stored with a number or a string

```js
let timeAsNumber = d.getTime();
let timeAsString = d.toISOString();

// Note there is shorthand to get the current time as number
let timeAsNumber = Date.now();
// is equivalent to
let date = new Date();
let timeAsNumber = d.getTime();
```

A like to store dates as numbers in my database for the following reasons:

 * A number takes around 4 bytes of storage, where a string takes around 20 bytes
 * Numbers are easier to sort than strings representing dates
 * Better performance when creating a date object from a number

To display dates, the first requirement is to know in what language to display it.

## Get language of the user

### From the URL

Depending on how URL are made in the site, one can also obtain language from the URL. For apps and dynamically generated content I recommend to get it from the HTTP request

### From the HTTP request

## Stringify a date in a given language

### `.totoLocaleDateString`

### 
### All options

```ts
    interface DateTimeFormatOptions {
        localeMatcher?: "best fit" | "lookup" | undefined;
        weekday?: "long" | "short" | "narrow" | undefined;
        era?: "long" | "short" | "narrow" | undefined;
        year?: "numeric" | "2-digit" | undefined;
        month?: "numeric" | "2-digit" | "long" | "short" | "narrow" | undefined;
        day?: "numeric" | "2-digit" | undefined;
        hour?: "numeric" | "2-digit" | undefined;
        minute?: "numeric" | "2-digit" | undefined;
        second?: "numeric" | "2-digit" | undefined;
        timeZoneName?: "long" | "short" | undefined;
        formatMatcher?: "best fit" | "basic" | undefined;
        hour12?: boolean | undefined;
        timeZone?: string | undefined;
    }
```
## Inside HTML
## Time zones
