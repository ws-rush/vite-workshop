---
title: Vite Plugins
---

# Vite Plugins

Vite has garnered a lot of attention due to its speed and simplicity. One of the key features that make Vite a powerful tool is its plugin system. While there are many community-contributed plugins available (see [aweasome-vite](https://github.com/vitejs/awesome-vite), [aweasome-rollup](https://github.com/rollup/awesome), and [rollip-vite plugins](https://vite-rollup-plugins.patak.dev/)), there may be times you need something more specific for your project. This tutorial will guide you through the process of writing your own custom Vite plugin.

## Understanding Vite Plugins

- Vite plugins are objects or factory functions that adhere to the Rollup Plugin Interface, with some additional Vite-specific hooks.
- Plugins in Vite can perform tasks like modifying initial configuration, transforming imports, adding custom assets, and more.

## Setting Up a Vite Project

1. Install Vite if you haven't already:

```
npm install -g create-vite
```

2. Create a new Vite project:

```
npm init vite@latest my-vite-project --template react
```

3. Navigate to the project directory:

```
cd my-vite-project
```

# Initial Plugin Structure

A basic Vite plugin can be an object with properties that define its functionality. It can also be a factory function returning such an object, allowing for configuration options.

Example:

```js
export default {
	name: 'my-vite-plugin',
	apply: 'build', // Specify when the plugin should be run (either 'serve' for dev server, 'build' for builds, (config: { command, mode }) => boolean for spesfic condations, or omit to always run)
	configResolved(config) {
		// Maybe we need get user config. This is called once the config is resolved.
		// Run before transform hook, so we can use it to access config and store them for other hooks.
	},
	load(id) {
		// Override the load phase for specific modules.
	},
	transform(code, id) {
		// Transform a given code snippet.
	},
	transformIndexHtml(html) {
		// Manuplate or generate index html file
		return html.replace(/<\/body>/, `<script>alert('hello!')</script></body>`);
	}
};
```

There are some other hooks, but that's enough to get us started.

# Adding Your Plugin to the Vite Configuration

Add your plugin in `vite.config.js`:

```js
// vite.config.js
import myPlugin from './my-plugin.js';

export default {
	plugins: [
		myPlugin({
			/* options */
		})
	]
};
```

# Vite Lifecycle Hooks

- **Universal Hooks**: Common Rollup hooks that work in Vite too, like `transform`, `resolveId`, `load`, etc.
- **Vite Specific Hooks**: Unique to Vite like `configResolved`, `configureServer`, etc.

## Example of Universal Hook

```js
{
  name: 'universal-hook-example',
  load(id) {
    // Hook logic
  }
}
```

## Example of Vite Specific Hook

```js
{
  name: 'vite-specific-hook-example',
  configResolved(config) {
    // Hook logic
  }
}
```
