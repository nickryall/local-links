local-links
===========

Determine cross-browser if an event or anchor element should be handled locally.

[![NPM](https://nodei.co/npm/local-links.png)](https://nodei.co/npm/local-links/)

[![browser support](https://ci.testling.com/lukekarrys/local-links.png)
](https://ci.testling.com/lukekarrys/local-links)

## Install

`npm install local-links --save`

## Why?

Browsers have quirks. Knowing if a link is local should be easy, since we
just want to know if the hosts are the same. But this can be difficult because
of the aforementioned browser quirks. A few of them:

- IE9 will add `:80` to the host of an anchor, but not the window
- IE9 wont put a leading slash on the pathname of an anchor, but will on the window
- Chrome 36 will report anchor.hash as '' if it has `href="#"`
- More? Please report test cases!

Because of that and a few other things I was doing all the time, such as
finding the closest anchor to an element based on an event object, I decided
it would be a good module (that at least I would use all the time).

## Usage

```html
<a href='/page2' id="local">Local</a>
<a href='#hash' id="hash">Local</a>
<a href='http://google.com' id="google">Google</a>
```

```js
var local = require('local-links');

// `pathname()` will return the pathname as a string
// if the link is local, otherwise it will return null
local.pathname(document.getElementById('local')) // '/page2'
local.pathname(document.getElementById('hash')) // null
local.pathname(document.getElementById('googe')) // null

// `hash()` will return the hash as a string
// if the hash is to this page, otherwise it will return null
local.hash(document.getElementById('local')) // null
local.hash(document.getElementById('hash')) // '#hash'
local.hash(document.getElementById('googe')) // null
```


## API


### Methods

- `pathname(Event or HTMLElement [, HTMLElement])`

Returns the pathname if it is a non-hash local link, or null if it is not.
Always includes the leading `/`.

- `hash(Event or HTMLElement, [, HTMLElement])`

Returns the hash if it is an in-page hash link, or null if it is not. Always
includes the leading `#`.


#### Supply either Event or HTMLElement (or both!)

Both the above methods will accept an `Event` object, like the one you get from
event handlers, or any `HTMLElement`. You can also supply an `Event` object
and your own `HTMLElement` as the second parameter and it will take precedence.

If only an `Event` object is supplied, the `HTMLElement` will be found from
`Event.target`.


#### Nested HTML Elements

In the case that any `HTMLElement` your provide is not an anchor
element, the module will look up `parentNodes` until an anchor is found.


#### Events

If an `Event` object is supplied, all methods will return `null` if any of the following
are true `altKey`, `ctrlKey`, `metaKey`, `shiftKey. This is because you almost always
want to treat modified click events as external page clicks.


#### Hash links

Using the `pathname` method will return null for hash links that do not point
to a different page. To get the hash for one of these links use the `hash()` method.


### Tests

Run `npm start` and open [`http://localhost:3000`](http://localhost:3000) to run the tests in your browser.

To run the tests in the cli, just run `npm test`.


### License

MIT
