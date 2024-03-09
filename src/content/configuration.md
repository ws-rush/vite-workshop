---
title: Vite Configuration
---

# Vite Configuration

Configuring Vite involves tweaking settings in a `vite.config.js` file at the root of your project. This configuration file allows you to control various aspects of your development and production build processes.

## Basic Structure

Here's a basic `vite.config.js` to start:

```js
// vite.config.js
import VitePlugin from 'some-vite-plugin';

export default {
	plugins: [VitePlugin()],
	server: {
		port: 8080
	},
	build: {
		outDir: 'dist'
	}
};
```

or dynamic config:

```js
import { defineConfig, loadEnv } from 'vite';

// command -> [build, serve, ....], mode -> [development, production], ssrBuild
export default defineConfig(({ command, mode, ssrBuild }) => {
	// also we can load enviroment variables: mode -> which mode variables, cwd -> current working directory, '' -> vriables that which this string
	const env = loadEnv(mode, process.cwd(), '');

	// return configs, it can be conditional, like diffrent config for diffrent modes
	return {};
});
```

## Important Fields

### `base`

Defines the base public path when served in development or production. Useful for deploying to subdirectories.

Set serve url in dev mode, if you integrate with backend project make `base` as static files of backend framework also may change `base` for deployment reasons

> notice: used absolute assets like src="/vite" will not work with new base, src='vite' will work with any

```js
export default {
	base: '/subdirectory/' // default is `/`
};
```

### `root`

Specifies the root directory for a project. It defaults to the location of the `vite.config.js` file.

```js
export default {
	root: './src'
};
```

### `publicDir`

Specifies the folder to serve as the root for the dev server and to be copied to the root of the `dist` directory during the build.

```js
export default {
	publicDir: 'public'
};
```

### `plugins`

An array of Vite plugins to be used. Plugins can affect how Vite bundles your code, handles assets, or even injects new features.

```js
import ViteReact from '@vitejs/plugin-react';
import ViteVue from '@vitejs/plugin-vue';

export default {
	plugins: [ViteReact(), ViteVue()]
};
```

### `server`

Configuration options for the development server, such as `host`, `port`, `strictPort`, and `proxy`.

```js
export default {
	server: {
		port: 8080,
		proxy: {
			'/api': 'http://localhost:3001'
		}
		// more options
	}
};
```

### `build`

Contains options for the build process, such as `rollupOptions`, `target`, `outDir`, and more. For example, you can control how CSS code-splitting works:

```js
export default {
	build: {
		outDir: 'dist', // specify build directory, can be'static/' for bakends like django
		manifest: true, // generate manifest.json in build dir, to easier mapping for pwa and backend integration
		target: 'es2022', // support newer js versions (not recommanded)
		cssCodeSplit: true
		// more options
	}
};
```

### `build.rollupOptions`

Set entrypoints for components, can be single entrypoint for entire app default is index.html and index.js in frontend if this option doesnt set when make more than one entrypoint we enable chuncks it is for more than one html file for all project

```js
export default {
	build: {
		rollupOptions: {
			input: {
				main: '/src/main.jsx',
				warning: '/src/warning.jsx',
				refrsh: 'refresh.js'
			}
		}
	}
};
```

### `optimizeDeps`

Used for handling dependencies that need special treatment for optimization. For instance, to pre-bundle certain dependencies:

```js
export default {
	optimizeDeps: {
		include: ['lodash']
	}
};
```

### `css`

CSS-related options, such as modules and preprocessor options, and PostCSS plugins, can be configured here.

```js
export default {
	css: {
		preprocessorOptions: {
			scss: {
				additionalData: '@import "src/variables.scss";'
			}
		},
		postcss: {
			plugins: [require('autoprefixer')]
		},
		devSourcemap: true // enable source map for css, scss, ...
	}
};
```

### `resolve`

Controls how module requests are resolved. You can define custom aliasing, extensions, and other options here.

for aliasing add `{ "baseUrl": ".", "paths": { "@/*": ["src/*"] } }` too `tsconfig`.

```js
export default {
	resolve: {
		alias: {
			'@': '/src'
		}
	}
};
```

### `envPrefix`

```js
export default {
	envPrefix: 'APP_' // default is VITE_,=
};
```

### `envDir`

`direnv` mean `.env` files should be in directory `./direnv`

```js
export default {
	envDir: 'direnv' // default is `.`
};
```

### `define`

Used to replace variables in code during bundling. See following example:

```js
export default {
	define: {
		'process.env.NODE_ENV': 'production'
	}
};
```

### `clearScreen`

Disable clear screen with every reboot.

```js
export default {
	clearScreen: true
};
```

### `logLevel`

Specify which log should output, `info`, `warn`, `error`, `silent`.

```js
export default {
	logLevel: 'silent' // default is `info`
};
```

### `jsx`

Controls JSX transformation, allowing you to specify the JSX factory, JSX fragment, or even disable JSX transformation.

```js
export default {
	jsx: {
		factory: 'h',
		fragment: 'Fragment'
	}
};
```

### `ssr`

Controls Server-Side Rendering (SSR) options, letting you specify an external file as the entry point for your server code and other settings.

```js
export default {
	ssr: {
		external: ['some-external-package'],
		noExternal: ['some-local-package']
	}
};
```

### Modes and Environment Variables

Vite supports environment variables and different modes (`development`, `production`, etc.). You can also create `.env` files that Vite will load depending on the mode.

### Extend with Plugins

Vite's ecosystem allows extending its functionality by using plugins, which can be essential for handling specific tasks like image optimization, linting, or even integrating with other build tools.

### TypeScript Support

Vite has first-class TypeScript support, and you can even write your `vite.config.js` as a TypeScript file (`vite.config.ts`).

### API Configurations

Vite also exposes a JavaScript API for more advanced configurations. This allows you to, for example, programmatically start the Vite development server with custom settings.

```js
// script.js
import { createServer } from 'vite';

async function startServer() {
	const vite = await createServer({
		// custom config
	});
	await vite.listen();
}
```
