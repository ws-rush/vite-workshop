---
title: Basic Setup
---

# Basic Setup

I made a little repository that has some pre-baked goodies for us to play around with. You don't _need_ anything in this repository and I'll show you how to generate a Vite project quickly and easily, but this repository has some fun stuff like images and little code snippets that we can use as needed.

So, go ahead and clone [`stevekinney/vite-starter`](https://github.com/stevekinney/vite-starter) or make a copy using `npx degit stevekinney/vite-starter vite-starter`.

Oh, and don't forget to **install the dependencies**.

## Starting Up the Server

One of the things you'll notice about this repository and a lot of [the Vite templates](https://github.com/vitejs/vite/tree/main/packages/create-vite) is that there is an `index.html` in the root of the project.

This is the entry point for Vite.

We can get the application started by running either of the following: `npm start` or `npx vite`.

You'll also see that a lot of projects use `npm run dev` and this will work with our starter as well.

Regardless of how you choose to run it, Vite will start up a development server with a simple webpage.

## Pulling in Some JavaScript

At first, adding a `<script>` tag doesn't seem very exciting, but this is how Vite determines the initial entry point for our application.

In `src/index.js`, let's add the following:

```js
console.log('Hello World!');

document.querySelector('h1').textContent = 'Hello World!';
```

In `index.html`, we'll add a reference to this code:

```html
<script src="/src/index.js"></script>
```

This will give us an error if we put it in the `<head>`, but we're going to ignore that error for a minute.

## Importing Files

In `src/counter.js`, we have the logic for a simple little counter. Let's pull it into `src/index.js`.

```js
import { initializeCounter } from './counter';

console.log('Hello World!');

document.querySelector('h1').textContent = 'Hello World!';

initializeCounter();
```

This will blow up in _a new and different way_. You should see an error that looks something like this:

> index.js:1 Uncaught SyntaxError: Cannot use import statement outside a module

Luckily, this is an easy fix.

```html
<script type="module" src="/src/index.js"></script>
```

This fixes both issues:

1. Imports work as expected.
2. The DOM is loaded by the time our script runs.

You can read more about `type="module"` [here](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type).

## Resolve Imports

To observe how Vite bundles files in development mode, you can install vite-plugin-inspect. This plugin allows you to inspect and analyze the bundling process of Vite or its plugins in every stage. By integrating this tool, you can gain insights into how Vite handles file bundling and see the specifics of each step in the process.

## Building

You can build the application using either:

1. `npm run build`
2. `npx vite build`

This will create a `dist` folder with the compiled output. The output will look something along these lines:

```
dist
├── android-chrome-192x192.png
├── android-chrome-512x512.png
├── apple-touch-icon.png
├── assets
│   └── index-03e7ded5.js
├── favicon-16x16.png
├── favicon-32x32.png
├── favicon.ico
├── index.html
├── logo192.png
├── logo512.png
├── manifest.json
└── robots.txt
```

It's everything in the `dist` directory, our `index.html`, and the compiled bundle `index-03e7ded5.js` in an `assets` directory. In this case, it does some basic inlining and minification. There is nothing particularly special to see here. The most important thing to notice is how Vite bundles the code in a format that the browser can efficiently understand.
