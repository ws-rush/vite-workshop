---
title: Hot Module Replacement
---

# Hot Module Replacement

Out of the box, Vite supports hot module replacement. This means that if you edit a file. Only that file will be replaced and the rest of the application will continue remain. This allows Vite to refresh the page quickly and maintain the state between reloads.

Most of the time, when we're using a framework, we don't have to think about it and we'll get this for free. But, if we're doing something that has side effects, we may want to clean up after ourselves.

`handleHotUpate(context)` hook will be called anytime we are going modify any module that we are currently importing in our js files

```js
// in vite plugin
const plugin = {
	transform(src, id) {
		if (/\.csv$/.test(id)) {
			const records = parse(src, { columns: true });
			return {
				code: `export default ${JSON.stringify(records)}`
			};
		}
	},
	async handleHotUpdate(context) {
		if (/\.csv$/.test(context.file)) {
			context.server.ws.send({
				type: 'custom',
				event: 'csv-update',
				// send the file content as data
				data: {
					url: context.modules[0].url,
					data: parse(await context.read(), { columns: true })
				}
			});
		}

		return [];
	}
};
```

```js
// in js code
import products from './products.csv';

document.querySelector('pre').textContent = JSON.stringify(products);

if (import.meta.hot) {
	// see vite actions here: https://vitejs.dev/guide/api-hmr#hot-on-event-cb
	import.meta.hot.on('csv-update', ({ data, url }) => {
		console.log(`[vite] hot updated: ${url}`);
		document.querySelector('pre').textContent = JSON.stringify(products);
	});
}
```

## Client-Server Communication

```js
// in vite plugin
const plugin = {
	configureServer(server) {
		// when connection start
		server.ws.on('connection', () => {
			server.ws.send('connected', 'Connection Established')
		})

		server.ws.on('ping'. (message. client) => {
			console.log(message)

			client.send('pong', 'Hello Client!!')
		})
	}
}
```

```js
// in js code

if (import.meta.hot) {
	import.meta.hot.on('connected', (message) => {
		console.log(message);

		import.meta.hot.send('ping', 'Hello Server!!');
	});

	import.meta.hot.on('pong', (message) => {
		console.log(message);
	});
}
```

## Accepting hot updates

When editing a file or module, Vite triggers a full update by default, potentially leading to a loss of context in the browser. In the following example, the module is configured to accept Hot Module Replacement (HMR) for itself and its direct dependencies. It also ensures that side effects, such as styles, are properly cleaned up upon module updates.

```js
import './submodule';

export const name = 'parent module';

export default 'default parent';

console.log('parent module');

let styles;

// Side effect: Adding a stylesheet
function addStylesheet() {
	styles = document.createElement('style');
	styles.innerHTML = 'body { backgroundColor: indiago; color: white; }';
	document.head.appendChild(styles);
}
addStylesheet();

// Function to remove the added stylesheet
function removeStylesheet() {
	styles.remove();
}

// Hot Module Replacement (HMR) configuration
if (import.meta.hot) {
	// Without this, every module edit triggers a full clean, resulting in only one log statement
	import.meta.hot.accept((updatedModule) => {
		console.log(updatedModule);
	});

	// Accepting HMR for specified dependencies, preventing unnecessary repetitions
	import.meta.hot.accept(['./submodule.js'], ([newModule]) => {
		console.log(newModule);
	});

	// Ensuring proper cleanup of side effects, like styles, on module disposal
	import.meta.hot.dispose(() => {
		removeStylesheet();
	});
}
```
