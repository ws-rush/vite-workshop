---
title: Glob Import
---

# Glob Import

Vite includes support for importing multiple modules at the same time `import.meta.glob`. Here are some ways that I tend to use this feature:

- Let's say hypothetically, I'm building a website for a course and I want to get an array of all of the Markdown files in a directory to make an index. Hypothetically, of course.
- More practically: Let's say that I have a directory of a bunch of fixture data of API responses and I want to run the same tests over all of them.

## What are we working with?

Let's start small.

```js
export default function logos() {
	const modules = console.log(import.meta.glob('./logos/**/*.svg'));
}
```

We should see something where the keys are the file names and the values are promises that are just import statements.

This means, we could do something like this:

```js
const content = document.querySelector('#content');

export default function logos() {
	// modules is object of dynamic imports
	const modules = import.meta.glob('./logos/**/*.svg');

	for (const m of Object.values(modules)) {
		// the dynamic import containe exports like { default: '3', convert: () => {} }
		m().then((svg) => {
			const img = document.createElement('img');
			img.width = 100;

			img.src = svg.default;
			content.appendChild(img);
		});
	}
}
```

<div class="exercise">

## Exercise

In the `vite-starter` repository, there a function that takes one of those character JSONs and renders it to a string. Can you glob import all of the JSON files that I left in there for you and use `vitest` to render them to the strings?

- You can find the example setup in `src/characters/render-character.test.js`.

</div>

## Tasting Notes

### Negative Patterns

- Prefixed with `!` to exclude files
- Transformed code when using negative patterns

### Lazy Loading and Chunks

- Default lazy-loaded via dynamic imports
- Split into separate chunks during build process

### Eager Importing

- Usage of `{ eager: true }` as the second argument
- Transformed code when using eager loading

```js
const content = document.querySelector('#content');

export default function logos() {
	// modules is object of exported imports { default: '3', convert: () => {} }
	const modules = import.meta.glob('./logos/**/*.svg', { eager: true });

	for (const m of Object.values(modules)) {
		const img = document.createElement('img');
		img.width = 100;

		img.src = m.default;
		content.appendChild(img);
	}
}
```

### Importing as Strings

- Utilizing Import Reflection syntax with `{ as: 'raw', eager: true }`, `{ 'src1': 'console.log("pure code")', ... }`
- Transformed code and its behavior
- Support for `{ as: 'url' }` for loading assets as URLs `{ 'src1': 'it's url', ... }`
- without `{ eager: true }` we get same result in `.then(url)`

### Named Imports

- Using the `import` options to selectively import parts of the modules

```js
const content = document.querySelector('#content');

export default function logos() {
	// modules is object of dynamic imports
	const modules = import.meta.glob('./logos/**/*.svg', { import: 'default' });

	for (const m of Object.values(modules)) {
		// the dynamic import containe the only named export
		m().then((svg) => {
			const img = document.createElement('img');
			img.width = 100;

			img.src = svg;
			content.appendChild(img);
		});
	}
}
```

- Tree-shaking support when combined with `eager`

### Custom Queries

- Using `query` option to provide custom queries
- How other plugins can consume these queries

## Caveats and Limitations

- Vite-only feature, not a web or ES standard
- Rules for glob patterns (relative, absolute, or alias paths)
- Reliance on fast-glob for matching
- Restrictions on literals; variables or expressions not allowed

## Solution

I hid the link to [a potential solution](./glob-import-solution.md) down here hoping that you'd never find it.
