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
let timeAsString = d.toJSON();

// Note that there is shorthand to get the current time as number
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

Depending on how URLs are made in the site, one can also obtain language from the URL. For apps and dynamically generated content I recommend to get it from the HTTP request

### From the HTTP request

```js
import Accept from "@hapi/accept";
import ISO6391 from "iso-639-1-plus";
import {langs, translate} from "./availableTranslations.js"; // example

// ... later, on request
request.lang = Accept.language(request.headers[`accept-language`], langs) || defaultLanguage;
request.freeLang = Accept.language(request.headers[`accept-language`]);
if (!ISO6391.getName(request.freeLang)) {
    request.freeLang = request.lang;
}
```

Don't forget the __or expression__ otherwise `request.lang` might be `undefined`,
the reason I use 2 different variables is to be able to use translate with a language I actually translated, but also display the date in another language or culture as well, for example translate in en but display the date in en-GB.

I also use ISO6391 library to make sure that freeLang is not garbage, which can make Date.toLocaleDateString throw.


## Stringify a date in a given language

### `.toLocaleString`

By default displays the date and the time, highly customizable

```js
const dateOptions = {timeStyle:`short`, dateStyle: `short`};
const timeString = date.toLocaleString(request.freeLang, dateOptions);
```

### `.toLocaleDateString`

A shorthand for `.toLocaleString` with options to only show the date.

```js
date.toLocaleString("en" , {
    year:  "2-digit" ,
    month: "2-digit" ,
    day:  "2-digit",
}) === date.toLocaleDateString("en"); // true
```

### `.toLocaleTimeString`


A shorthand for `.toLocaleString` with options to only show the time.

```js

date.toLocaleString("en" , {
    hour:  "2-digit" ,
    minute: "2-digit" ,
    second:  "2-digit",
}) === date.toLocaleTimeString("en"); // true
```

### All options

The first interface is a high level overview, and the second is more detailed. They cannot be mixed when in conflict (dateStyle and weekday for example).

To not display seconds, use timeStyle short.

To force something to not be displayed (for example the year in a date), use the detailed options instead and omit the part that should be hidden.



```ts
interface DateTimeFormatOptions {
    formatMatcher?: "basic" | "best fit" | "best fit" | undefined;
    dateStyle?: "full" | "long" | "medium" | "short" | undefined;
    timeStyle?: "full" | "long" | "medium" | "short" | undefined;
    dayPeriod?: "narrow" | "short" | "long" | undefined;
    fractionalSecondDigits?: 0 | 1 | 2 | 3 | undefined;
}
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

To render the date inside html use the time element. Use the `.toJSON` method to make the date machine readable as well.

```js
const timeHTML = `
<time
    datetime="${date.toJSON()}"
>
    ${date.toLocaleString(request.freeLang, dateOptions)}
</time>`
```

## Time zones

This is something you cannot get from a simple HTTP request.

Before you get it you can use the `timeZoneName` option with `"long"` or `timeStyle` with `"long"` to clearly indicate the time zone the time is displayed in and use the most relevant time zone.

Once you know which time zone to use, for example by asking the user in his profile page, use the `timeZone` option inside `dateOptions`.

Or let the time be rendered on the client side, which will most likely have the wanted timezone and locale. To use the locale of the JS engine simply use `undefined` as first value. Make sure to have have a fallback in case JS is disabled.

## Recap example

```js
import http from "node:http";
import ISO6391 from "iso-639-1-plus";
import Accept from "@hapi/accept";


const defaultLang = `fr`;
const langs = [`fr`, `en`, `pt`];
const PORT = 8888;
    
const server = http.createServer((request, response) => {    
    let date;
    date = new Date();

    // macro options
    // const dateOptions = {
    //     timeStyle:`long`,
    //     dateStyle: `long`,
    // };

    // very detailed options
    const dateOptions = {
        // year: "long" // hidden because omitted
        // month: "long" // hidden because omitted
        weekday: `long`, // extra
        day: `2-digit`,
        hour:  "2-digit" ,
        minute: "2-digit" ,
    };

    const timeOptions = {
        timeZoneName: `short`,
        second: "numeric", // only display seconds and minutes
        minute: "2-digit",
    };

    // use for translations
    request.lang = Accept.language(request.headers[`accept-language`], langs) || defaultLang;
    request.freeLang = Accept.language(request.headers[`accept-language`]);
    if (!ISO6391.getName(request.freeLang)) {
        request.freeLang = request.lang;
    }
    
    if (request.method === `GET`) {
        response.end(`<!doctype html>
            <meta charset="utf-8">
        <h1>Time</h1>
        <p>toLocaleString <br>
            <time
                datetime="${date.toJSON()}"
            >
                ${date.toLocaleString(request.freeLang, dateOptions)}
            </time>
        </p>
        <p>toLocaleTimeString only display seconds and minutes<br>
            <time
                datetime="${date.toJSON()}"
            >
                ${date.toLocaleTimeString(request.freeLang, timeOptions)}
            </time>
        </p>
        <p>toLocaleTimeString only display seconds and minutes<br>
        with client side overwrite <br>
            <time
                datetime="${date.toJSON()}"
                data-time
            >
                ${date.toLocaleTimeString(request.freeLang, timeOptions)}
            </time>
            <script type="module">
            // add data-time where this should happen
            document.querySelectorAll("time[data-time]").forEach(timeElement => {
                const dateAsString = timeElement.getAttribute("datetime");
                const date = new Date(dateAsString);
                const localDate = date.toLocaleTimeString(undefined, ${JSON.stringify(timeOptions)})
            });
            </script>
        </p>
        <p>toLocaleDateString <br>
            <time
                datetime="${date.toJSON()}"
            >
                ${date.toLocaleDateString(request.freeLang/*, use default options*/)}
            </time>
        </p>`);
        return;
    }
    
    response.setHeader(`Content-Type`, `text/plain; charset=utf-8`);
    const error = `Method Not Allowed`;
    response.writeHead(415);
    response.end(error);
});


server.listen(PORT, () => {
    console.log(`listenting on ${PORT}`);
});
```

If you test it locally, there will most likely not be any difference between the client side and the server side `toLocaleTimeString`.

## Performance

```js
const formatReady = new Intl.DateTimeFormat(lang, options); // can be reused
const stringDate = formatReady.format(date)
```

Preparing a formatReady variable like this might be more performant than using date.toLocaleString with the same lang and options multiple times. How much faster ?

## Relative time

There is a

```js
Intl.RelativeTimeFormat
```

Not used yet.

## Edits

 * Thanks Special-Tie-3024 for giving the idea to render the date on the client side
 * prefer toJSON over toISOString (they do the same)