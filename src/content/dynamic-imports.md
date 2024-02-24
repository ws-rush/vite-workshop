---
title: Dynamic and Conditional Imports
---

# Dynamic and Conditional Imports

## Dynamic Imports

In Baisc Setup example, even through we've broken our code up between two modules, Rollup is smart enought to figure out that they're all loaded at the same time and inlines them.

If we used a `import()` to load our file dynamically, we'll see that it's smart enough to split up our code.

```js
import('./counter.js').then(({ initializeCounter }) => {
	initializeCounter();
});
```

We'll now see that we have two assets.

```
dist
├── …
├── assets
│   ├── counter-7777d3c9.js
│   └── index-1060a589.js
└── …
```

**Extension**: I wrote a little piece on adding [Hot Module Replacement](./hot-module-replacement.md) to this example. But, let's be honest if you're not writing your own framework, you're probably _not_ doing this yourself.

### Exercise: Dynamic Loading

This is a little bit contrived, but we're going to work with what we have. If the count goes negative, we want to show a banner.

We'll probably start with something like this:

```js
const render = () => {
	countElement.textContent = count;

	if (count < 0) {
		// Your code here.
	}
};
```

Some **tasting notes**:

- Try out using an regular import and a dynamic import.
- You can place our little note into the `#content` element.
- Don't worry about removing or dismissing the banner. (This is a workshop on a build tool, not DOM manipulation. But, like, feel free to remove it if you want, I guess.)
- Don't worry about styling.

Your code could be as simple as something like this:

```js
export const addBanner = (text) => {
	document.querySelector('#content').textContent = text;
};
```

<details><summary>Solution</summary>

A quick and easy way to add a banner:

```js
const render = () => {
	countElement.textContent = count;

	if (count < 0) {
		import('./add-banner.js').then(({ addBanner }) => {
			addBanner('The counter is negative!');
		});
	}
};
```

</details>

## Conditional Imports

Conditional imports allow you to dynamically load different modules based on certain conditions, such as the current environment (development, production, etc.), the platform (browser, Node.js), or even custom conditions that you define.

Vite leverages JavaScript's dynamic `import()` syntax and some additional configuration options to make conditional imports possible.

### Basic Conditional Import Example

Here's a simple example of using a dynamic import based on a condition:

```ts
let module;

if (process.env.NODE_ENV === 'development') {
	module = import('./dev-module.js');
} else {
	module = import('./prod-module.js');
}

module.then(function (m) {
	// Do something with the loaded module
});
```

In this example, the module that gets loaded depends on the current environment.

### Environment-Based Conditional Imports

Vite exposes certain environment variables that can be used for conditional imports. For example, you can use `import.meta.env.MODE` to determine the current mode (`'development'`, `'production'`, etc.).

```ts
if (import.meta.env.MODE === 'development') {
	// Import development-only module
}
```

You can also define your own environment variables in a `.env` file and use them for conditional imports.

### Vite-Specific Conditional Import Features

Vite offers more advanced features like the `define` option, which lets you replace global variables at compile time:

```ts
export default {
	define: {
		__MY_CONDITION__: 'some value'
	}
};
```

Then, you can use this in your code to conditionally import modules:

```ts
if (__MY_CONDITION__ === 'some value') {
	// Conditionally import something
}
```

### Using Dynamic Import with `vite-plugin-dynamic-import`

While Vite doesn't offer built-in direct support for conditional imports in `import` statements, there's a third-party plugin called [`vite-plugin-dynamic-import`](https://www.npmjs.com/package/vite-plugin-dynamic-import) that provides such a feature.

```
npm install vite-plugin-dynamic-import
```

Then, add it to your `vite.config.js`:

```js
import dynamicImport from 'vite-plugin-dynamic-import';

export default {
	plugins: [dynamicImport()]
};
```

This plugin allows you to write conditional imports directly in the `import` statements based on conditions you define.

```js
import(`./content/${variable}.js`);
```
