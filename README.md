# Puppeteer for crawling

Adds methods like `attr`, `html`, `getElementsAttribute`, etc.. that puppeteer misses by default

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

        let description = await page.q('[itemprop="about"]').text();
        let readme = await page.q('#readme').html();
        let id = await page.q('#repository_id').attr("value");
        let inputNames = await page.getElementsAttribute('input', "name");

```

# Added methods

- [class: ElementSelector](#class-elementselector)
  * [elementHandle.isVisible()](#elementhandleisvisible)
  * [elementHandle.attr(name, val)](#pageselector)
  * [elementHandle.text()](#pageselector)
  * [elementHandle.prop(prop)](#pageselector)

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

# You may also want

For fast debugging/printing:
https://github.com/tetreum/perfect-print-js
