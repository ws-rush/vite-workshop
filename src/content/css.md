---
title: Working with CSS
---

# Working with CSS

Writing and bundling JavaScript is only part of the battle. A lot of times we tend to need to style our applications as well. To no one's surprise, that's pretty straight forward as well.

## Adding CSS

Vite gives a few different ways to add CSS.

1. We can add `<link>` tags as we've done for years.
2. We can import the CSS files from our JavaScript.
3. We can use CSS Modules.

### Using a `<link>` tag

Let's start with the most boring, but straight forward of the bunch. Add the following to your HTML.

```html
<link rel="stylesheet" href="/src/style.css" />
```

### Importing a Stylesheet

In `counter.js`, we can import a stylesheet.

```js
import './counter.css';
```

In both cases, the CSS is loaded globally. The notable difference here is that this CSS file will only be loaded when this module is loaded.

In production mode global css will bundled as one file imported externally, in dev mode every css file will injected as `<style>` tag in html.

## Using CSS Modules

If you look closely, you'll notice that this CSS is _not_ rendering.

```css
.count {
	font-size: 4em;
	color: rebeccapurple;
}
```

This makes sense, because because we don't have anything with that class.

If we give a CSS file a `*.module.css` extension, then we can access its fingerprinted classes.

```js
import styles from './counter.module.css';

// …

countElement.classList.add(styles.count);
```

This is the resulting class name.

```html
<p id="count" class="_count_1o9rn_1">4</p>
```

<div class="exercise">

### Exercise: Add Banner Styles

Can you use `banner.module.css` as a CSS module?

<details><summary>Solution</summary>

```js
import styles from './banner.module.css';

// …

banner.classList.add(styles.banner);
closeButton.classList.add(styles.button);
```

</details>

Notice how the CSS file is also dynamically added to the DOM as needed.

</div>

**The moral of the story**: Dealing with asynchronous module loading is definitely less easy than just writing some `import` statements at the top of the file, but it also gives you the ability to slim down the size of your bundle and load _both_ CSS and JavaScript on an as-needed basis.

### Typed CSS Modules

If you're using TypeScript and CSS modules, you may want type support for your CSS classes. You can use a package like `tcm`, `typed-css-modules` or scripts that generate `.d.ts` files for your CSS modules.

Here's how you might use `typed-css-modules`:

```
npm install -D typed-css-modules
```

Generate types:

```
tcm src -w
```

Include this in your `build` script to ensure the CSS types are always up-to-date.

After all these steps, your `package.json` scripts section may look something like:

```json
{
	"scripts": {
		"build": "vite build && tsc --declaration --emitDeclarationOnly --outDir dist/types && tcm src -w"
	}
}
```

## Tasting Notes

Here are some additional notes on working with CSS that are a little esoteric. I'll leave these here for you to peruse if you're interested.

### Disabling CSS injection into the page

The automatic injection of CSS contents can be turned off via the ?inline query parameter. In this case, the processed CSS string is returned as the module's default export as usual, but the styles aren't injected to the page.

```js
import './foo.css'; // will be injected into the page
import otherStyles from './bar.css?inline'; // will not be injected
```

> CSS support the usage of ?url to obtain only the file path.

### `@import` Handling

- Support for CSS `@import` inlining via `postcss-import`
- Vite aliases are respected for `@import`
- `url()` references are rebased automatically, ensuring correctness across different directories

### Lightning CSS

- Available starting Vite 4.4
- Enabled by adding `css.transformer: 'lightningcss'` to config
- Requires installation of `lightningcss` dependency

#### Configuration

- `css.lightningcss` config option
- `css.lightningcss.cssModules` for CSS Modules
- `build.cssMinify: 'lightningcss'` for minification

#### Limitations

- Doesn't support CSS Pre-processors
