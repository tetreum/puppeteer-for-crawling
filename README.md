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
  * [elementSelector.prop(prop)](#elementselectorprop)

- [class: Page](#class-page)
  * [page.q(selector)](#pageselector)
  * [page.exists(selector)](#pageselector)
  * [page.getElementsAttribute((selector, attribute)](#pageselector)
  * [page.fill(selector, fields)](#pageselector)

- [class: ElementHandle](#class-page)
  * [elementHandle.isVisible()](#elementhandleisvisible)
  * [elementHandle.attr(name, val)](#pageselector)
  * [elementHandle.text()](#pageselector)
  * [elementHandle.prop(prop)](#pageselector)


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
- returns: <[bool]>

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

# You may also want

For fast debugging/printing:
https://github.com/tetreum/perfect-print-js
