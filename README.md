# Puppeteer for crawling

Adds methods like `attr`, `html`, `getElementsAttribute`, etc.. that puppeteer misses by default.

Example:

- Puppeteer:
```js
let description = await page.evaluate( () => {
    return document.querySelector('[itemprop="about"]').innerText
})
```

- Puppeteer + puppeteer-for-crawling:
```js
let description = await page.q('[itemprop="about"]').text();
```

# Install

`npm install puppeteer-for-crawling`

# Usage

```js
        const puppeteer = require('puppeteer');

        require('puppeteer-for-crawling')

        const browser = await puppeteer.launch({
            args: [
                '--no-sandbox',
                '--disable-setuid-sandbox',
            ]
        });
        const page = await browser.newPage();

        await page.goto('https://github.com/tetreum/puppeteer-for-crawling')

        // you can use q for faster coding
        let description = await page.q('[itemprop="about"]').text();
        let readme = await page.q('#readme').html();
        let id = await page.q('meta[name="octolytics-dimension-repository_id"]').attr("content");
        let inputNames = await page.getElementsAttribute('input', "name");

        console.log("Q method:", id, description, inputNames, readme)

        // or keep using puppeteer's selector ($) and call the new methods
        let description = (await page.$('[itemprop="about"]')).text();
        let readme = (await page.$('#readme')).html();

        console.log("$ method:", id, description)

        await page.goto('https://github.com/login')

        if (await page.q('#login form').isVisible()) {
            await page.fill('#login form', {
                'login': "test",
                'password': "test_password"
            });
        }


```

# Added methods

- [class: ElementSelector](#class-elementselector)
  * [elementSelector.isVisible()](#elementselectorisvisible)
  * [elementSelector.attr(name, val)](#elementselectorattr)
  * [elementSelector.text()](#elementselectortext)
  * [elementSelector.prop(prop)](#elementselectorpropname)

- [class: Page](#class-page)
  * [page.q(selector)](#pageqselector)
  * [page.exists(selector)](#pageexistsselector)
  * [page.getElementsAttribute((selector, attribute)](#pagegetelementsattributeselector-attribute)
  * [page.fill(selector, fields)](#pagefillselector-fields)

- [class: ElementHandle](#class-elementhandle)
  * [elementHandle.isVisible()](#elementhandleisvisible)
  * [elementHandle.attr(name, val)](#elementhandleattrname-val)
  * [elementHandle.text()](#elementhandletext)
  * [elementHandle.prop(prop)](#elementhandlepropname)


# Documentation

### class: ElementSelector

New class introduced by this package. ElementSelector can be created with the [page.q](#pageselector) method.

Unlike `page.$`, calling `page.q` alone won't perform any evaluate action/element won't get requested. It is made to request element parts like, attributes, or inner content, rather than the element itself.
It will make the crawling experience faster/easier to maintain:
```js
let $el = await (await page.$('title')).text()
let $el2 = await page.q('title').text()
console.log($el, $el2)
```

Once you call a method (like `text`, `attr`, etc..) over q, the `elementHandle` inside will be cached so it won't be requested everytime you call another method over it

#### elementSelector.isVisible()
- returns: <[boolean]>

Checks if selector is visible.

#### elementSelector.attr(name, val)
- `name` <[string]> Attribute's name
- `val` <[mixed]> Optional attribute's value to set
- returns: <[null]>

Gets/Sets requested attribute

#### elementSelector.text()
- returns: <[string]>

Returns element's innerText

#### elementSelector.prop(name)
- `name` <[string]> Property name
- returns: <[string]>

Returns element's property name

### class: Page

From puppeteer.

#### page.q(selector)
- `selector` <[string]> A [selector] to query frame for
- returns: <[ElementSelector]>

The method prepares to queries frame for the selector.

#### page.exists(selector)
- `selector` <[string]> A [selector] to query frame for
- returns: <[boolean]>

The method checks if given selector exists.

#### page.getElementsAttribute(selector, attribute)
- `selector` <[string]> A [selector] to query frame for
- `attribute` <[string]> Attribute's name to get from selector
- returns: <[Array]<[String]>>

Gets attribute values from from the selector

#### page.fill(selector, fields)
- `selector` <[string]> A [selector] to query frame for the form
- `fields` <[Object]> Key (field name)<->Value object of fields to set
- returns: <[Array]<[String]>>

Gets attribute values from from the selector

### class: ElementHandle

From puppeteer.

#### elementHandle.isVisible()
- returns: <[boolean]>

Checks if selector is visible.

#### elementHandle.attr(name, val)
- `name` <[string]> Attribute's name
- `val` <[mixed]> Optional attribute's value to set
- returns: <[null]>

Gets/Sets requested attribute

#### elementHandle.text()
- returns: <[string]>

Returns element's innerText

#### elementHandle.prop(name)
- `name` <[string]> Property name
- returns: <[string]>

Returns element's property name

[Array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array "Array"
[Object]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object "Object"
[string]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#String_type "String"
[boolean]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Boolean_type "Boolean"
[Page]: #class-page "Page"
[ElementSelector]: #class-elementselector "ElementSelector"
[selector]: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors "selector"
[null]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Null_type "Null"


# You may also want

For fast debugging/printing:
https://github.com/tetreum/perfect-print-js
