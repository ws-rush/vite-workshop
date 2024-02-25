---
title: Multi Page App
---

# Multi Page App

By default, Vite utilizes only `index.html` as the app entry point. While using multiple HTML files works in development, it may not function correctly in the build. To address this, it's necessary to register each HTML file as an entry point in the configuration under `build.rollupOptions.input`.

```js
{
  main: resolve(__dirname, 'index.html'),
  login: resolve(__dirname, 'login/index.html'),
}
```

## Integration with Backend Frameworks

In certain scenarios, Vite might be used as a replacement for a framework's template engine. In such cases, configuring a Multi-Page Application (MPA) is essential. Additionally, setting `manifest: true` is crucial. You can refer to the [django-vite](https://gitlab.com/ws-rush/django-vite) integration example for a practical demonstration.

> Setting `manifest: true` will export a file in the dist directory containing the dist map, which is used to retrieve file names after the build process.
