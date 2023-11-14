---
title: Import Web Workers
---

# Web Workers

## Import with Constructors

A web worker script can be imported using new Worker() and new SharedWorker(). Compared to the worker suffixes, this syntax leans closer to the standards and is the recommended way to create workers.

```ts
const worker = new Worker(new URL('./worker.js', import.meta.url))
```

The worker constructor also accepts options, which can be used to create "module" workers:

```ts
const worker = new Worker(new URL('./worker.js', import.meta.url), {
  type: 'module',
})
```

## Import with Query Suffixes

A web worker script can be directly imported by appending ?worker or ?sharedworker to the import request. The default export will be a custom worker constructor:

```js
import MyWorker from './worker?worker'

const worker = new MyWorker()
```

The worker script can also use ESM import statements instead of importScripts(). Note: During dev this relies on browser native support, but for the production build it is compiled away.

By default, the worker script will be emitted as a separate chunk in the production build. If you wish to inline the worker as base64 strings, add the inline query:

```js
import MyWorker from './worker?worker&inline'
```

If you wish to retrieve the worker as a URL, add the url query:

```js
import MyWorker from './worker?worker&url'
```

See [Worker Options](https://vitejs.dev/config/worker-options.html) for details on configuring the bundling of all workers.