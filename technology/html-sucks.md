## Inconsistencies

### Boolean attributes

Here's how to use spellcheck attribute

```html
<textarea spellcheck="true"></textarea>
```

```html
<textarea spellcheck="false"></textarea>
```

Now guess how to use the Boolean `required` attribute.

```html
<textarea required="required"></textarea>
or use shorthand
<textarea required></textarea>
```

Just omit it entirely for not required:

```html
<textarea></textarea>
```

Using `required="false"` will make it required ...

Wait, there is another ... now guess how to use the autocomplete attribute:

```html
<label>User
    <input name="user" autocomplete="on">
</label>
```

```html
<label>User
    <input name="user" autocomplete="off">
</label>
```

Ouch. See the inconsistency ?

To be fair, `autocomplete` is not really Boolean as it can take other values such as `email` `tel` etc.

## Loading stuff and in-lining stuff

How to use an inline script

```html
<script>
/* Code here */
</script>
```

Alright, what about loading it from outside :


```html
<script src="./myjs.js"></script>
```

So far so good. What about stylesheets ?


```html
<style>
    .block{display: block; margin-bottom: 0.5em;}
</style>
```

And to load it from an external source:

```html
<link href="./mycss.css" rel="stylesheet">
```

That's a lot of damage. `link` but not `style`, `href` but not `src`.

### Loading HTML

[Issue in github.com/whatwg/html](https://github.com/whatwg/html/issues/2791)

There is no built-in way to load external HTML.

All of the top 10 most used JavaScript frameworks support loading external HTML. And I'm pretty sure it even goes to the top 50. It is not a coincidence. It is one of the most wanted feature. So then comes the question: If it is one of the most wanted feature, why is there still no built-in way to do it ?

Well there were several attempts to add such a feature to the web platform. And they all failed.

[Can I use HTML Imports ?](https://caniuse.com/imports) The fact that it requires JavaScript makes me think that the champions had a lack of vision. If you need JavaScript you might as well just use one JavaScript framework that let's you import HTML with 1 line of code. Which make it useless.

[HTML modules](https://github.com/WICG/webcomponents/blob/gh-pages/proposals/html-modules-explainer.md) Not really failed but needs implementers interests. And by implementers, they mean the big 5 browsers vendors. But they are too busy adding web payment methods. Also not enough people are asking for the feature because most web developers nowadays use a JavaScript framework which makes this lack of feature invisible. So a chicken and egg problem.

Something that also slows this: There are people who argue that you don't actually need client side HTML includes, since you can do it on the server side. And while that is true, it does not considers the fact that thousands of web developers do it anyway using a 5-100KB JavaScript framework in the process.

Also with client side includes, if combined with cache and HTTP/2 it may be more optimized than server side includes.

[Can I use iframe seamless ?](https://caniuse.com/iframe-seamless) Nope. I don't know exactly why it was dropped, but in general `<iframe>` opened a lot of security issues in the past.

## The DOM

The document object model is the way to programmatically expose the HTML document to programming languages. And the DOM API are ugly and badly designed. I believe this is the number 1 reason people hate JavaScript, even though JavaScript as a programming language can be used without the DOM and the DOM can also be accessed without JavaScript (Java, Flash, Bighton, WASM).

## Historic artefacts

You have to use in every page for historic reasons

```html
<meta name="viewport" content="width=device-width">
```

## Forms

### Missing methods

A `<form>` method can only be `GET` and `POST`. The number of people who know why is probably equal to the number of editors of the spec and they live in a cave (joke).

Imagine me circa 2015 learning REST and discovering that I have to redesign all my `DELETE` routes to also work with `POST`...

Ideally I would use the HTTP methods GET, POST, PUT, DELETE, etc.. But now I need to make a choice. Make the form submit with JavaScript with the correct method, but now my site does not work without JS, or put the method name somewhere in the URL and make my server code ugly because I need to handle POST and read the URL to see the real method, and still handle direct DELETE request for server to server communications. Or put the method in a hidden input. And yes the server needs to parse and validate and use its value.

### Confusing appearance and controls

What is difference between a `<select>` and a group of  `<input type="radio">` ? It is the visual appearance. But they do the same. Which mean it should have been a CSS appearance instead.

### Hidden input or input hidden ?

Do you know the difference between : hidden, readonly, disabled and type="hidden"

```html
<input type="hidden" name="a" value="a">
```

Visually hidden, sends value with form. Only usable with tag = input

```html
<input hidden name="a" value="a">
```

Same as above, but prefer the above version except for other inputs tags like select.

```html
<input disabled name="a" value="a">
```

Visually greyed out ,cannot be changed, does not sends value with form

```html
<input readonly name="a" value="a">
```

Cannot be changed, but visually clear and copiable, sends value with form

```html
<input value="a">
```

Normal input but nothing will be sent, because it lacks the name attribute

What about the inert attribute ? It is relatively new so I did not try yet , but here is what is written on MDN:

> The HTMLElement property inert is a Boolean value that, when present, makes the browser "ignore" user input events for the element

## No binary version

No binary html version means it misses out on some compression.

Opera mini uses a version of binary html, that is sent by opera mini servers to reduce bandwidth.

No binary means that inlining binary data like images inside html requires base64 encoding which takes extra data than the binary version

## Conclusion

Write in the comments if you have more.

HTML is still the best we have for many reasons : It is both human and machine readable, it is extensible with JavaScript and CSS, it is accessible with screen readers and more, it is lightweight, viewable on any modern device on the planet, and open standard which continues to evolve.
